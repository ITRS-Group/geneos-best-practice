# Geneos Best Practice - Configuration - Attributes

Geneos Attributes are used to annotate Manage Entities with information and are used to categorise, group and filter those Entities quickly and efficiently. Setting and using Attributes should always be preferred over using wildcard matching of Managed Entity names. Ensuring their consistent use across an organisation's monitored estate is crucial to making the best use of Geneos.

## Uses

Attributes have a number of uses:

1. View Path in Active Console

   The View Path in the Active Console allows the user to build their own visualisation hierarchy of their estate. Attribute names are used to identify the levels and their order, while the values are the names of the nodes that appear in the State Tree. Because the Active Console can connect to multiple Gateways at once, the use of the View-path becomes the principal way to merge the Managed Entities from all of the Gateways in a meaningful way.

1. Filtering of XPath Targets

   Geneos uses XPaths extensively to select data from the live directory of monitored data.

   * Rules
   * Commands
   * Permissions
   * Dashboards

1. Values passed to Actions and Effects as environment variables

   When the Gateway runs an Action from a Rule, or an Effect as the result of an Alert, it passes a set of values that contain information about the data item that triggered the Action/Effect, various internal parameters and also all the Attributes that are set on the Managed Entity. For an external executable these are set as OS environment variables and for a shared library they are passed as `name=value` pairs are arguments to the function being called. The Attributes can be used to classify the Action/Effect, to present to a user in the form of a list of values and more.

   >[!NOTE]
   >This use is the reason we strongly recommend not prefixing Attribute names with an underscore, as all the other values passed to the Action/Effect have an underscore prefix.

## General

Attributes are name/value pairs and only apply to Managed Entities and not to any other level of the Geneos directory.

Both the name and value of an Attribute are case-sensitive, so `EXAMPLE`, `Example` and `example` are all different.

We recommend that the names of Attributes should be all CAPITALS and make minimal use of non-alphanumeric characters, even if a wider character set is allowed by the software. For Attribute names avoid using spaces and punctuation like dashes, underscores, percentage signs and dots.

The values of each Attribute can be more general but should be consistent across your estate, e.g. avoid mixing case like `London` and `LONDON` or using different word separators, e.g. `Data Center 1` and `Data-Center-1`. The values should have a clear meaning and this must be enforced though a policy of consistent definition and use.

With the exception of Dynamic Mappings, the value of an Attribute is fixed and cannot include variables. This makes sense if you consider it is at the Managed Entity level that User Variables are resolved to their final values before being used by configuration items referenced by the Managed Entity, e.g. within Samplers.

## Sources

Attributes can be set in two areas:

* Gateway Configuration

  * Managed Entities
  * Managed Entity Groups

* Dynamic Entities

  * Mappings

## Attribute Categories

While Attributes themselves are simple key/value pairs, it is best to think of them addressing specific use categories.

We recommend using Attributes that fall into 4 categories:

1. Location (Physical or Logical)
1. Technology
1. Organisational
1. Miscellaneous

### Location

Location Attributes represent a way of referring to the monitored elements that are contained in a Managed Entity. In more traditional on-premises deployments these could include a data-centre, city, country, region etc. In cloud and orchestrated environments this may be the cloud region or availability zone, the tenancy, cluster or virtual machine instance and so on.

1. General

    | Attribute  | Alternatives | Examples               | Description                                                            |
    | ---------- | ------------ | ---------------------- | ---------------------------------------------------------------------- |
    | `LOCATION` |              | `Core A`               | A general purpose indication of the location of the monitored resource |
    | `REGION`   |              | `EMEA`, `us-central-1` | This can be a geographical area or a cloud region.                     |

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
    | `PLATFORM`   |                                | `AWS`, `Azure`, `Rackspace` | This can also be a technology Attribute |
    | `AWS-REGION` | `AZURE-REGION`, `CLOUD-REGION` | `eu-west-2`                 |                                         |
    | `ZONE`       |                                |                             | Availability Zone                       |
    | `VPC`        |                                |                             |                                         |
    | `CLUSTER`    |                                | `appCluster1`               |                                         |
    | `NAMESPACE`  |                                |                             |                                         |
    | `NODE`       |                                |                             |                                         |
    | `ESTATE`     |                                |                             |                                         |

### Technology

Attributes that represent the technology being monitored are less likely to be hierarchical and more represent a matrix of properties of the monitored elements.

| Attribute     | Alternatives | Examples | Description |
| ------------- | ------------ | -------- | ----------- |
| `PLATFORM`    |              |          |             |
| `APPLICATION` |              |          |             |
| `COMPONENT`   |              |          |             |
| `CATEGORY`    |              |          |             |
| `OS`          |              |          |             |
| `DATABASE`    |              |          |             |

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

    | Attribute      | Alternatives       | Examples | Description |
    | -------------- | ------------------ | -------- | ----------- |
    | `OWNER`        | `CONTACT`, `EMAIL` |          |             |
    | `CLIENT`       | `CUSTOMER`         |          |             |
    | `USER`         |                    |          |             |
    | `ORGANISATION` | `COMPANY`          |          |             |
    | `VENDOR`       |                    |          |             |

### Miscellaneous

1. Functional Attributes

    | Attribute  | Alternatives | Examples | Description |
    | ---------- | ------------ | -------- | ----------- |
    | `INCIDENT` |              | `True`   |             |
    | `SLA`      |              | `2h`     |             |

