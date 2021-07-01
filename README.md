# Création d'un rapport de rétrofacturation pour Azure Purview

Nous allons voir comment utiliser les informations d'Azure Log Analytics afin de créer un rapport permettant la retrofacturation d'utilisation des scans.
Ci-dessous une illustration d'un exemple de type de rapport que vous pourrez créer :

![sparkle](Pictures/000.png)


## Pre requis

- Un compte [Azure Purview](https://docs.microsoft.com/fr-fr/azure/purview/create-catalog-portal)
- Un espace de travail [Azure Log Analytics](https://docs.microsoft.com/fr-fr/azure/azure-monitor/logs/quick-create-workspace)
- [Power BI Desktop](https://www.microsoft.com/fr-fr/download/details.aspx?id=58494) 
- Optinonel : une licence Power BI Pro ou Premium


## Azure Purview
### Configuration d'Azure Purview

Dans un premier temps, nous allons configuer Azure Purview afin d'envoyer les informations de télémétries à Azure Log Analytics

Depuis le portail Azure, cherchez et sélectionnez votre compte Azure Purview
Cliquez sur **"Diagnostic settings"**, puis sur **"+ Add diagnostic setting"**

![sparkle](Pictures/001.png)

Dans la fenêtre "Diagnostic setting", dans la partie "Category details" cochez les cases :
- ScanStatusLogEvent
- AllMetrics

Puis dans la partie "Destination details" cochez la case **"Send to Log Analytics workspace"** et resneignez les information de votre espace de travail Azure Log analytics.

Cliquez sur le bouton **"Save"**

![sparkle](Pictures/002.png)


## Azure Log Analytics
### Création de la requête Azure Log Analytics

Depuis le portail Azure, cherchez et sélectionnez votre espace de travail Azure Log Analytics

Puis cliquez sur **"Logs"**


![sparkle](Pictures/003.png)

Nous allons utiliser les informations provenant de la table "PurviewScanStatusLogs"
Copiez la requête ci-dessous afin de récupérer les logs des 30 derniers jours :

```javascript
PurviewScanStatusLogs
| where TimeGenerated > ago(30d)
```

Collez cette requête dans l'éditeur comme illustré dans la copie d'écran ci-dessous, puis cliquez sur **"Run"**  :

![sparkle](Pictures/004.png)

### Export vers Power BI
Maintenant que nous avons le résultat désiré, nous allons l'exporter vers Power BI

CLiquez sur le bouton **"Export"**, puis sur **"Export to Power BI (M query)"**

![sparkle](Pictures/005.png)

Un fichier texte est alors généré puis téléchargé avec le script M permettant la récupération des données depuis Power BI Desktop.

![sparkle](Pictures/006.png)

## Création du rapport Power BI
### Récupéation des données Azure Log Analytics

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

