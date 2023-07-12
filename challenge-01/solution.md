# Solution

```bash
az group create --name mdw-oh-02-westeurope --location westeurope

az deployment group create --name mdw-deployment --resource-group mdw-oh-02-westeurope --template-file .\challenge-01\DeployMDWOpenHackLab.json --parameters .\challenge-01\DeployMDWOpenHackLab.parameters.json

az storage account create --name datalakesouthridge02 --resource-group mdw-oh-02-westeurope --location westeurope --enable-hierarchical-namespace true

az storage container create --name challenge01 --account-name datalakesouthridge02 --resource-group mdw-oh-02-westeurope

az storage blob upload --account-name datalakesouthridge02 --container-name challenge01 --file C:\Windows\Media\Alarm01.wav --name alarm.wav
```