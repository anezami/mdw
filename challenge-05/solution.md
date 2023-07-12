# Solution

```bash
az synapse sql pool create --name sqlpool --performance-level DW400c --resource-group mdw-oh-02-westeurope --workspace-name southridgesynapse02
```

Execute the following SQL script against the sqlpool

```sql
CREATE TABLE [dbo].[DimActors] (
    [ActorSK]     INT              NOT NULL,
    [ActorID]     UNIQUEIDENTIFIER NOT NULL,
    [ActorName]   VARCHAR (81)     NOT NULL,
    [ActorGender] CHAR (1)         NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimCategories] (
    [MovieCategorySK]          TINYINT      NOT NULL,
    [MovieCategoryDescription] VARCHAR (50) NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimCustomers] (
    [CustomerSK]      INT              NOT NULL,
    [CustomerID]      UNIQUEIDENTIFIER NOT NULL,
    [LastName]        VARCHAR (50)     NOT NULL,
    [FirstName]       VARCHAR (30)     NOT NULL,
    [AddressLine1]    VARCHAR (50)     NOT NULL,
    [AddressLine2]    VARCHAR (50)     NULL,
    [City]            VARCHAR (30)     NOT NULL,
    [State]           CHAR (2)         NOT NULL,
    [ZipCode]         CHAR (5)         NOT NULL,
    [PhoneNumber]     CHAR (10)        NOT NULL,
    [RecordStartDate] DATE             NOT NULL,
    [RecordEndDate]   DATE             NULL,
    [ActiveFlag]      BIT              NOT NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerSK]));

CREATE TABLE [dbo].[DimDate] (
    [DateSK]         INT      NOT NULL,
    [DateValue]      DATE     NOT NULL,
    [DateYear]       SMALLINT NOT NULL,
    [DateMonth]      TINYINT  NOT NULL,
    [DateDay]        TINYINT  NOT NULL,
    [DateDayOfWeek]  TINYINT  NOT NULL,
    [DateDayOfYear]  SMALLINT NOT NULL,
    [DateWeekOfYear] TINYINT  NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimLocation] (
    [LocationSK]   SMALLINT     NOT NULL,
    [LocationName] VARCHAR (50) NOT NULL,
    [Streaming]    BIT          NOT NULL,
    [Rentals]      BIT          NOT NULL,
    [Sales]        BIT          NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimMovieActors] (
    [MovieID] UNIQUEIDENTIFIER NOT NULL,
    [ActorID] UNIQUEIDENTIFIER NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimMovies] (
    [MovieSK]         INT              NOT NULL,
    [MovieID]         UNIQUEIDENTIFIER NOT NULL,
    [MovieTitle]      VARCHAR (100)    NOT NULL,
    [MovieCategorySK] TINYINT          NOT NULL,
    [MovieRatingSK]   TINYINT          NOT NULL,
    [MovieRunTimeMin] SMALLINT         NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimRatings] (
    [MovieRatingSK]          TINYINT     NOT NULL,
    [MovieRatingDescription] VARCHAR (5) NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[DimTime] (
    [TimeSK]          INT      NOT NULL,
    [TimeValue]       TIME (7) NOT NULL,
    [TimeHour]        TINYINT  NOT NULL,
    [TimeMinute]      TINYINT  NOT NULL,
    [TimeSecond]      TINYINT  NOT NULL,
    [TimeMinuteOfDay] SMALLINT NOT NULL,
    [TimeSecondOfDay] INT      NOT NULL
)
WITH (HEAP, DISTRIBUTION = REPLICATE);

CREATE TABLE [dbo].[FactRentals] (
    [RentalSK]       INT              NOT NULL,
    [TransactionID]  UNIQUEIDENTIFIER NOT NULL,
    [CustomerSK]     INT              NOT NULL,
    [LocationSK]     SMALLINT         NOT NULL,
    [MovieSK]        INT              NOT NULL,
    [RentalDateSK]   INT              NOT NULL,
    [ReturnDateSK]   INT              NULL,
    [RentalDuration] TINYINT          NULL,
    [RentalCost]     MONEY            NOT NULL,
    [LateFee]        MONEY            NULL,
    [TotalCost]      MONEY            NULL,
    [RewindFlag]     BIT              NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerSK]));

CREATE TABLE [dbo].[FactSales] (
    [SalesSK]      INT              NOT NULL,
    [OrderID]      UNIQUEIDENTIFIER NOT NULL,
    [LineNumber]   TINYINT          NOT NULL,
    [OrderDateSK]  INT              NOT NULL,
    [ShipDateSK]   INT              NULL,
    [CustomerSK]   INT              NOT NULL,
    [LocationSK]   SMALLINT         NOT NULL,
    [MovieSK]      INT              NOT NULL,
    [DaysToShip]   TINYINT          NULL,
    [Quantity]     TINYINT          NOT NULL,
    [UnitCost]     MONEY            NOT NULL,
    [ExtendedCost] MONEY            NOT NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerSK]));

CREATE TABLE [dbo].[FactStreaming] (
    [StreamingSK]       INT              NOT NULL,
    [TransactionID]     UNIQUEIDENTIFIER NOT NULL,
    [CustomerSK]        INT              NOT NULL,
    [MovieSK]           INT              NOT NULL,
    [StreamStartDateSK] INT              NOT NULL,
    [StreamStartTimeSK] INT              NOT NULL,
    [StreamEndDateSK]   INT              NULL,
    [StreamEndTimeSK]   INT              NULL,
    [StreamDurationSec] INT              NULL,
    [StreamDurationMin] DECIMAL (10, 4)  NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerSK]));

CREATE TABLE [dbo].[Numbers] (
    [Number] INT NOT NULL
)
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = REPLICATE);


-- Declare local variables...
DECLARE
	@MaxNumber integer = 1;

-- Insert the seed number...
INSERT INTO dbo.Numbers
(Number)
VALUES
(1);

-- Loop through and insert additional data up to 1,048,576...
WHILE (@MaxNumber < 1000000)
BEGIN
	INSERT INTO dbo.Numbers
	(Number)
	SELECT Number + @MaxNumber
	FROM dbo.Numbers;

	SELECT @MaxNumber = MAX(Number)
	FROM dbo.Numbers;
END

INSERT INTO dbo.DimDate
(DateSK, DateValue, DateYear, DateMonth, DateDay, DateDayOfWeek, DateDayOfYear, DateWeekOfYear)
SELECT 
	Number AS DateSK,
	DateValue, 
	YEAR(DateValue) AS DateYear,
	MONTH(DateValue) AS DateMonth,
	DAY(DateValue) AS DateDay,
	DATEPART(WeekDay, DateValue) AS DateDayOfWeek,
	DATEPART(DayOfYear, DateValue) AS DateDayOfYear,
	DATEPART(Week, DateValue) AS DateWeekOfYear
FROM
(
	SELECT Number, DATEADD(d, Number - 1, '2017-01-01') AS DateValue
	FROM
	(
		SELECT Number
		FROM dbo.Numbers
		WHERE Number <= 365
	) AS N
) AS D;

INSERT INTO dbo.DimTime
(TimeSK, TimeValue, TimeHour, TimeMinute, TimeSecond, TimeMinuteOfDay, TimeSecondOfDay)
SELECT
	Number AS TimeSK,
	TimeValue,
	DATEPART(HOUR, TimeValue) AS TimeHour,
	DATEPART(MINUTE, TimeValue) AS TimeMinute,
	DATEPART(SECOND, TimeValue) AS TimeSecond,
	DATEPART(MINUTE, TimeValue) + (DATEPART(HOUR, TimeValue) * 60) AS TimeMinuteOfDay,
	DATEPART(SECOND, TimeValue) + ((DATEPART(MINUTE, TimeValue) + (DATEPART(HOUR, TimeValue) * 60)) * 60) AS TimeSecondOfDay
FROM
(
	SELECT Number, CAST(DATEADD(s, Number - 1, '2017-01-01') as time(7)) AS TimeValue
	FROM dbo.Numbers
	WHERE Number <= 86400
) AS T;


```