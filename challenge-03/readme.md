# 3 - Breaking the fourth wall

## Background story

Congratulations, Southridge Video has now centralized their own data!
Remember that several stores have joined the Southridge family over
a relatively short period of time.
Leadership has become increasingly concerned that they are unable to effectively
make decisions or capitalize on opportunities if they do not incorporate the data
from these additional stores.
With the establishment of an enterprise data lake, the time to leverage the additional
data is close at hand!
First, this additional data needs to be extracted.

Southridge would like the team to update the data flow
to incorporate data from the acquired locations.
They would like the team to leverage the services
they provisioned for the previous import effort, which brought
both the Azure SQL Databases and Azure Cosmos DB data to
the enterprise data lake.
Additionally, they would like the team to ensure that
any solutions created to extract data from these new stores
can be further leveraged when any additional stores are acquired.

Once there's a baseline for the services used to import data,
Southridge leadership also requires that the team establish version control to ensure that the work performed by the team can be
persisted, tracked, and potentially audited.

## Technical details

The team will now begin working with the data from the acquired stores.

Although the data will land in the same data lake,
it should be grouped by the source system of record.

Mapping tables have been created in each on-premises data source
to relate movies which exist in both the on-premises system and the
Southridge cloud-based movie catalog.

### Fourth Coffee Resources

For the purposes of this OpenHack,
Fourth Coffee's environment is modelled as an Azure VM.
This VM represents Fourth Coffee's on-premises solution,
in which they store CSV data in the `C:\Rentals` directory.

The team will note that there is also a `C:\Transactions_2018` folder.
This data is for future use and is not meant to be processed during this challenge.

The team's coach can provide VM login information,
or the team can [reset the VM's administrator login information](https://docs.microsoft.com/en-us/azure/virtual-machines/troubleshooting/reset-rdp#reset-by-using-the-azure-portal).  

### VM Credentials  

By default, the automated lab setup process uses `southridge` as the VM administrator username and `md@ohPwd001` as the password.  This is the same for both VMs.  

UID:  

```text  
southridge
```  

PWD:  

```text  
md@ohPwd001
```  

### VanArsdel, Ltd. Resources

For the purposes of this OpenHack,
VanArsdel, Ltd.'s environment is modelled as an Azure VM.
This VM represents VanArsdel, Ltd.'s on-premises solution,
in which they maintain an on-premises SQL Server database.

The team will note that there is a `Hidden` schema in the database.
This data is for future use and is not meant to be processed during this challenge.

The team's coach can provide VM login information,
or the team can [reset the VM's administrator login information](https://docs.microsoft.com/en-us/azure/virtual-machines/troubleshooting/reset-rdp#reset-by-using-the-azure-portal).  

By default, the lab setup process uses `southridge` as the VM administrator username.  

By default, the lab setup process uses `southridge` as the SQL administrator login username.  

Both VMs use the same password by default.  See the credentials above for more information.

## Success criteria

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

## Tips

- You can use the SQL Server Management Studio installed on VanArsdel Ltd.
VM to connect to and manage their databases.
- Regarding common Azure infrastructure:
    - It **is** acceptable to author new pipelines, create new directories,
    or install new runtimes on existing resources
    - It **is not** acceptable to create new storage accounts or data factories
- Teams choosing to use the self-hosted integration runtime should take note of
[the supported file formats](https://docs.microsoft.com/en-us/azure/data-factory/supported-file-formats-and-compression-codecs)
and any additional prerequisites or conditions that may
apply to the selected format
(e.g., installation of a JRE for [Parquet](https://docs.microsoft.com/en-us/azure/data-factory/supported-file-formats-and-compression-codecs#parquet-format) or [ORC](https://docs.microsoft.com/en-us/azure/data-factory/supported-file-formats-and-compression-codecs#orc-format))
    - Teams may avoid some of these additional prerequisites
    by copying the data "as-is" and deferring
    additional conversions until they perform cloud processing
    - For security and performance purposes in production scenarios,
    the self-hosted integration runtime is ideally installed on a separate
    machine from the one that hosts the data itself (e.g., on a
    [jumpbox](https://docs.microsoft.com/en-us/azure/architecture/reference-architectures/n-tier/n-tier-sql-server#architecture)).
    For the scope of this OpenHack, it is acceptable to install the runtime on the machine which hosts the data.
- When using Azure DevOps, note that an Azure DevOps organization is limited to
[5 users with *Basic* access](https://docs.microsoft.com/en-us/azure/devops/organizations/security/access-levels?view=azure-devops).
- It is only required that the team persists **their authored solutions** to
version control. This does **not** include templates to deploy infrastructure
or code for any existing resource (e.g., scripts to create existing databases)
    - Example, if the team chooses to use Azure Data Factory:
        - all authored pipelines should be in version control
        - there is no need to persist Azure CLI, PowerShell,
        or ARM templates in version control
        - there's no need to add any existing source of data to source control

## Resources

### Ramp Up

- [Self-Hosted Integration Runtime](https://docs.microsoft.com/en-us/azure/data-factory/concepts-integration-runtime#self-hosted-integration-runtime)
- [About Version Control](https://git-scm.com/book/en/v2/Getting-Started-About-Version-Control)

### Choose Your Tools

- [GitHub Guides - Hello World](https://guides.github.com/activities/hello-world/)
- [Azure DevOps - What is source control?](https://docs.microsoft.com/en-us/azure/devops/user-guide/source-control?view=vsts)

### Dive In

- [Load data into a dedicated SQL pool with SQL Server Integration Services (SSIS)](https://docs.microsoft.com/en-us/sql/integration-services/load-data-to-sql-data-warehouse?toc=%2Fazure%2Fsynapse-analytics%2Fsql-data-warehouse%2Ftoc.json&bc=%2Fazure%2Fsynapse-analytics%2Fsql-data-warehouse%2Fbreadcrumb%2Ftoc.json&view=sql-server-ver15)
- [Copy data from an on-premises SQL Server database to Azure Blob Storage](https://docs.microsoft.com/en-us/azure/data-factory/tutorial-hybrid-copy-portal)
- [Copy data to or from a file system by using Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/connector-file-system)
- [Visual authoring in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/author-visually)
