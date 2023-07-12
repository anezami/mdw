# Solution

## Success criteria:

* All data from the dbo schema from the CloudSales Azure SQL database has been extracted and stored in the enterprise data lake
* All data from the dbo schema from the CloudStreaming Azure SQL database has been extracted and stored in the enterprise data lake
* All data from the movies collection in Cosmos DB has been extracted and stored in the enterprise data lake

We setup everything in Azure Synapse

```bash
az synapse workspace create --file-system synapse --name southridgesynapse02 --resource-group mdw-oh-02-westeurope --sql-admin-login-password md@ohPwd001! --sql-admin-login-user southridge --storage-account datalakesouthridge02

ip=$(curl ipinfo.io/ip)

az synapse workspace firewall-rule create --name allowClientIp --workspace-name southridgesynapse02 --resource-group mdw-oh-02-westeurope --start-ip-address $ip --end-ip-address $ip
```

Next, set up code repository and check out following resources:

## Linked services

- Linked Service: CloudSales (Azure SQL Database)
- Linked Service: CloudStreaming (Azure SQL Database)
- Linked Service: DataLake (Storage)
- Linked Service: Movies (Cosmos DB)

## Datasets
- DataSet: CloudSalesDataset (Azure SQL Database). Note that this has a general parameterized tableName @dataset().tableName
- DataSet: CloudStreamingDataset (Azure SQL Database), also using parameterized tableName
- DataSet: DataLakeParquet (Storage as Parquet). Folder path and filename are parameterized
- DataSet: DataLakeMoviesJson (Storage as Json). Folder path and filename are parameterized
- DataSet: MoviesDataSet (Cosmos DB Source)

## Pipelines
- Pipeline: Challenge02_CloudSalesPipeline. This pipeline will run over an array of table names and copy data from Azure SQL Database to Datalake as a set of parquet files
- Pipeline: Challenge02_CloudStreamingPipeline. This pipeline will run over an array of table names and copy data from Azure SQL Database to Datalake as a set of parquet files
- Pipeline: Challenge02_MoviesPipeline. This pipeline extracts all data from Cosmos Database and stores it on the Datalake as a Json file