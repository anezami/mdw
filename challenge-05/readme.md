# 5 - A star is born

## Background story

Southridge leadership is very impressed with the team's work!
At last, all of their data is in one centralized location.
Excitement is growing; they're so close to making sense of it all!

In the past, the marketing team had described the reports they wished to see
in Power BI, and a contractor had been brought in to design the [necessary
schema](https://gist.github.com/riserrad/fabf9103ac1d8a383f16a1feee54a73a)
to support them. Southridge Video would like to see those reports finally come to life.
Leadership would like the team to leverage the conformed dataset in the data lake
to create and populate a data store which serves the marketing team's reports.

It is highly likely that data quality issues will be encountered.
These should be addressed as part of the data warehouse load.
Leadership has decided that if there are conflicts (incorrect movie rating or
categorization, for example), the fact that a data quality issue was found
should be logged, and the data from the Southridge systems should be used.

Looking toward the future, if new data becomes available,
the marketing team would like to be able to access it within an hour of its creation.

Southridge leadership would also like the team to explore how to efficiently determine
whether changes to their solution will work, preferably
before deploying it to production. Currently, the process for ensuring the
data import solution is correct is to manually analyze the results of a test
execution. Considering the time and effort this process requires, and the
ever-present chance of human error, they would like
the team to automate the process.

## Technical details

Note that the dimensions defined on the Table Definitions will be considered
[Type 1 dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_1:_overwrite),
except for DimCustomers which will be considered a
[Type 2 dimension](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row).

Unit tests avoid using external resources such
as remote services or http requests.
External resources can be [mocked](https://en.wikipedia.org/wiki/Mock_object)
or stubbed as appropriate.

Additional tests (e.g., integration, end to end, synthetic monitoring)
would make use of these external resources to ensure that the system
components are properly connected. However, this challenge deals only with
the unit tests, which assert the more granular behaviors or
logic of a single functional component.

### Table Definitions

The team will use these
[table definitions for the star schema](https://gist.github.com/riserrad/fabf9103ac1d8a383f16a1feee54a73a).

Note that the coach can supply scripts to accelerate table creation
after the team has chosen their target technology and created
at least one of the tables.

### Reports

The [Power BI](https://powerbi.microsoft.com/en-us/get-started/) reports are
available to the team in two styles, one which
[connects to Azure Synapse Analytics (formerly Azure SQL Data Warehouse)](https://ohmdwstor.blob.core.windows.net/teamresources/MDWOH-SQLDW.pbix),
and another which [connects to SQL Server Analysis Services](https://ohmdwstor.blob.core.windows.net/teamresources/MDWOH-SSAS.pbix).

## Success criteria

- A data store is created to support the marketing team's reporting requirements
- A process exists to cleanse and load data from the data lake
into the data store which serves the reports
- While the team is currently working with a static, bulk dataset,
the solution should support surfacing any new data in reports within one hour
of its ingestion into the data lake
- Data quality issues between source systems are identified, logged, and handled
    - In case of data conflicts (e.g., inconsistent ratings for a movie),
    values from the Southridge data are used
- The team has implemented at least one unit test to exercise a component of the pipeline
- No external data stores or services are used in the unit tests
- Unit tests are automatically executed for any code which is under review
- Reports are generated from each unit test execution, detailing
    - The pass/fail status
    - Logs containing more information about each failure

## Tips

- The team will likely need to consider network security.
Azure supports a mechanism to allow access from [Trusted Microsoft services](https://docs.microsoft.com/en-us/azure/storage/common/storage-network-security#trusted-microsoft-services).
- After selecting a target data store and creating at least one of the tables,
the team may ask their coach for scripts to accelerate the creation of the remaining tables.

## Resources

### Ramp Up

#### Star Schema

- [Star schema](https://en.wikipedia.org/wiki/Star_schema)
- [Dimension based on a Star Schema Design](https://docs.microsoft.com/en-us/sql/analysis-services/multidimensional-models-olap-logical-dimension-objects/dimensions-introduction?view=sql-server-2017#dimension-based-on-a-star-schema-design)

#### Unit Testing

- [Unit testing](http://softwaretestingfundamentals.com/unit-testing/)
- [Continuous Integration & Continuous Delivery with Databricks](https://databricks.com/blog/2017/10/30/continuous-integration-continuous-delivery-databricks.html)
    - See _Productionize and write unit test_ heading

### Choose Your Tools

- [What is Azure Synapse Analytics (formerly Azure SQL Data Warehouse)](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is)
- [What is Azure Analysis Services](https://docs.microsoft.com/en-us/azure/analysis-services/analysis-services-overview)
- Depending on the team's chosen path, a variety of unit testing tools
will be applicable. The team's coach will provide more directed references
to ensure the best fit with team's selected technologies.

### Dive In

- [Connecting Power BI to Azure Synapse Analytics](https://docs.microsoft.com/en-us/power-bi/connect-data/service-azure-sql-data-warehouse-with-direct-connect#connecting-through-power-bi-desktop)

#### Azure Synapse Analytics (formerly Azure SQL Data Warehouse)

- [Loading data with PolyBase](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview?toc=/azure/storage/blobs/toc.json#azure-sql-data-warehouse-polybase)
- [Accessing Azure Synapse Analytics (formerly Azure SQL Data Warehouse) from Azure Databricks](https://docs.microsoft.com/en-us/azure/databricks/data/data-sources/azure/sql-data-warehouse)
- [Copy data to or from Azure Synapse Analytics (formerly Azure SQL Data Warehouse) by using Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/connector-azure-sql-data-warehouse)

#### Azure Analysis Services

- [Tabular modeling (1400 compatibility level)](https://docs.microsoft.com/en-us/sql/analysis-services/tutorial-tabular-1400/as-adventure-works-tutorial?view=sql-server-2017)

#### Automating tests for Azure DevOps repositories

- [Pipelines: Test Python code](https://docs.microsoft.com/en-us/azure/devops/pipelines/languages/python?view=vsts&tabs=ubuntu-16-04#test)
- [Pipelines: Build Java apps in Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/languages/java?view=vsts#build-your-code-with-maven)

#### Automating tests for GitHub repositories

- [Build and test with Azure Pipelines](https://github.com/marketplace/azure-pipelines)
- [Build and test with Travis CI](https://github.com/marketplace/travis-ci)
- [Build and test with CircleCI](https://github.com/marketplace/circleci)
