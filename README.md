# Azure

## Ressource Gruppe über CLI erstellen

`az login`

`az group create --name myResourceGroupCli --location <Region>`

`az appservice plan create --name myAppServicePlanCli --resource-group myResourceGroupCli --sku F1 --location <Region>`

`az webapp create --name myWebAppClione --resource-group myResourceGroupCli --plan myAppServicePlanCli`

## Ressource Gruppe über Powershell erstellen


```powershell

-> hat nicht funktioniert

# Setze Variablen
$resourceGroupName = "myResourceGroupPs"
$location = "centralus"
$appServicePlanName = "myAppServicePlanPs"
$webAppName = "myWebAppPs"

# Erstelle eine Ressourcengruppe
New-AzResourceGroup -Name $resourceGroupName -Location $location

# Erstelle einen App Service Plan
New-AzAppServicePlan -ResourceGroupName $resourceGroupName -Name $appServicePlanName -Location $location

# Erstelle eine Web-App und weise den Service Plan zu
New-AzWebApp -ResourceGroupName $resourceGroupName -Name $webAppName -Location $location -AppServicePlan $appServicePlanName
```

## Storage via Bash ein File hochladen

```bash
az login
storageAccountName=aditeststorage1234
resourceGroupName=adistoragetestch
sasToken=$(az storage account generate-sas \
    --permissions rwdlacup \
    --account-name $storageAccountName \
    --services b \
    --resource-types sco \
    --expiry $(date -u -d "1 hour" '+%Y-%m-%dT%H:%MZ') \
    --output tsv)
connectionString="BlobEndpoint=https://${storageAccountName}.blob.core.windows.net/;SharedAccessSignature=${sasToken}"
echo $connectionString
az storage blob upload \
    --container-name container1 \
    --name test.txt \
    --file test.txt \
    --connection-string $connectionString
```

```powershell
# Speicher-Konto und Schlüssel definieren
$storageAccountName = "mystoragelaura"
$storageAccountKey = "key2"
$containerName = "containertexts"
$localFilePath = "C:\Users\laura\OneDrive - TBZ\HF Cloud Native\ITCNE 24_Stundenplane_1.Sem_FR24.pdf"
$blobName = "Stundenplaene"

# Speicher-Konto Kontext erstellen
$context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

# SAS-Token für das Speicher-Konto erstellen (1 Tag gültig)
$sasToken = New-AzStorageAccountSASToken -Service Blob,File,Queue,Table -ResourceType Service,Container,Object -Permission rwlcup -ExpiryTime (Get-Date).AddDays(1) -Context $context 

# SAS URL für das Speicher-Konto generieren
$storageAccountUrl = "https://$storageAccountName.blob.core.windows.net"
$sasUrl = "$storageAccountUrl/$containerName?$sasToken"

Write-Host "SAS URL: $sasUrl"

# AzCopy Pfad definieren (angepasst auf deinen Installationspfad von AzCopy)
$azCopyPath = "C:\Users\laura\Downloads\azcopy_windows_amd64_10.24.0\azcopy_windows_amd64_10.24.0\azcopy.exe"

# Datei mit AzCopy hochladen
Start-Process -NoNewWindow -FilePath $azCopyPath -ArgumentList @("/Source:$localFilePath", "/Dest:$sasUrl", "/DestKey:$sasToken", "/Pattern:$blobName", "/S")

Write-Host "Datei wurde erfolgreich hochgeladen."


.\azcopy copy "C:\Users\laura\OneDrive - TBZ\HF Cloud Native\ITCNE 24_Stundenplane_1.Sem_FR24.pdf" "https://mystoragelaura.blob.core.windows.net/sv=2023-08-03&ss=bfqt&srt=sco&se=2024-05-16T14%3A04%3A47Z&sp=rwlcup&sig=%2Bnc%2B1xcUkRhyexgCbsZd1lfvv6S5I4%2FpgbdD8YcLsdk%3D"
```

hat nicht funktioniert