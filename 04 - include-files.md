# Geneos Best Practices - Include Files

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


## Read-Only

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


