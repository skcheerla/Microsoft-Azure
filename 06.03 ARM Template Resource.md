
The **`resources`** section is the **core** of an Azure Resource Manager (ARM) template, as it declaratively defines every Azure service or component you want to deploy, update, or delete. It is an array of JSON objects, where each object represents a single resource.

-----

## Structure of an ARM Template Resource

Every resource definition within the `resources` array must have a few common, essential properties and then includes unique properties specific to that Azure service.

### Essential Resource Properties

| Property | Description | Example Value |
| :--- | :--- | :--- |
| **`type`** | **Required.** The namespace and type of the resource. This determines which Azure service is being deployed. | `Microsoft.Storage/storageAccounts` |
| **`apiVersion`** | **Required.** The version of the Resource Provider REST API to use. It controls the properties and syntax available for the resource. Always use the latest stable version when creating new templates. | `2023-01-01` |
| **`name`** | **Required.** A string value for the name of the resource. This is often set using template functions to ensure uniqueness and to reference parameters/variables. | `[variables('storageAccountName')]` |
| **`location`** | **Required (for most tracked resources).** Specifies the Azure region where the resource will be deployed. This is typically set to the resource group's location. | `[resourceGroup().location]` |
| **`properties`** | **Required (for configuration).** A nested object containing all the resource-specific configuration settings (e.g., storage SKU, network address prefixes, VM size). | `{ "sku": { "name": "Standard_LRS" } }` |

### Optional but Common Properties

| Property | Description | Example Use |
| :--- | :--- | :--- |
| **`tags`** | Key/value pairs used for organizing, billing, and management. | `{ "Environment": "Prod", "Project": "WebApp" }` |
| **`dependsOn`** | Defines an explicit dependency on another resource. ARM automatically handles most dependencies, but this is used for complex ordering requirements. | `[resourceId('Microsoft.Network/virtualNetworks', 'myVNet')]` |
| **`sku`** | Defines the tier, pricing, or capacity of the resource (often an alternative to being inside `properties`). | `{ "name": "Standard_B1" }` for an App Service Plan. |

-----

## Example: Deploying an Azure Storage Account

The example below shows a complete resource definition for a **Storage Account**.

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2023-01-01",
    "name": "[parameters('storageName')]",
    "location": "[parameters('location')]",
    "sku": {
      "name": "Standard_GRS" // Standard GRS (Geo-Redundant Storage) SKU
    },
    "kind": "StorageV2", // General-purpose v2 storage account
    "properties": {
      "accessTier": "Hot" // Hot access tier for frequently accessed data
    }
  }
]
```

<img width="1167" height="491" alt="image" src="https://github.com/user-attachments/assets/e0365468-f7b8-465f-a793-eabce0be3e70" />


### Key Takeaways

1.  **Declarative Nature:** You specify the resource's *final state* (e.g., I want a Storage Account, with this name, in this location, and of type GRS). ARM handles the process of making it so.
2.  **Resource Types:** Every resource is identified by its **type** (`Microsoft.Provider/ResourceType`).
3.  **API Version:** The `apiVersion` ensures that your template is compatible with a specific version of the resource's API, which is critical for long-term consistency.

The video below explains the template structure in detail, including how to define and use resource properties. [Creating Azure Resources like a Pro using ARM Templates](https://www.youtube.com/watch?v=q36cSy1wqS8) is a video that goes over the structure of an ARM template and how to define resources like a Web App and App Service Plan.
http://googleusercontent.com/youtube_content/0
