# Geneos Best Practice - Configuration - Attributes

Attributes are name/value labels that are used with, and only apply to, Managed Entities ("entities") to label, group and filter those entities quickly and efficiently. Setting and using attributes should _always_ be preferred over using wildcard matching of entity names. Ensuring their consistent use across your organisation's monitored estate is crucial to making the best use of Geneos.

Attributes can be set on Managed Entity Groups and on individual Managed Entities. They can also be set on Dynamic Mapping Groups and Dynamic Mappings, which are used to create Dynamic Entities. Attributes are inherited from the Managed Entity Group to the Managed Entity and from the Dynamic Mapping Groups to Dynamic Mappings and then to each Dynamic Entity created.

## Basic Attributes

To get you going, you should aim to use the attributes below, starting with the primary list and then, as required, the secondary and tertiary ones. A more extensive list of suggested names and their uses follows in the next section.

Don't be tempted to overload an entity with too many attributes, as this can make it difficult to manage and maintain your gateways. Use only those attributes that are relevant to the way you want to label, group and filter your entities. Use Annotations for any additional name/value pairs that are only used for passing data to Actions and Effects and not for grouping and filtering.

>[!IMPORTANT]
>Attribute names and values are case-sensitive and you need to keep this in mind to ensure the consistency required across your growing Geneos estate.
>
>Because attributes can be exported outside of Geneos (see [Uses](#Uses) below) their names should be kept to all **CAPITALS** and make minimal use of non-alphanumeric characters, even if a wider character set is allowed by the Gateway Setup Editor. If you use dashes or underscores in any names then you should only use one or the other for all other cases too. The values of each attribute can be more general but should be consistent across your estate, e.g. avoid mixing case like `London` and `LONDON` or using different word separators, e.g. `Data Center 1` and `Data-Center-1`. The values should have a clear meaning and this must be enforced though a policy of consistent definition and use.

### Primary

* `ENVIRONMENT` - Production / PROD, QA, Development, Test, etc.
* `LOCATION` - Datacentre name, City, Country, Cloud Region, etc.
* `CATEGORY` - Infrastructure, Application, Database, etc.

### Secondary

* `SUBCATEGORY` - Refines the CATEGORY, e.g. Server, Switch etc.
* `APPLICATION` - Name of the application being monitored, e.g. `Geneos`, `Jenkins`, `Oracle`, etc.
* `COMPONENT` - DB Server, Web Server, etc.

### Tertiary

* `OS_FAMILY` - Windows, Linux, Solaris, etc.
* `DASHBOARD` - Dashboard filter name for synthesised views

## Uses


Attributes have a number of uses:

1. State Tree hierarchy in Active Console via the View Path setting

   The View Path in the Active Console allows the user to build their own hierarchy of entities in the State Tree window. Attribute names are used to identify the levels and their order, while the values are the names of the nodes that appear in the State Tree. Because the Active Console can connect to and aggregate the data from multiple gateways at once, the use of the the view path becomes the principal way to merge the entities from all of the connected gateways in a meaningful way.

1. Fast filtering of XPath targets

   Geneos uses XPaths extensively to select data from the live directory of monitored data. You may already have seen these used in the following configuration sections:

   * Rules
   * Commands
   * User and Role Permissions
   * Dashboard modifiers

1. Passed as parameters to Actions and Effects

    When the gateway runs either an action from a rule, or an effect as the result of an alert firing, it passes a set of name/value pairs that contain information about the data item that triggered it, including internal parameters and all the attributes that are set on the entity.

    > NOTE: Another Geneos feature called Annotations means that you do not have to overload entities with extra name/value pairs that are only used for Actions and Effects. Annotations are a separate set of configurable name/value pairs that are only passed to Actions and Effects and not for anything else. Annotations can also be defined at a more granular level than entities.

1. Dimensions passed to ITRS Analytics

   When the Gateway sends data to ITRS Analytics ("IAX"), it passes all attributes as dimensions, allowing the data to be grouped and filtered in IAX by those dimensions.

## Sources

Attributes can be set in a number of places in Geneos:

1. Gateway configuration

    In the main gateway configuration file, attributes can be set on the following elements:

    * Managed Entity Groups
    * Managed Entities

    Gateway configuration file merging and inheritance mean that the set of attributes applied to an entity will depend on the overall configuration of the gateway and the position of the entity in the hierarchy.

    This is the most common way to set attributes and is the recommended approach for most users.
    
    You should create a hierarchy of Managed Entity Groups that reflect your chosen attributes, setting at least one primary attribute on each group and the group name should be the value of that attribute. For example:

    ...

1. Self-Announcing Netprobe configuration

    A Self-Announcing Netprobe ("SAN") can declare its own entities and set attributes on them. This is done in the SAN configuration file, which is read by the Netprobe when it starts up.

1. Dynamic Entities

    Metrics being injegted into Geneos from external sources can be used to create Dynamic Entities. These entities are created by the gateway when it receives the data and they can have attributes set on them in two ways:

    * Dynamic Mapping Groups
    * Dynamic Mappings

    As well as fixed _Local Attributes_ (which can, uniquely, include interpolated strings from variables), mappings that set the entity name also set the same dimension as an attribute on the created entity. In general these created attributes do not follow the advice in this document and are likely to use names in lowercase etc.

## Recommended Taxonomy

While attributes themselves are arbitrary key/value pairs, it is best to think of them as meeting specific needs.

Attributes fall into 4 common categories:

1. Physical or Logical Location
1. Technology
1. Organisational
1. Miscellaneous

### Location

Location attributes represent a way of referring to the physical or logical position of an entity. In more traditional on-premises deployments these could include a data-centre name, city, country, region etc. In cloud and orchestrated environments this may be the cloud region or availability zone, the tenancy, cluster or virtual machine instance and so on.

1. General

    | Attribute  | Alternatives | Examples               | Description                                                |
    | ---------- | ------------ | ---------------------- | ---------------------------------------------------------- |
    | `LOCATION` |              | `Core A`               | A general purpose indication of the location of the entity |
    | `REGION`   |              | `EMEA`, `us-central-1` | This can be a geographical area or a cloud region.         |

1. Physical Location Attributes

    | Attribute    | Alternatives       | Examples   | Description |
    | ------------ | ------------------ | ---------- | ----------- |
    | `COUNTRY`    |                    | `France`   |             |
    | `STATE`      |                    | `Illinois` |             |
    | `CITY`       | `TOWN`             | `London`   |             |
    | `DATACENTER` | `DATACENTRE`, `DC` | `Alpha`    |             |

1. Logical Location Attributes

    | Attribute    | Alternatives                   | Examples                    | Description                             |
    | ------------ | ------------------------------ | --------------------------- | --------------------------------------- |
    | `PLATFORM`   |                                | `AWS`, `Azure`, `Rackspace` | This can also be a technology attribute |
    | `AWS-REGION` | `AZURE-REGION`, `CLOUD-REGION` | `eu-west-2`                 |                                         |
    | `ZONE`       |                                |                             | Availability Zone                       |
    | `VPC`        |                                |                             |                                         |
    | `CLUSTER`    |                                | `appCluster1`               |                                         |
    | `NAMESPACE`  |                                | `kotsadm`, `cert-manager`   |                                         |
    | `NODE`       |                                |                             |                                         |
    | `ESTATE`     |                                |                             |                                         |

### Technology

Attributes that represent the technology being monitored are less likely to be hierarchical and more represent a matrix of properties of the monitored elements.

| Attribute     | Alternatives | Examples                                       | Description                                       |
| ------------- | ------------ | ---------------------------------------------- | ------------------------------------------------- |
| `PLATFORM`    |              |                                                | This can also be used as a logical location label |
| `CATEGORY`    |              | `Infrastructure`, `Market Data`, `Application` |                                                   |
| `SUBCATEGORY` |              | `Publisher`, `Router`                          |                                                   |
| `APPLICATION` |              | `Superduper`                                   | The name of the application being monitored       |
| `COMPONENT`   |              | `DB`, `Web Server`                             | The component of the application                  |
| `OS`          | `OS_FAMILY`  | `Linux`, `Windows`                             |                                                   |
| `DATABASE`    |              | `PostgreSQL`, `SQLServer`                      |                                                   |

### Organisational

1. General

    | Attribute     | Alternatives       | Examples                    | Description |
    | ------------- | ------------------ | --------------------------- | ----------- |
    | `ENVIRONMENT` |                    | `PROD`, `Production`, `DEV` |             |
    | `CMDB-ID`     | `APP-ID`, `SERIAL` |                             |             |

1. Purpose

    | Attribute       | Alternatives       | Examples | Description |
    | --------------- | ------------------ | -------- | ----------- |
    | `LOB`           | `DIVISION`         |          |             |
    | `BUSINESS-UNIT` |                    |          |             |
    | `DEPARTMENT`    |                    |          |             |
    | `TEAM`          |                    |          |             |

1. Owner / User

    | Attribute      | Alternatives       | Examples | Description                                                  |
    | -------------- | ------------------ | -------- | ------------------------------------------------------------ |
    | `ORGANISATION` | `COMPANY`          |          |                                                              |
    | `VENDOR`       |                    |          | Vendor managing the entity or host the entity is deployed on |
    | `OWNER`        | `CONTACT`, `EMAIL` |          | Role-based contact for entity being monitored                |
    | `CLIENT`       | `CUSTOMER`         |          |                                                              |
    | `USER`         |                    |          |                                                              |

### Miscellaneous

1. Functional Attributes

    | Attribute  | Alternatives | Examples | Description                                                           |
    | ---------- | ------------ | -------- | --------------------------------------------------------------------- |
    | `INCIDENT` |              | `True`   | Should an incident be created if a critical is raised on this entity? |
    | `SLA`      |              | `2h`     |                                                                       |

