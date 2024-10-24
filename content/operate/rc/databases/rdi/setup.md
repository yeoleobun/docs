---
Title: Prepare source database
alwaysopen: false
categories:
- docs
- operate
- rc
description: Prepare your source database and database credentials for Data integration.
hideListLinks: true
weight: 1
---

## Create new data pipeline

1. In the [Redis Cloud console](https://cloud.redis.io/), go to your target database and select the **Data Pipeline** tab.
1. Select **Create data pipeline**.
1. Select your source database type. The following database types are supported:
    - MySQL
    - Oracle
    - SQL Server
    - PostgreSQL
1. If you know the size of your source database, enter it into the **Source dataset size** field.

## Prepare source database

Before using the pipeline, you must first prepare your source database to use the Debezium connector for change data capture (CDC).

See [Prepare source databases]({{<relref "/integrate/redis-data-integration/data-pipelines/prepare-dbs/">}}) to find steps for your database type.

See the [RDI architecture overview]({{< relref "/integrate/redis-data-integration/architecture#overview" >}}) for more information about CDC.

## Share source database credentials

You need to share your source database credentials and certificates in an Amazon secret with Redis Cloud so that the pipeline can connect to your database.

In the [AWS Management Console](https://console.aws.amazon.com/), use the **Services** menu to locate and select **Security, Identity, and Compliance** > **Secrets Manager**. [Create a secret](https://docs.aws.amazon.com/secretsmanager/latest/userguide/create_secret.html) of type **Other type of secret** with the following key/value fields:

- `username`: Database username
- `password`: Database password
- `server_certificate`: Server certificate in PEM format *(TLS only)*
- `client_certificate`: [X.509 client certificate](https://en.wikipedia.org/wiki/X.509) or chain in PEM format *(mTLS only)*
- `client_certificate_key`: Key for the client certificate or chain in PEM format *(mTLS only)*
- `client_certificate_passphrase`: Passphrase or password for the client certificate or chain in PEM format *(mTLS only)*

{{<note>}}
If your source database has TLS or mTLS enabled, we recommend that you enter the `server_certificate`, `client_certificate`, and `client_certificate_key` into the secret editor using the **Key/Value** input method instead of the **JSON** input method. Pasting directly into the JSON editor may cause an error. 
{{</note>}}

After you store this secret, you can view and copy the [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/secretsmanager/latest/userguide/reference_iam-permissions.html#iam-resources) of your secret on the secret details page. 

## Set up connectivity

To expose your source database to Redis, you need to add Redis Cloud as an Allowed Principal on the [AWS PrivateLink VPC permissions](https://docs.aws.amazon.com/vpc/latest/privatelink/configure-endpoint-service.html#add-remove-permissions) for the PrivateLink connected to your source database.

1. Copy the Amazon Resource Name (ARN) provided in the **Setup connectivity** section.
1. Open the [Amazon VPC console](https://console.aws.amazon.com/vpc/) and select **Endpoint services**.
1. Navigate to **Allow principals** tab.
1. Add the Redis Cloud ARN and choose **Allow principals**.
1. Copy your PrivateLink service name for later.

For more details on AWS PrivateLink, see [Share your services through AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/privatelink-share-your-services.html).


## Next steps

After you have set up your source database and prepared connectivity and credentials, select **Start pipeline setup** to [provision data pipeline infrastructure]({{<relref "/operate/rc/databases/rdi/provision">}}).