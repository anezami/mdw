# 2 - Lights, camera, action

## Background story

Great job! Through your efforts, Southridge now has a centralized location
in which they can store all of their enterprise data.

Remember that Southridge Video currently stores data in Azure SQL Databases
and Cosmos DB document collection.
Southridge would now like to extract the data from these systems into the data lake.
This will set the stage for incorporating additional data from the brick and mortar
stores which Southridge has recently acquired, and support a variety of
usage patterns and user personas.

## Technical details

The team has the freedom during OpenHack to choose the solutions which best fit the needs of Southridge Video.
However, the team must be able to explain the thought process behind the decisions to the team's coach.

At present, encryption and access control is not a requirement for the data.

The team will find the following resources in the OpenHack lab subscription.

### Southridge Video Resources

Southridge has **two** Azure SQL DBs and a document collection in Cosmos DB.
The team will focus on these resources for Challenge 2.

The team will note that there is a `Hidden` schema in the databases.
This data is for future use and is not meant to be processed during this challenge.

[Access keys for Cosmos DB](https://docs.microsoft.com/en-us/azure/cosmos-db/secure-access-to-data)
are available from within the Azure portal.

The team's coach can provide credentials for the SQL databases.
By default, the lab setup process uses `southridge` as the SQL administrator login username.
Alternatively, the team may [set the Active Directory Admin](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server)
to one of the provided OpenHack accounts.

### Fourth Coffee Resources

Fourth Coffee resources will be introduced in a later challenge.

### VanArsdel, Ltd. Resources

The VanArsdel, Ltd. resources will be introduced in a later challenge.

## Success criteria

- All data from the `dbo` schema from the `CloudSales` Azure SQL database
has been extracted and stored in the enterprise data lake
- All data from the `dbo` schema from the `CloudStreaming` Azure SQL database
has been extracted and stored in the enterprise data lake
- All data from the `movies` collection in Cosmos DB
has been extracted and stored in the enterprise data lake

## Tips

- Focus on the immediate objective of landing the data in the data lake.
There is no need to implement transformation, cleansing, etc. at this stage.
Once the data has been landed, such processing can take place in future challenges.
- The previous challenge has mentioned setting permissions on sensitive data.
It is **not required** that the team sets such permissions in this challenge,
only that they have previously communicated **how they would** do so.
    - If the team does wish to set the permissions, be aware
    of a [known issue with special characters](https://github.com/Microsoft/AzureStorageExplorer/issues/980),
    for which the workaround is to ensure that the object names do not contain
    certain special characters. For example, `dbo.Customers` will work, while
    `[dbo].[Customers]` will not.
- Some of the brick and mortar stores which Southridge Video is acquiring
will store their data on premises, and some may use flat files rather than relational databases.
When selecting a technology to ingest the datasets for this challenge,
the team should consider whether that same technology may be leveraged in the future.

## Resources

### Ramp Up

This challenge focuses on the Extract portion of ETL and ELT workloads,
and the Ingestion and Storage stages of modern data warehousing:

- [ETL and ELT overviews](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/etl)

### Choose Your Tools

- [What is Azure Synapse Analytics?](https://docs.microsoft.com/en-us/azure/synapse-analytics/overview-what-is)
- [Dedicated SQL pool (formerly SQL DW)](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [Introduction to Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/introduction)
- [Azure Feature Pack for Integration Services (SSIS)](https://docs.microsoft.com/en-us/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-2017)

### Dive In

- [Quickstart: Bulk load data using the COPY statement with SQL pool](https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/quickstart-bulk-load-copy-tsql)
- [5-Minute Quickstarts: Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/#5-minute-quickstarts)
- [Move data to or from Azure Blob Storage using SSIS connectors](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/move-data-to-azure-blob-using-ssis)
