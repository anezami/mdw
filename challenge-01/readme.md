# 1 - Based on a true story

## Background story

Southridge Video uses Azure SQL Databases to store information about video streaming and DVD sales.
Their movie catalog data is separately stored in a Cosmos DB document collection.
While the current data structures are wonderful for managing orders, working with individual customers, and
displaying the movie catalog online, having the data sourced from different systems complicates their reporting.
Business stakeholders have expressed a desire to generate their own reports to dig deeper into sales trends.
In addition, upper management would like to be able to take advantage of machine learning
to increase sales and identify customer behavior.

To enable this long term vision, Southridge Video would like to establish
an enterprise data lake to feed current and future data needs.
This central repository can later be leveraged for
analysis, reporting and - eventually - machine learning.

Because the databases contain sales information,
the data lake will contain sensitive information
such as phone numbers and physical addresses.
Southridge Video is concerned about their customers' data,
so they want to ensure such data will be protected at all times,
with access limited to those who truly require it for business reasons.

## Technical details

The team has the freedom during OpenHack to choose the solutions which best fit the needs of Southridge Video.
However, the team must be able to explain the thought process behind the decisions to the team's coach.

At present, encryption is not a requirement for the data.
However, the selected technologies must include mechanisms
for controlling access to sensitive data.

## Success criteria

- The team has selected and provisioned a storage technology for use as an enterprise data lake
- The selected storage technology must support storing both structured and unstructured data
from both relational and non-relational source systems
- The team has stored any arbitrary file in the selected storage
- The team must explain to a coach how the selected technology would support restricting access
to the file, such that only designated users or groups could access it

## Tips

- Once logged in to the Opsgility portal, the team will have 6 Microsoft
Accounts (MSA) available for logging into Microsoft and Azure services.
**It is OK if two or more team members choose to use the same account**.
    - All accounts are in a single Azure subscription and Azure Active Directory,
    with the same access level.
    - Using at least two accounts will ensure that teams
    can experiment with collaboration features, whereas sharing a single account
    would not highlight how multiple identities can work together.
    - Using more than five accounts could limit functionality when using
    free tiers of certain services.
- The team does not need to **effectively set** permissions on the file;
they only need to explain to a coach **how** the selected storage technology would support doing so.
- A variety of storage options are available within Azure.
The team should consider the features and tradeoffs between these offerings.
- In particular, the team should consider that the enterprise data lake will serve
a variety of business needs and user personas.
File system semantics will be extremely useful as the team advances through the challenges.

## Resources

### Ramp Up

- [Data lakes](https://docs.microsoft.com/en-us/azure/architecture/data-guide/scenarios/data-lake)
- [Data Lakes and Data Warehouses: Why You Need Both](https://www.arcadiadata.com/blog/data-lakes-and-data-warehouses-why-you-need-both/)

### Choose Your Tools

- [What is Azure Synapse Analytics?](https://docs.microsoft.com/en-us/azure/synapse-analytics/overview-what-is)
- [Dedicated SQL pool (formerly SQL DW)](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [Secure a dedicated SQL pool](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-manage-security)
- [Introduction to Azure Data Lake Storage Gen2](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-introduction)
- [Azure Data Lake Storage Gen1](https://docs.microsoft.com/en-us/azure/data-lake-store/)
- [What is Azure Blob storage?](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-blobs-overview)

### Dive In

- [Quickstart: Create and query a dedicated SQL pool in Azure synapse Analytics](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/create-data-warehouse-portal)
- [Quickstart: Create an Azure Data Lake Storage Gen2 storage account](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-quickstart-create-account)
- [Get started with Azure Data Lake Storage Gen1 using the Azure portal](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal)
- [Create a storage account](https://docs.microsoft.com/en-us/azure/storage/common/storage-quickstart-create-account?toc=%2Fazure%2Fstorage%2Fblobs%2Ftoc.json&tabs=azure-portal)
