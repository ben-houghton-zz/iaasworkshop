
## Deploy Windows VM using Azure Command Line Interface

https://docs.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-cli

```
az login

az group create --name benhovm-rg --location westeurope

az vm create `  
  --resource-group benhovm-rg `  
  --name benho-vm1 --image win2016datacenter `  
  --admin-username azureuser `  
  --admin-password myPassword12

```
