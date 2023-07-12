# 7 - Limited release

## Background story

Congratulations! Southridge Video now has the tools and reports they need
to make intelligent business decisions!

Based on the reports, Southridge has decided that they **will**
continue to acquire brick and mortar stores,
and they want to accelerate these acquisitions.
This will of course result in an increased rate of change
for the team's solution, so the manual deployment process currently in
place will not remain tenable.

Leadership is asking the team to focus on
automating the deployment of changes into production.
Remembering pains from past projects, leadership has also requested
an isolated testing environment
where final checks can be performed
before the changes are ultimately pushed to production.

Additionally, the reports have indicated that there may be opportunities
to optimize the logistics of shipping DVD orders.
Southridge has asked their data scientists to explore this further.
The data scientists have in turn requested that the `DaysToShip`
calculation is brought back into the data lake,
so that it is available for their more direct consumption.

Leadership has determined that implementing this `DaysToShip` change
for the data scientists is a perfect, low-risk opportunity for the
team to prove out their deployment strategies.

## Success criteria

- A new, isolated environment has been created for testing purposes
- The calculated column for `DaysToShip` has been added to the
**transformed output** for all processing of the CloudSales data
- A pipeline has been created to deploy the updated transformation assets
(e.g, Scala jars, Python packages, Databricks Notebooks, SSIS packages)
    - The pipeline automatically deploys changes into the new testing environment
    when changes are merged into the team's deployment branch
    (e.g., `master`, `release`)
    - The pipeline performs automatic validation that the `DaysToShip` data
    appears correctly in the **transformed output** in the team's data lake,
    with a value equal to `[ShipDate] - [OrderDate]`
- The pipeline also deploys the changes into the production environment; however
    - The pipeline does not deploy to production until a team member explicitly approves it
    - The pipeline performs the same automatic validations from the testing environment

## Resources

### Ramp Up

- [Continuous Delivery](https://martinfowler.com/bliki/ContinuousDelivery.html)
- [DataDevOps](https://github.com/devlace/datadevops)

### Choose Your Tools

- [Azure Resource Manager overview](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview)
- [Azure Command Line Interface](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest)

### Dive In

- [Install the Azure CLI](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Create an Azure data factory and pipeline by using ARM template](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-resource-manager-template)
- [Create an Azure data factory and pipeline using PowerShell](https://docs.microsoft.com/en-us/azure/data-factory/quickstart-create-data-factory-powershell)
- [Upload and run a Spark JAR](https://docs.azuredatabricks.net/api/1.2/index.html#example-upload-and-run-a-spark-jar)
