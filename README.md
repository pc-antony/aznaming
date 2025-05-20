# aznaming - Resource Naming Excel Workbook for Azure
## Introduction
The AzNaming spreadsheet was created to help cloud administrators define and manage their naming conventions, while providing a simple interface for design and build teams to generate a compliant name. The tool was developed using a naming pattern based on [Microsoft's best practices](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging). The cloud administrator/architect can defined the organizational components, and then users can use the spreadsheet to generate a name for the desired Azure resource. It ensures a consistent structure for resource naming whilst remaining flexible to allow individual adjustments to resource names.

The resources are named using abbreviations taken from the Cloud Adoption Framework. The [following table](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations) has abbreviations mapped to resource and resource provider namespace, and these have been used to generate the data in the excel sheet.

## Using the Workbook
The workbook has 2 visible tabs:
* Entities - this worksheet is for the cloud administrator/architect to set the structure of the naming convention.
* Resources - this worksheet is for the design/build teams to populate as resources are built. It allows you to select a resource and unique label, then creates a compliant name with this data. You can also select the Resource Group which this resource sits within (Resource Group names are created on the Entities tab).

There are also 2 hidden tabs which are used to populate the data behind the worksheets.
* _resource - this contains all the Azure resource standard names, groups, namespace and abbreviations as taken from the Cloud Adoption Framework. 
* _types - 
* 
### Resource Structure
![image](https://github.com/user-attachments/assets/cb2c520a-15b7-457d-9ea3-ab4294b4df34)
Please refer to the table shown in the Azure [recommended naming components](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming#recommended-naming-components) section for details of components. This workbook includes two more typical components:
* Tenant Name - large organisations may have multiple Azure tenants due to M&A, to allow for separation of Entra ID for regulatory or security reasons, or for partner or customer environments (B2B).
* Subscription - it is typical for large organisations to have multiple subscriptions in a tenant for resource and cost isolation for billing (internal billing/chargeback), resource quotas, or access control.

The sheet allows you to enter a description for each component, and then provide an abbreviation which is suggested to be between 3-5 characters maximum. This abbreviation is used when constructing the name. You don't have to include a value for all the components, only the ones you want to use in your naming structure.

Once this section is completed, define your structure using the dropdown boxes for each element. You need to define at least 3 elements in your name which can be a combination of any component defined above. The resource element can either be put at the start or at the end of name (select option: 'Resource Mnemonic at Start' or 'Resource Mnemonic at End'). 
![image](https://github.com/user-attachments/assets/7b24c5b5-776c-4c1b-a335-d5fcf80a92dc)

Once selected, as seen above the sheet will give you a visual representation of the naming convention.
