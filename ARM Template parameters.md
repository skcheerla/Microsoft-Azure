
ARM Template **parameters** are inputs that allow you to customize a template when it's deployed. They enable the same template file to be reused across different environments (e.g., Development, Staging, Production) or regions by simply changing the parameter values.

They are defined in the dedicated **`parameters`** section of the ARM JSON file.

-----

## Structure and Definition of Parameters

Each parameter is defined as a key-value pair, where the key is the parameter name, and the value is an object defining its type and optional constraints/defaults.

### Example Parameters Section

```json
"parameters": {
  "adminUsername": {
    "type": "string",
    "metadata": {
      "description": "The administrative username for the Virtual Machine."
    }
  },
  "storageAccountType": {
    "type": "string",
    "defaultValue": "Standard_LRS",
    "allowedValues": [
      "Standard_LRS",
      "Standard_GRS",
      "Premium_LRS"
    ],
    "metadata": {
      "description": "The SKU for the storage account."
    }
  }
}
```

### Key Parameter Properties

| Property | Description | Example Usage |
| :--- | :--- | :--- |
| **`type`** | **Required.** Specifies the data type of the parameter value. | `string`, `int`, `bool`, `object`, `array`, `securestring`, `secureObject`. |
| **`defaultValue`** | **Optional.** The value used if a parameter is not provided during deployment. | `"westus"`, `10`, `true`. |
| **`allowedValues`** | **Optional.** An array of values the parameter is permitted to take. Azure Resource Manager validates the input against this list. | `["Dev", "Prod"]`, `["Standard_LRS"]`. |
| **`minValue`/`maxValue`** | **Optional.** Used for `int` types to define the acceptable numerical range. | `minValue: 1`, `maxValue: 10`. |
| **`minLength`/`maxLength`** | **Optional.** Used for `string` and `array` types to define acceptable length/size. | `minLength: 6`, `maxLength: 24`. |
| **`metadata`** | **Optional.** Used to provide a `description` for the parameter, which is displayed in the Azure Portal's custom deployment UI. | `"description": "The region to deploy resources."` |

-----

## How Parameters are Used

Parameters are referenced throughout the `variables`, `resources`, and `outputs` sections using the `parameters()` function within square brackets `[]`.

### 1\. In the `resources` Section

Using a parameter to set the **location** and **name** of a resource:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2023-01-01",
    // Name is set by the input parameter
    "name": "[parameters('storageName')]", 
    // Location is set by the input parameter
    "location": "[parameters('location')]", 
    "sku": {
      // SKU is set by the input parameter
      "name": "[parameters('storageAccountType')]" 
    },
    // ... other properties
  }
]
```

### 2\. Parameter Files (`.parameters.json`)

While you can pass parameter values directly via the Azure CLI or PowerShell, best practice is to use a separate **parameter file**. This keeps sensitive values (like passwords) out of the main template and makes deployments repeatable.

The parameter file simply maps your template's parameter names to the values for a specific deployment:

```json
// azuredeploy.parameters.json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "value": "eastus"
    },
    "storageName": {
      "value": "mystorageprod001"
    },
    "storageAccountType": {
      "value": "Standard_GRS"
    },
    // Secure strings and objects should never have a defaultValue in the template.
    "adminPassword": { 
      "value": "SecurePassword123!" 
    }
  }
}
```

<img width="1161" height="492" alt="image" src="https://github.com/user-attachments/assets/ebe32a95-70f7-4094-87fc-dbae7d591a16" />


### Key Security Point: `securestring`

Use the parameter type **`securestring`** (or `secureObject`) for sensitive data like passwords or secrets. Azure prevents these values from being logged to the deployment history, protecting them from exposure. For security, never provide a `defaultValue` for a `securestring` directly in the template file.
