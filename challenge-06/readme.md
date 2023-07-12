# 6 - It's Groundhog Day... again

## Background story

The data store for serving marketing reports has been established,
and the Southridge Video leadership is thrilled with the team's progress!
Of course, a one time ingestion of the data isn't going to fulfill ongoing business needs,
so they would like the team to investigate the scheduling of automated imports from all data sources.

Leadership requires that each new day's data is imported.
Given the time and cost of ingestion,
they want to ensure that only the new data is imported rather than a full import of all historical data.

Because data may change over time, such as a customer's address,
leadership wants to ensure that no data is overwritten or changed during the import process.
All data should be brought over in its current state,
and it must then be conformed to the data structure
previously established by the team.

Additionally, business stakeholders have expressed concerns
around the duration of the initial extraction process.
They are also puzzled as to how they can trust that
the ongoing daily ingestion is not missing any data.
They have asked the team to establish a process
to track and report on the execution of the pipelines.

## Technical notes

In previous challenges, the team has processed the bulk data from 2017.
Now, the team will incrementally process the data from 2018 on a daily basis.
Start with Jan 1, 2018, then Jan 2, 2018, and so on.

All source systems include a `CreatedDate` and `UpdatedDate`.
Note that these are at the Date granularity, not the DateTime granularity.
When importing incremental loads, the intent is to process each day's full data;
that is, there is no requirement to support more granular (e.g., hourly) imports.

### Southridge Video and VanArsdel, Ltd. SQL databases

A stored procedure has been provided in each data store to add data for each new day.

Any time the team would like to test the import process,
run the `MoveData` stored procedure to add data for the desired date,
and then run the solution to import and process the data.

```sql
exec dbo.MoveData @DateToMove='2018-01-01'
```

This procedure has been designed to move only one day's data at a time,
so that the team can have full control over how much data is processed
as they iterate upon and validate their solution.

The `@SwitchIn` parameter may be ignored, as it is defaulted to 1.
The intent of this parameter is to support the reversal of inadvertent execution.
That is, if the team accidentally executes the procedure for
`@DateToMove='2018-05-25'` or some other arbitrary date,
the team can `exec dbo.MoveData @DateToMove='2018-05-25', @SwitchIn=0`
to revert that data.

If errors are encountered when executing the MoveData procedure,
the team may ask the coach for assistance.

### Fourth Coffee CSV Files

There is a `C:\Transactions_2018` directory alongside the `C:\Rentals` directory.
While the initial data from 2017 had been consolidated to a single file for the entire year,
the 2018 data has been captured into daily files.

The team may decide whether to move or copy the daily files
into the `C:\Rentals` directory or leave them where they are; that is,
they may add the `C:\Transactions_2018` directory as a new file system source.

## Success criteria

- All **new or updated** data is imported from all data sources
    - Data currently in the data lake **is not** modified in any way
    - **No** existing data is included when the import process executes
- The import process is scheduled to run nightly at midnight, and it may also be run on-demand
    - In either case, due to the Date granularity, data from the current date is not processed
- The team has logged the number of records ingested from each source
- The team has explained to a coach how they would surface the ingestion metrics to leadership

## Resources

### Ramp Up

- [Incremental ETL Processing With Azure Data Factory v2](https://sqlofthenorth.blog/2017/10/17/incremental-etl-loading-with-azure-data-factory-v2/)

### Choose Your Tools

- [Azure Monitor overview](https://docs.microsoft.com/en-us/azure/azure-monitor/overview)
- [What is Application Insights?](https://docs.microsoft.com/en-us/azure/azure-monitor/app/app-insights-overview)

### Dive In

#### Incrementally loading data

- [Incrementally load data from a source data store to a destination data store](https://docs.microsoft.com/en-us/azure/data-factory/tutorial-incremental-copy-overview)
- [Run a Databricks notebook with Databricks Notebook Activity in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/transform-data-using-databricks-notebook)
- [Change Data Capture (SSIS)](https://docs.microsoft.com/en-us/sql/integration-services/change-data-capture/change-data-capture-ssis?view=sql-server-2017)

#### Logging and telemetry

- [Find and diagnose run-time exceptions with Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-runtime-exceptions)
- [Monitor and alert on application health with Azure Application Insights](https://docs.microsoft.com/en-us/azure/azure-monitor/learn/tutorial-alert)
