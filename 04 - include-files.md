# Geneos Best Practices - Gateway Configuration - Include Files

Gateway include files should be used in all but the most trivial Geneos set-ups.

Include files allow the partitioning of gateway configurations into a prioritised and mergeable form which can then be shared and synchronised across multiple gateways. This not only saves time in the long run, it also ensures the configurations are consistent and easier to maintain.

In this document we will describe best practices for Geneos include files, including how to use them, how to structure them, and how to share them across multiple gateways.

## How Include Files Work

While we don't intend to duplicate the Geneos documentation, it is worth briefly establishing how some of the key features of include files work, especially around the process of merging configurations.

### Priorities

To reemphasise the documentation, every include file must, and the main setup file, have a unique priority value. The numeric priority is lower-number, higher-priority and the lowest value is 1. The main setup file has the default priority of 1, but this can be changed, and each include you add must have it's own value. The priority value can, in theory, be different for different gateways but this is not common and we would advise against this. In some circumstances the priority could be encoded in the file name, but the value still must be set in the configuration.

```
Main `gateway.setup.xml`, priority 1 (highest)
- 📄 Include file `include-example1.200.xml`, priority 200
- 📄 Include file `include-example2.300.xml`, priority 300
```

>[!TIP] Best Practice:
>Use a consistent priority scheme across all gateways, and use the same priority for the same include file across all gateways. This will make it easier to manage and maintain your configurations.

### Merging and then Validating

The gateway first merges the main setup file and all the enabled include files based on priority and then loads, validates and applies the resulting configuration. Understanding the order of this process is important because if some configuration items have the same name, e.g. samplers, but have been created in different files and in a different folder hierarchy they will generate errors about conflicting names during validation. This is because the merge process does not check the validity of items, it simply merges the files based on priority.

### Merging at the Configuration Item Level

The merging process also works at the level of configuration item folders. For example, if a _Managed Entity Group_ is defined in two include files (or one include and the main setup file) the resulting configuration will be the sum of the two settings with conflicting parts resolved based on priority - so, for example, if one definition included attributes `A` and `B` and the other had `B` and `C`, the resulting group will have attributes `A`, `B` and `C`. Where both define attribute `B` with different values, the value from the include file with the higher priority will be used. For example:

```
Main `gateway.setup.xml`, priority 1 (highest)
- 📁 Example Group: Attributes: A=1, B=2
  - 🟦 Entity: Attributes: C=3

Include file `include.xml`, priority 2
- 📁 Example Group: Attributes: B=3, D=4
```

After merging, the resulting `Entity` will have attributes `A=1, B=2, C=3, D=4` - `B` retains the value from the higher priority main setup file, while `D` is added from the lower priority include file. `C` comes from the entity itself.

This does not apply to non-group/non-folder configuration items, such as samplers, which are not merged but instead the higher priority definition is used and the lower priority definition is ignored.

This feature is very useful for creating a read-only base configuration in one include file and then allowing overriding of specific settings in another (higher priority) file, but it can also lead to unexpected results if the merging process is not well understood. We will use this later in this guide.

### Merging is not inheritance

The process of merging include files, and the content of configuration groups above, is unrelated to the process of inheritance in the configuration. Merging happens first and then specific configuration items located in a hierarchy of groups will inherit settings from their parent groups. This is a separate process and should not be confused with merging.

## Categories of Include Files

There are five kinds of include files:

1. Functional - Redistributable common functionality, read-only, versioned and upgradeable
2. Shared configuration - Common settings for your organisation spanning multiple gateways, editable but should be versioned
3. Local configuration - Custom settings for a single gateway or gateway server, editable
4. Templates - Examples and starting points for new functionality, read-only and copyable
5. Metadata - Normally externally generated information about the configuration and structure of your Geneos environment, read-only

The main gateway setup file, which loads the includes, doesn't fall directly into any of these categories.

### Functional

A functional include file is designed to encapsulate specific monitoring functionality and can be shared across multiple Gateways. These include files are typically read-only and upgradeable, allowing for consistent monitoring across your environment.

* Read-only
* Upgradeable
* Infra, Basic App, Geneos self-monitoring
* Specific tech solutions
* Naming: ORG.LEVEL.PURPOSE.xml
* Located in `../../includes/...`

### Shared configuration

Shared configuration include files are intended to provide common settings and configurations that can be used across multiple Gateways. These files are editable and reloadable, allowing for easy updates and changes.

* Editable
* Shared, reloadable
* Should match base includes (like above) one-for-one
* Located in `../../shared/...`

### Local configuration

Local configuration include files are specific to a single Gateway and are used to provide custom settings and configurations that are unique to that Gateway. These files are editable and can be reloaded as needed.

