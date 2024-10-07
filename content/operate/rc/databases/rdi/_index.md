---
Title: Data Integration
alwaysopen: false
categories:
- docs
- operate
- rc
description: Use Redis Data Integration with Redis Cloud.
hideListLinks: true
weight: 99
---

Redis Cloud now supports [Redis Data Integration (RDI)]({{<relref "integrate/redis-data-integration">}}), a fast and simple way to bring your data into Redis from other types of primary databases.

A relational database usually handles queries much more slowly than a Redis database. If your application uses a relational database and makes many more reads than writes (which is the typical case) then you can improve performance by using Redis as a cache to handle the read queries quickly. Redis Cloud uses [ingest]({{<relref "/integrate/redis-data-integration/">}}) to help you offload all read queries from the application database to Redis automatically.

Using a data pipeline lets you have a cache that is always ready for queries. RDI Data pipelines ensure that any changes made to your primary database are captured in your Redis cache within a few seconds, preventing cache misses and stale data within the cache. 

RDI helps Redis customers sync Redis Cloud with live data from their primary databases to:
- Meet the required speed and scale of read queries and provide an excellent and predictable user experience.
- Save resources and time when building pipelines and coding data transformations.
- Reduce the total cost of ownership by saving money on expensive database read replicas.

Using RDI with Redis Cloud simplifies managing your data integration pipeline. No need to worry about hardware or underlying infrastructure, as Redis Cloud manages that for you. Creating the data flow from source to target is much easier, and there are validations in place to reduce errors.

## Data pipeline architecture

A RDI data pipeline sits between your source database and your target Redis database. Initially, the pipeline reads all of the data and imports it into the target database during the *initial cache loading* phase. After this initial sync is complete, the data pipeline enters the *change streaming* phase, where changes are captured as they happen. Changes in the source database are added to the target within a few seconds of capture.

For more info on how RDI works, see [RDI Architecture]({{<relref "/integrate/redis-data-integration/architecture">}}).

### Pipeline security

Data pipelines are set up to ensure a high level of data security. Source database credentials and TLS secrets are stored in AWS secret manager and shared using the AWS Secret Manager CSI driver for secrets. See [Share source database credentials]({{<relref "/operate/rc/databases/rdi/setup#share-source-database-credentials">}}) to learn how to share your source database credentials with Redis Cloud.

Connections to the source database are through [AWS PrivateLink](https://aws.amazon.com/privatelink/), ensuring that the data pipeline is only exposed to the specific database endpoint. See [Set up connectivity]({{<relref "/operate/rc/databases/rdi/setup#set-up-connectivity">}}) to learn how to connect your PrivateLink to the Redis Cloud VPC.

RDI encrypts all network connections with TLS. The pipeline will process data from the source database in-memory and write it to the target database using a TLS connection. There are no external connections to your data pipeline except from Redis Cloud management services.

## Prerequisites

Before you can create a data pipeline, you must have:

- A [Redis Cloud Pro database]({{< relref "/operate/rc/databases/create-database/create-pro-database-new" >}}) hosted on Amazon Web Services (AWS)
- One supported source database, also hosted on AWS and connected to [AWS PrivateLink](https://aws.amazon.com/privatelink/):
    - MySQL
    - Oracle
    - SQL Server
    - PostgreSQL

{{< note >}}
Please be aware of the following limitations:

- The target database must be a Redis Cloud Pro database hosted on Amazon Web Services (AWS). Redis Cloud Essentials databases and databases hosted on Google Cloud do not support Data Integration.
- Only the Ingest use case is supported at this time.
- Source databases must also be hosted on AWS.
- One source database can only be connected to one target database.
{{< /note >}} 

## Get started

To create a new data pipeline, you need to:

1. [Prepare your source database]({{<relref "/operate/rc/databases/rdi/setup">}}) and any associated credentials.
2. [Create data pipeline]({{<relref "/operate/rc/databases/rdi/create">}}) and troubleshoot errors.
3. [Define the data pipeline]({{<relref "/operate/rc/databases/rdi/define">}}) by selecting which tables to sync.

Once your data pipeline is defined, you can [view and edit]({{<relref "/operate/rc/databases/rdi/view-edit">}}) it.