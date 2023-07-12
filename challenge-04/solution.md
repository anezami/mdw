# Solution

## Success criteria

- A new dataset has been stored in the data lake, which brings together the data from the source systems
    - Data from the source systems has been transformed to use **consistent data types and formats**;
    for example, if source systems use different data types or formats for a date,
    the conformed dataset would store all dates in a single, consistent data type and format.
    - Downstream consumers have no need to load data from various systems in order
    to process data from all sources; this new dataset is their single location
    for all data from all source systems that feed into the lake today and tomorrow.
    > NOTE: While the team will eventually process all of the data
    > (i.e., sales, streaming, rentals, customers, addresses, movies, and actors),
    > the team may advance to the next challenge after processing
    > a subset of the data as defined below.  
    >
    > **One of**: (a) Sales orders with details, (b) Streaming transactions, or (c) Rental transactions  
    > **AND**  
    > **One of**: (a) Customers with addresses, or (b) Movies with actors and mappings
- Each record in the new dataset is marked with an identifier of the original source system
- The original extracted data is preserved within the data lake

```bash
az storage container create --name conformed --account-name datalakesouthridge02 --resource-group mdw-oh-02-westeurope

az synapse spark pool create --name SparkPool --node-count 3 --node-size Medium --resource-group mdw-oh-02-westeurope --spark-version 2.4 --workspace-name southridgesynapse02 --enable-auto-pause true
```

Notebook: 
- Conform Catalog data
- Conform Customer data
- ...