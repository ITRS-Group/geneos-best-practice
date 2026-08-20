# Geneos Best Practices - Include Files

(Align with attributes)

Types:

* Functional
  * Read-only
  * Upgradeable
  * Infra, Basic App, Geneos self-monitoring
  * Specific tech solutions
  * Naming: ORG.LEVEL.FUNCTION.xml
  * Located in `../../includes/...`

* Shared configuration
  * Editable
  * Shared, reloadable
  * Should match base includes (like above) one-for-one
  * Located in `../../shared/...`

* Local configuration
  * Editable
  * Separate file(s) for auth access and partitioning functionality
  * Located in gateway working directory

* Templates
  * Examples
  * To be copied before editing
  * Some automation to generate
  * Located in other repo / directory, not directly loaded by gateways

* Metadata
  * Read-only
  * Generated, reloadable
  * Located in gateway working directory / URL

...

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


