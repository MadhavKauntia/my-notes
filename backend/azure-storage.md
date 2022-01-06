# Azure Storage

## Blob Storage

A massively scalable object store for text and binary data.
Idea for:

- Serving images or documents directly to a browser
- Storing files for distributed access
- Streaming video and audio
- Storing data for backup and restore, disaster recovery and archiving
- Storing data for analysis by an on-premises or Azure-hosted service

![Azure Blob Storage](https://cloud.telestream.net/images/tutorials/azure-structure-679c37b5.png)

### Blob Storage Access Tier

- **Hot** - optimized for frequent access of objects
- **Cool** - optimized for storing large amounts of data that is infrequently accessed and stored for at least 30 days
- **Archive** - optimized for data that can tolerate several hours of retrieval latency and will remain in the Archive tier for at least 180 days

## File Storage

Managed file shares for cloud or on-premise deployments.

![Azure File Storage](https://ucarecdn.com/2e60d249-17e4-4df2-abb4-ddc32797f72a/)

## Queue

A messaging store for reliable messaging between application components.

![Queue](https://docs.microsoft.com/en-us/azure/includes/media/storage-queue-concepts-include/azure-queue-service-components.png)

## Table Storage

A NoSQL store for schemaless storage of structured data.
