# Geneos Best Practice - Configuration

## Introduction

### ITRS Monitoring Maturity Model

You may have seen references to the ITRS Monitoring Maturity Model. This layered approach looks at the "type and use" of the monitoring in your organisation. These Best Practice guidelines concentrate on Levels 1 and 2 and we introduce a supplementary level 0 related to the Geneos platform itself.

* Level 0 - Geneos Self-Monitoring
* Level 1 - Infrastructure
* Level 2 - Basic Application
* Level 3 - Advanced Application
* Level 4 - End-to-End
* Level 5 - Business Activity


## Standard Include Files



### Read-Only

### Templates

### Defaults and Overrides


## Suggested Conventions

### Naming

Your organisation may already have internal standards on naming components of your IT estate. Follow those where possible to ensure that Geneos integrates cleanly and any alerts and other interactions have meaning outside your immediate team. You may have a Configuration Management Database (CMDB) that drives this.

Things in Geneos that should have clear, sensible and (generally) unique names:

* Probes

    Each Netprobe is normally identified using a combination of the hostname and either it's function or the port it listens on. Normally probe names are not visible to end-users.

* Managed Entities

    Managed Entities form the core collection object for Geneos monitoring. Each Managed Entity should be uniquely named across your whole monitored estate.

* Types and Samplers

    Types represent related collections of samplers and their names should reflect the overall objective of that monitoring. Sampler names must be unique on a particular Gateway but more than one instance of a Sampler can be used in a Managed Entity as long as you use Types to distinguish them. This is however rare.

    Both Types and Samplers are often shared between Gateways through Include Files and because of this their names should be clear and meaningful given the context of the monitoring distributed over those Gateways.

### Grouping and Hierarchies


## Managed Entities

### Attributes

Correctly using and standardising Managed Entity Attributes is critical to the efficiency and utility of your Geneos installation.

Attributes are used extensively in the Processing and Visualisation tiers. They can be used to partition monitoring, to dynamically change the display of the monitored estate to users and much more.

At the basic level attributes are name/value pairs. Both name and value are case-sensitive, so `EXAMPLE`, `Example` and `example` are all considered different.

They can be defined on Managed Entities and on Managed Entity Groups and are inherited using a most-specific-wins basic. Even so, your configuration should be structured in such a way that for any given Managed Entity each application attribute is only defined once in the hierarchy from the Entity to the top level container.

Names are used as selectors while values are typically displayed to users and/or used as match filters.

All attribute names should be all UPPER-CASE and only contain letters and only occasionally a numeric suffix. For example `REGION` or `APPLICATION` or `LAYER1`. Avoid using spaces and the allowed punctuation like dashes, underscores, percentage signs and dots.

Attributes values, with some exceptions, should be a human readable and meaningful string.

* Suggested Attribute Names

 Name | Example Values | Comment
---------|----------|---------
 `APPLICATION` | `TradeFarm` | C1
 `COMPONENT` | `Database`, `Controller` | C2
 `REGION` | `EMEA`, `APAC` | C3
 `LOCATION` | `London`, `Houston` | C3
 `DATACENTER` | `NYC1`, `LNWest` | C3
 `OWNER` | `John Doe` | C3
 `CONTACT` | `l2team@example.com` | C3
 `ENVIRONMENT` | `PROD`, `DEV` | C3


## Include Files

Include files allow you to divide your configuration into reusable parts. 

Classes of include file:

* Remote, read-only
* Local, shared
* Local, gateway specific

### Version Control

XML Comments


## Authentication

SSO

Local admin

Permissions / Roles

Edit config

Run commands

### Netprobe Security

Commands

Passwords

Plugin Whitelists



## Variables

Variables can be use in many set-up fields as an interpolated part of a larger string or, for other fields, as an alternative to a static value.

If you are creating new sampler configurations then don't expose every setting as a variable, but be mindful of how you can re-use your work again later on. Select common settings that may need alternative values, such as the Sample Interval, file paths and Active Times.

Thresholds for generic rules

Variables should almost always be used for encrypted passwords. The environments containing these can then be stored with more restricted permissions.




### Environments

Environments are collections of variables.

Environments can be attached to Types or to a Managed Entity (but not a group) or an instance of a Type on a Managed Entity or Group.


### Using Defaults

Environments inherit values from their parent environments and this can be used to create a structure with default and more specific values with ease.

For example, a "Database" environment, with "MySQL" and "SQLServer" sub-environments, then in each of these further specific values for individual database instances. Changing a value such as a shared hostname can be done in one place.

All Types should have default for individual variables or a default environment.

## Active Times

Active Times allow you control your monitoring across the working day and over wider periods. You can use them to help "monitor by exception" and to reduce noise. Not many people are interested in events during maintenance periods or at weekends, just as long as their systems are back online and functioning for the start of the day.

### When To NOT Use Active Times

* File-content based samplers

    File-content based samplers (such as FKM, Statetracker, FIX Analyser 2 and Message Tracker) should not have Active Times applied at the Sampler level. Where possible Active Times should be applied to plugin-specific settings. When a Sampler is outside it's Active Time is is not turned-off but instead it is suspended - effectively each sampler is skipped - so none of the work of advancing the read position in a file is done. When the sampler is reactivated the file is read from the previous location, and will almost certainly result in activity being processed that shouldn't be.

    For some plugins, like Statetracker, there is no Active Time other than at the Sampler level. In these cases you should focus more on any Rules that are triggered by changes in the data.

### Active Time Variables

Abstract configuration

Set a default

### Holiday Calendars

You should create and maintain holiday calendars for each country or region where you have active monitoring. These can then be incorporated into other Active Times to ensure that you do not trigger alerts when it is unlikely that you would need them. A holiday calendar should be named in a way that makes it clear to other Geneos managers what they are. Typical, unrelated, examples would be:

* `UK Holidays`
* `JP Holidays`

Or, 

* `Holidays - France`
* `Holidays - Malaysia`

It is probably a good idea to put all holiday calendar Active Times in their own group with the name `Holidays`.

A holiday Active Time should normally only contain specific Dates with the Override checkbox selected.

Create yourself (or your team) a calendar entry in your wider organisation's chosen Calendar application to update the holiday Active Times once each quarter or annually.

**Create standard calendar includes**

## Shared Data

### Exported Data Sets



## Database Logging

Table names and types

Event table