* Editable
* Separate file(s) for auth access and partitioning functionality
* Located in gateway working directory

### Templates

Template include files are intended to provide examples and starting points for creating new monitoring functionality. These files are typically read-only and can be copied and modified to create new include files.

* Examples
* To be copied before editing
* Some automation to generate
* Located in other repo / directory, not directly loaded by gateways

### Metadata

Metadata include files provide information about the configuration and structure of your Geneos environment. These files are typically read-only and can be used to document your configuration and provide context for other users.

* Read-only
* Generated, reloadable
* Located in gateway working directory / URL

## Specific Kinds of Include Files

### Authentication


## Using Include Files

Locations:

All paths should be relative where possible to allow moving of configuration files. Or URLs.

Priorities:

Priorities should be grouped by monitoring level (e.g. 1000, 2000), all includes that do NOT override others (e.g. local overrides) should be high end of the group.

Authentication/Access:

Auth settings should protect read-only files from all but Administrators or specific local user types.

Reloading: On but with Active Times advised for production

General Rules:

* Probes and Entities should only appear in main or local include, except virtual probes - but even then beware uniqueness constraints
* Use built-in features, like self-monitoring, where possible and configure in main or local include
* Names that appear in XPaths or Action/Effect env variables should be meaningful (e.g. probes, samplers, type etc.) and consistent
* Variables should in Environments where possible
* Variables should NOT be used to control all settings, just key ones
* All "macro" variables will be predefined in generated includes like instance.setup.xml

Writing Includes / main setup:

* Use (matching) groups to categorise different items, base on Entity hierarchies where appropriate
* Design for overriding or variables but not both
* Use comments to explain set-up, especially for templates
* Sampler Includes should be used for the 4 common plugins to consolidate monitored data, for each level and function
* Don't create lots of small files for the sake of it. Create units of functionality
* Use include merging rules to build empty group hierarchies like Entities that can be used as templates in config includes

Sharing / Publishing

* All functional includes should also create shared datasets with consistent names
* Disable publishing for samplers that aggregate data, e.g. dashboard views

Editing and Hot Standby Sync

* All non-local includes should be edited in only one gateway and this should also be the only gateway that syncs files, unless the sync is done through VCS or URLs

There are three common styles of Geneos Gateway include files:

1. **Monitoring Modules**

    The first is used to encapsulate functionality and to enable specific monitoring functionality. These styles of include file are intended to be re-used and shared across multiple Gateways, potentially including versioning with an release and update process. Include files like this will have  common features:

    1. **Self-contained**
    
        Apart from being used in Managed Entity or Managed Entity Group via a Type or Samplers, these include files should not require set-up beyond domain specific settings. Where possible defaults should exist in the include file itself for as much as the specific functionality allows for.

    2. **Read-only**

        The Geneos administrator using a shared include file should not need to edit the file to use the functionality. In general the include file should be read-only to prevent divergence from the original functions.

2. **Templates**

    Unlike Monitoring Modules above, Template include files are intended for sharing examples of monitoring functionality and to act as starting points for you, the Geneos administrator, to take and customise to your requirements. The result of your changes may then become an include file that meets the requirements for a Monitoring Module and becomes self-contained and read-only.

3. **Functional Segregation**

    The second is to segregate functionality within a single Gateway. This segregation may be for a number of reasons, including:

    1. For access control, to prevent unauthorised users from making changes

    2. For administrative convenience, to make it easier to manage defined monitoring especially in a large environment





## Previous

* Enable/Disable + Variables

* No disabled content
* Defaults for all variables
* Rules fixed priority for overrides
* Virtual probes
* Generic naming

* Suggested Roles
* Alerting to Roles

* Sampler Include Base Samplers
    * logs files
    * processes

* Static Vars
    * FKM Tables
        * Geneos Error / Ignore
        * Geneos-component specifics
        * Java-aware
    * Process Descriptors
        * Geneos-component specifics

### Framework

* Standard Actions
* Priming Namespaces
    * ME Group with Attributes names for load
* Defaults from default
    * Gateway Controlled Active Times
    * Self-monitoring off ?
    * Rule Threads N
* Exported Samplers

### Level 0 ?

* Monitor-the-monitor
* Geneos Performance
    * Processes
    * Logs
    * Load
    * Rules

### Level 1

* Infrastructure
    * Hardware
    * Networks

### Level 2

* Processes
* Files

## Templates

* Copy and change
* Example Includes
    * Complex Plugins
    * Collection Agent
    * Monitoring Containers
    * Gateway-SQL for Dashboards
    * Active Times - Typical Schedules
    * ME Group Hierarchy with Attributes

### Authentication


