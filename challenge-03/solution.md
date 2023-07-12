# Solution

## Success criteria:
- All data from the `dbo` schema of the VanArsdel, Ltd. database
has been extracted and stored in the enterprise data lake
- All data from the `C:\Rentals` directory of the Fourth Coffee
VM has been extracted and stored in the enterprise data lake
- All extracted data is grouped by the source system of record
- All acquired stores - current and future - will leverage a common Azure infrastructure:
    - The same data lake created for Southridge must also be used for the data from Fourth Coffee,
    VanArsdel, Ltd.,
    and any stores acquired in the future
    - Any cloud-based data processing service must be used for all stores,
    rather than provisioning a separate resource for each acquired store
- The team has established version control
- The team has persisted all extraction solution(s) in version control

## Steps to solution

1. Remote in the Virtual Machines (southridge/md@ohPwd001!) and install the Integration Runtime from https://www.microsoft.com/en-us/download/details.aspx?id=39717  (you may need to disable IE Protected Mode and enable File Download)
2. Configure the Authentication Key on each of the machines (No need to update the IR for the challenge labs)
3. Add Linked Service: FourthCoffeeFileServer   
    - Type: File Server
    - Host: C:\Rentals
    - User name: FourthCoffee\southridge
    - Connect via integration runtime
4. Add Linked Service: VanArsdelSqlServer
    - Type: SQL Server
    - Connect via integration runtime
    - Server name: VanArsdelLtd
    - Database name: OnPremRentals
    - Authentication: Windows
    - User name: VanArsdelLtd\southridge
5. Add Dataset: FourthCoffeeFileSystemDataset
    - Filename is parameterized (using in foreach loop)
6. Add Dataset: VanArsdelLtdSqlTableDataset
    - Tablename is parameterized (using in foreach loop)
7. Add Dataset: DataLakeBinary
    - This dataset is used as a destination for the file server
8. Add Pipeline: Challenge03_FourthCoffeeFileSystemPipeline
    - A parameter "files" contains an array of files that need to be copied.
9. Add Pipeline: VanArsdelLtdSqlServerPipeline
