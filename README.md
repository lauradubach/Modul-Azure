# Azure

## Ressource Gruppe über CLI erstellen

`az login`

`az group create --name myResourceGroupCli --location <Region>`

`az appservice plan create --name myAppServicePlanCli --resource-group myResourceGroupCli --sku F1 --location <Region>`

`az webapp create --name myWebAppClione --resource-group myResourceGroupCli --plan myAppServicePlanCli`

## Ressource Gruppe über Powershell erstellen


```powershell
# Setze Variablen
$resourceGroupName = "myResourceGroupPs"
$location = "centralus"
$appServicePlanName = "myAppServicePlanPs"
$webAppName = "myWebAppPs"

# Erstelle Ressourcengruppe
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Erstelle App Service Plan
New-AzAppServicePlan -ResourceGroupName $resourceGroupName -Name $appServicePlanName -Location $location

# Erstelle Web-App und weise den Service Plan zu
New-AzWebApp -ResourceGroupName $resourceGroupName -Name $webAppName -Location $location -AppServicePlan $appServicePlanName
```
