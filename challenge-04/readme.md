# 4 - More than meets the eye

## Background story

Now that Southridge Video has all the data from their original systems
as well as from the acquired systems, they would like to begin
making better use of it.
Unfortunately, the data from the various systems is not consistent.
For example, a cursory look over the data has shown that some dates
are stored as integers, there is no consistent strategy for boolean values,
and each company had their own unique identifiers for movies.

It is time to conform the source data in the data lake into a more useable dataset.
Downstream consumers of the data should not need to worry about
negotiating these differences between data types and formats.
These downstream consumers also want a one-stop shop for all the data.
This will be especially important as new source systems are brought into the lake,
so that downstream consumers will not need to continuously repeat the boilerplate work
of locating or conforming the new data.
However, the original source data must also be preserved.
This will enable the creation of alternative intermediate datasets at any time,
and it will also enable deeper exploration for use cases such as anomaly detection.

As Southridge looks toward the future, they would also like to introduce a review process
such that all changes to the solution under source control must be approved by a second developer.

## Technical details

- One of Southridge's most immediate needs will be to populate a star schema
to serve reporting needs, but additional goals include enabling data scientists
to explore the data or train machine learning models.
    - The creation of this intermediate dataset is the team's opportunity
    to give the various downstream consumers a single location from which to load all
    of the data from all source systems.
    - This intermediate dataset also removes cognitive overhead from downstream consumers,
    since the heterogenous data types and inconsistent column names from the original systems
    will be homogenous and consistent in the conformed dataset.
    - The team is completely free to choose what data types the intermediate dataset will use.
    This is possible because no changes are made to the original source systems,
    which means there is no impact to any component which consumes directly from those source systems.
    Additionally, no changes are being made "in place" on the raw, extracted data in the data lake.
- While some entities are common between the various source systems,
it is important to understand that the data represents three unique business models.
The team might consider modelling the target dataset as the following five collections:
    - DVD sales orders
    - Video streaming
    - Video rentals
    - Customers (with addresses)
    - Movies (with actors)
- When conforming this data for downstream consumption:
    - The team **is** expected to create and store unified sets of common elements
    which exist across all source systems (e.g., the set of movies)
    - The team **is not** expected to design a schema in which transactions
    from different business models are combined;
    for example, teams should not attempt to map a Shipment Date of a DVD
    sales order to the Return Date of a rental
- Given the various downstream consumption patterns and user personas,
e.g. business intelligence versus machine learning,
the team should not apply any logical business rules at this time.
    - Creating a homogenous dataset with consistent column names and data types
    is common work which will benefit most, if not all, downstream consumers.
    - Various downstream consumers may have different requirements when it comes to
    de-duplication, resolution of conflicts between source systems, etc.
    The team should not make such decisions now,
    as these downstream requirements have not yet been specified.

## Success criteria

- Policies have been implemented in the version control solution such that
    - Developers cannot push changes directly to the main branch
    - Changes are reviewed with at least one explicit approval from another developer
    before being incorporated into the main branch
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

## Tips

- The team should focus on **fatal** anomalies when transforming this data,
and should not attempt to perform a full cleansing of the data.
    - Downstream consumers **must not** encounter exceptions due to unexpected data types
    - While other anomalies like duplicate records or inconsistent movie ratings
    might be later cleansed for business intelligence and reporting purposes,
    some consumers could find unexpected value in these anomalies!
- [A sample data catalog](../../resources/DataCatalog/DataCatalog.xlsx)
does exist to serve as **an example and inspiration** for the team,
but **it is not required** that the team match the catalog's target schema.
The team remains free to shape the intermediate output dataset in any way they choose,
as long as it fulfills the success criteria.

## Resources

### Ramp Up

- [Extract, transform, load (ETL)](https://docs.microsoft.com/en-us/azure/architecture/data-guide/relational-data/etl)
- [Extract, load, and transform (ELT)](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading)
- [How to teach Git](https://rachelcarmena.github.io/2018/12/12/how-to-teach-git.html)

### Choose Your Tools

- [Ingest, prepare, and transform using Azure Databricks and Data Factory](https://azure.microsoft.com/en-us/blog/operationalize-azure-databricks-notebooks-using-data-factory/)
- [What is Azure HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hadoop/apache-hadoop-introduction)
- [Load the data into Azure Synapse Analytics (formerly Azure SQL Data Warehouse) staging tables using PolyBase](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/design-elt-data-loading#4-load-the-data-into-sql-data-warehouse-staging-tables-using-polybase)

### Dive In

#### Azure Databricks

- [Tutorial: Extract, transform, and load data using Azure Databricks](https://docs.microsoft.com/en-us/azure/azure-databricks/databricks-extract-load-sql-data-warehouse)

#### HDInsight

- [Tutorial: Extract, transform, and load data by using Apache Hive on Azure HDInsight](https://docs.microsoft.com/en-us/azure/storage/blobs/data-lake-storage-tutorial-extract-transform-load-hive)

#### Polybase

- [Tutorial: Load New York Taxicab data to Azure Synapse Analytics (formerly Azure SQL Data Warehouse)](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)

#### Azure DevOps

- [Improve code quality with branch policies](https://docs.microsoft.com/en-us/azure/devops/repos/git/branch-policies?view=vsts)

#### GitHub

- [Enabling required reviews for pull requests](https://help.github.com/articles/enabling-required-reviews-for-pull-requests/)
