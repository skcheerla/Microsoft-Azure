The Azure Storage platform is a scalable, highly available, and secure cloud storage solution centered around the **Azure Storage Account** and various **core data services**.

---

### Core Storage Concepts

| Concept | Description |
| :--- | :--- |
| **Storage Account** | A container that groups a set of Azure Storage data services (blobs, files, queues, tables). It provides a unique namespace for your data, accessible globally over HTTP or HTTPS. |
| **Azure Blob Storage** | A massively scalable **object store** for unstructured data like text, binary data (images, videos, logs, backups, etc.). It includes support for **Data Lake Storage Gen2** for big data analytics. |
| **Azure Files** | Provides fully managed **file shares** in the cloud that can be accessed using the industry-standard **Server Message Block (SMB)** protocol or **Network File System (NFS)**. Ideal for "lift and shift" of on-premises file servers. |
| **Azure Queue Storage** | A service for storing and retrieving large numbers of messages. Used for **asynchronous messaging** between application components to build scalable and loosely coupled applications. |
| **Azure Table Storage** | A **NoSQL key-value store** for structured, non-relational data in a schema-less design. Cost-effective for storing large amounts of data that doesn't require complex joins. |
| **Azure Disk Storage** | **Block-level storage** volumes, primarily used as **managed disks** for Azure Virtual Machines (VMs) to provide persistent, durable, and high-performance storage. |

---

### Key Operational Concepts

* **Redundancy** (Durability and High Availability): Azure Storage offers different options to ensure your data is protected against hardware failures and regional disasters:
    * **Locally Redundant Storage (LRS):** Three copies of your data within a single data center.
    * **Zone-Redundant Storage (ZRS):** Three copies across multiple *Availability Zones* in a single region.
    * **Geo-Redundant Storage (GRS) / Read-Access Geo-Redundant Storage (RA-GRS):** Six copies across two geographically separate regions.
* **Access Tiers** (Cost Optimization): For Blob Storage, different tiers are available based on access frequency, allowing you to optimize costs:
    * **Hot Tier:** For frequently accessed data.
    * **Cool Tier:** For infrequently accessed data, stored for at least 30 days. Lower storage cost but higher access cost than Hot.
    * **Archive Tier:** For rarely accessed data, stored for at least 180 days, with the lowest storage cost but high retrieval latency and cost.
* **Security and Access Control:** Access to the storage account is secured through:
    * **Access Keys:** Primary and secondary keys providing full access.
    * **Shared Access Signatures (SAS):** A URI token that grants limited permissions to storage resources for a specified interval.
    * **Role-Based Access Control (RBAC):** Integrates with Azure Active Directory (Azure AD) to provide fine-grained access control.

The following video provides an introduction to Azure storage accounts and their different types.
[Azure Storage Accounts, Storage Types, and Storage Tiers](https://www.youtube.com/watch?v=ZXJj5kU3wsg)
http://googleusercontent.com/youtube_content/0
