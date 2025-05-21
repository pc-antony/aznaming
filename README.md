# aznaming - Resource Naming Excel Workbook for Azure
## Introduction
The AzNaming spreadsheet was created to help cloud administrators define and manage their naming conventions, while providing a simple interface for design and build teams to generate a compliant name. The tool was developed using a naming pattern based on [Microsoft's best practices](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging). The cloud administrator/architect can defined the organizational components, and then users can use the spreadsheet to generate a name for the desired Azure resource. It ensures a consistent structure for resource naming whilst remaining flexible to allow individual adjustments to resource names.

The resources are named using abbreviations taken from the Cloud Adoption Framework. The [following table](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations) has abbreviations mapped to resource and resource provider namespace, and these have been used to generate the data in the excel sheet.

## Using the Workbook
The workbook has 2 visible tabs:
* Entities - this worksheet is for the cloud administrator/architect to set the structure of the naming convention based on client and environment attributes.
* Resources - this worksheet is for the design/build teams to populate as resources are built. It allows you to select a resource and unique label, then creates a compliant name with this data. You can also select the Resource Group which this resource sits within (Resource Group names are created on the Entities tab).

There are also 2 hidden tabs which are used to populate the data behind the worksheets.
* _resource - this contains all the Azure resource standard names, groups, namespace and abbreviations as taken from the Cloud Adoption Framework. It also repeats the select naming structure from 'Entities' worksheet and can be used for per-resource customisation - this is explained below.  
* _types - this is just a data sheet used to populate drop-down boxes etc. It should not typically require editing.
  
### Resource Structure
![image](https://github.com/user-attachments/assets/cb2c520a-15b7-457d-9ea3-ab4294b4df34)
Please refer to the table shown in the Azure [recommended naming components](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming#recommended-naming-components) section for details of components. This workbook includes two more typical components used for naming:
* Tenant Name - large organisations may have multiple Azure tenants due to M&A, to allow for separation of Entra ID for regulatory or security reasons, or for partner or customer environments (B2B).
* Subscription - it is typical for large organisations to have multiple subscriptions in a tenant for resource and cost isolation for billing (internal billing/chargeback), resource quotas, or access control.

### 'Entities' worksheet
This sheet allows you to enter a description for each component, and then provide an abbreviation which is suggested to be between 3-5 characters maximum. This abbreviation is used when constructing the name. You don't have to include a value for all the components, only the ones you want to use in your naming structure.

> 🖋️ **Note:** The workbook only allows a specific workload to be specified, and a specific environment. Therefore, if you have multiple workloads and each workload has multiple environments (e.g. prod, dev, test etc..) then you will need a version of the workbook for each of these iterations. This is design intent, as downstream environments should replicate the higher environment - so as you build from non-production to production, you can just create a copy of the workbook, change - for example - the environment to a new value, and all the resources will update automatically.

Once this section is completed, define your structure using the dropdown boxes for each element. You need to define at least 3 elements in your name which can be a combination of any component defined above. The resource element can either be put at the start or at the end of name (select option: 'Resource Mnemonic at Start' or 'Resource Mnemonic at End'). 

![image](https://github.com/user-attachments/assets/7b24c5b5-776c-4c1b-a335-d5fcf80a92dc)

Once selected, as seen above the sheet will give you a visual representation of the naming convention.

Now the structure is defined, the first naming job will be resource groups (RGs) which will be used to hold your resources. As high level entities, these are also defined in the 'Entities' worksheet. Describe the group of resources, then add a descriptive prefix which will be added to the name. Once defined, the worksheet will automatically create a Resource Group name which can be used in the next worksheet...

> 🖋️ **Note:** As resources are added to the RG, you will see a count on this worksheet so you know how many resources are in each RG.

**Example:*** In the example below, we have added the client information and then selected a structure using the workload, subscription, environment name, and Azure region code with the resource name given at the end (just before the resource count): 

![image](https://github.com/user-attachments/assets/6bfc1192-2ba5-449f-854c-f4a0e0fea6ab)

### 'Resources' worksheet
This worksheet is where you create per-resource names for all your resources. The resource group is selected first (based on the Azure resource namespace), then a resource type is selected. For each resource is it mandated that a specific name is added to make the resource unique to it's purpose. The 'Resource Unique Name Required' column gives a hint to what this name should be. The next column allows you to add a value (3-6 characters).

Finally, select a resource group to place the resource in (this will be a dropdown box of RG's defined in the Entities workbook). Once added, the resource name should automatically be generated.

> 🖋️ **Note:** The resource name will be created based on the required naming convention and restrictions. For example, storage accounts cannot have hyphens so for this resource type, the spreadsheet creates a name without any.

The option 'count option' on this page relates to how the worksheet increments the resource count (last element of the naming structure):
* Instance Count Same Resource - the count increments for each interation of a resource, regardless of whether the resource unique name is different or the resource is in a different RG. This is useful if as part of your naming convention you want to be able to see a count of each resource type.
* when a duplicate resource name is created (where the 'resource unique name' is the same)
* Instance Count Same Name - the count only increments when a duplicate resource name is created (where the 'resource unique name' is the same as is the rest of the naming convention (i.e. same env, azure region ...etc based on the structure decided). This effectively ensures no duplicate names are created which may cause build failures.
* Instance Count Same Name in RG - the count only increments when a duplicate resource name is created (where the 'resource unique name' is the same) within the same resource group.

> ⚠️ **Warning:** In Azure, the scope at which resource names must be unique depends on the type of resource. Some resources need to be globally unique (e.g. storage accounts, app services, key vaults, ACR) whilst most compute and networking resources only need to be unique within the RG. Be careful if using the 'Instance Count Same Name in RG' option as you may still get errors if you duplicate names in different RGs when the resource naming scope is global or tenant wide.

###Examples:

**Example 1**: 4 SQL Managed Instances created. Count option is per resource so the count is from 1-4 (4 managed instances)
![image](https://github.com/user-attachments/assets/1a794725-a06f-4f67-88d0-e6cc9fba6bf0)

**Example 2**: The same 4 SQL Managed Instances but with Count option 'Same Name' - so here only the resources with the exact same name (resource unique name value = 'data') is incremented. As there are 2 resources called 'data', these are differentiated.
![image](https://github.com/user-attachments/assets/ee33c2d4-70c6-459b-a3ba-0ef98ab82db7)

**Example 3**: Finally, the same 4 SQL Managed Instances but with Count option 'Same Name in RG' - so here although there are still 2 resources with exact same name (unique name 'data'), as they are in different resource groups, the count remains '-001' for both.

## Modifying Naming Structure per Resource
It is recommended that the same naming structure is used for all resources. By restricting the length of characters used for the resource unique name and the other elements used to build the name, resource names should be able to be created without exceeding the maximum length allowed. If there is a name created which is too long, you should see an error like this:
![image](https://github.com/user-attachments/assets/8e8720ff-da8c-4ebf-aa3b-4a9692b49c3c)

If names are too long, there is an option to allow per-resource modification of the naming structure used and the ability to drop an element or use different client attributes. If you unhide the worksheet '_resources' you will see each resource type in it's corresponding group; together with data for the structure of the name, the maximum characters allowed, and element information

> ⚠️ **Warning:** Be careful about modifications in this sheet as it is what the Resources sheet uses as a database. If you start rearranging or adding columns/rows you may upset the VLOOKUP functions which are used to select data.

If you look at columns H-K you will see the elements pre-populated to match what has been selected on the 'Entities' tab. However, each cell has a drop-down list which allows you to select an alternative element. You can also delete the value in column K (which is element 5 or 6 - depending on whether you have the resource mnemonic at the start or end of the name), if you want a shorter name.

Example:
![image](https://github.com/user-attachments/assets/be50a7c1-fe08-4c5c-9fd8-7fdf42e0a28c)

Option 1: We could change the naming structure from hyphenated to alphanumeric. In this case, key vaults are allowed hyphens in the name, but as our name has 7 elements it means each name has 6 hyphens in it. By changing the Key Vault names to be alphanumeric only, they are immediately shortened by 6 characters:
![image](https://github.com/user-attachments/assets/50669990-2b2f-4045-bc67-600622f1659e)
As our previous name was 5 characters too long, this now is accepted and the 'Resources' workbook has automatically updated the name:
![image](https://github.com/user-attachments/assets/172820e4-36bb-4913-bec7-6cf0f1297a10)

Option 2: We could change the naming structure to reduce the length of the name. In this case, with our key vault again we have kept the structure as hyphenated but we have removed the 'Subscription' element from the name:
![image](https://github.com/user-attachments/assets/72ca9236-2cdb-4346-8a10-ba8c7ef8a3fa)
This shorter name is now below the length limit:
![image](https://github.com/user-attachments/assets/f945ab1b-665f-4e92-bd8a-e483cb02c9ca)

> 🖋️ **Note:** If you change resource structures for specific resources, you will need to record them as exceptions in your naming convention (as they don't match the standard convention as defined in the 'Entities' worksheet.
