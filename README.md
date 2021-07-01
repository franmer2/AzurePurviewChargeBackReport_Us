# Create a chargeback report for Azure Purview

We'll see how to use information from Azure Log Analytics to create a chargeback report.
The following is an illustration of the type of report that you can create:

![sparkle](Pictures/000.png)


## Pre requis

- An [Azure Purview account](https://docs.microsoft.com/fr-fr/azure/purview/create-catalog-portal)
- An [Azure Log Analytics workspace](https://docs.microsoft.com/fr-fr/azure/azure-monitor/logs/quick-create-workspace)
- [Power BI Desktop](https://www.microsoft.com/fr-fr/download/details.aspx?id=58494) 
- Optional: a Power BI Pro or Premium license


## Azure Purview
### Azure Purview setup

First, we'll configure Azure Purview to send telemetry to Azure Log Analytics

From the Azure portal, find and select your Azure Purview account
click **"Diagnostic settings"**, then **"+ Add diagnostic setting"**

![sparkle](Pictures/001.png)

In the "Diagnostic setting" window, in the "Category details" section, check:
- ScanStatusLogEvent
- AllMetrics

Then in the "Destination details" part check the box **"Send to Log Analytics workspace"** and fill in the information for your Azure Log Analytics workspace.

Click **"Save"**

![sparkle](Pictures/002.png)


## Azure Log Analytics
### Create the Azure Log Analytics query

From the Azure portal, find and select your Azure Log Analytics workspace

Click **"Logs"**


![sparkle](Pictures/003.png)

We will use the information from the "PurviewScanStatusLogs" table
Copy the query below in order to retrieve the logs for the last 30 days:

```javascript
PurviewScanStatusLogs
| where TimeGenerated > ago(30d)
```

Paste this query into the editor as shown in the screenshot below, and then click **"Run"**  :

![sparkle](Pictures/004.png)

### Export to Power BI
Now that we have the desired result, we will export it to Power BI

click **"Export"**, then click **"Export to Power BI (M query)"**

![sparkle](Pictures/005.png)

A text file is then generated and downloaded with the M script allowing data recovery from Power BI Desktop.

![sparkle](Pictures/006.png)

## Report creation
### Retrieving Azure Log Analytics data

Ouvrez le fichier texte précédement téléchargé puis copiez le script M (la partie encadrée en rouge)

![sparkle](Pictures/007.png)

Depuis Power BI Desktop, cliquez sur **"Get data"** puis **"Blank Query"**

![sparkle](Pictures/008.png)

une fois dans Power Query Editor, dans l'onglet **"Home"**, cliquez sur **"Advanced Editor"**

![sparkle](Pictures/009.png)

Supprimez le code par défault puis collez le script M copiez précédement, puis cliquez sur **"Done"** :
![sparkle](Pictures/010.png)

Vous devez obtenir un résultat similaire à celui ci-dessous. Vous pouvez aussi renommer la requête directement depuis le champ **"Name"**

![sparkle](Pictures/011.png)

Cliquez sur le bouton **"Close & Apply"** pour revenir dans Power BI desktop et commencer à créer votre rapport

![sparkle](Pictures/012.png)

