---
title: Azure HDInsight Python SDK Preview
description: Reference for Azure HDInsight Python SDK. The HDInsight Python SDK provides classes and methods that allow you to manage your HDInsight clusters.
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
---

# HDInsight Python Management SDK Preview

## Panoramica

SDK HDInsight Python fornisce classi e metodi che ti permettono di gestire i tuoi cluster HDInsight. Includono operazioni per creare, eliminare, aggiornare, elencare, scalare, eseguire azioni script, monitorare, ottenere proprietà di cluster HDInsight, ed altro.

## Prerequisiti

* Un account Azure. Se non ne hai uno [richiedi una prova gratuita](https://azure.microsoft.com/free/).
* [Python](https://www.python.org/downloads/)
* [pip](https://pypi.org/project/pip/#description)

## Installatione SDK 

HDInsight Python SDK è disponibile su [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) e può esseere installando eseguendo: 

`pip install azure-mgmt-hdinsight`

## Autenticazione

Le SDK devono essere prima autenticate con la tua sottoscrizione Azure.  Segui l'esempio sottostante per creare un servizio principale e usarlo per autenticarti. Una volta completato avrai una instanza di `HDInsightManagementClient` che contiene molti metodi (descritti nelle sezioni successive) che possono essere usate per eseguire operazioni di amministrazione.

> [!NOTE]
> Ci sono altri metodi per autenticarsi rispetto all'esempio sottostante che posso essere più adatti alle tue necessità. Tutti i metodi sono descritti qui: [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](https://docs.microsoft.com/it-it/python/azure/python-sdk-azure-authenticate?view=azure-python)

### Esempi di autenticazione usando un servizio Principale

Innanzitutto effettua il login su [Azure Cloud Shell](https://shell.azure.com/bash). Verifica di utilizzare la sottoscrizione in cui vuoi che il servizio principale venga creato. 

```azurecli-interactive
az account show
```

Le informazioni della tua sottoscrizione è mostrata come JSON.

```json
{
  "environmentName": "AzureCloud",
  "id": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "isDefault": true,
  "name": "XXXXXXX",
  "state": "Enabled",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "user": {
    "cloudShellID": true,
    "name": "XXX@XXX.XXX",
    "type": "user"
  }
}
```

Se non sei loggato nella sottoscrizione corretta, seleziona quella giusta eseguendo il comando: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> Se non hai già registrato l'HDInsight Resource Provider con un altro metodo (come creare un cluster HDInsight tramite il portale Azure), devi farlo una volta prima di poterti autenticare. Questo può essere fatto tramite la [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo il seguente comando:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Dopo, scegli un nome per il tuo servizio principale e crealo con il seguente comando:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

Le informazioni sul servizio principale sono mostrate come JSON.

```json
{
  "clientId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "clientSecret": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "subscriptionId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "tenantId": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX",
  "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
  "resourceManagerEndpointUrl": "https://management.azure.com/",
  "activeDirectoryGraphResourceId": "https://graph.windows.net/",
  "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
  "galleryEndpointUrl": "https://gallery.azure.com/",
  "managementEndpointUrl": "https://management.core.windows.net/"
}
```
Copia il frammento di codice Python sottostante e popola le variabili `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` e `SUBSCRIPTION_ID` con le stringhe del JSON che sono state ritornate dopo aver eseguito il comando per creare il servizio principale.

```python
from azure.mgmt.hdinsight import HDInsightManagementClient
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.hdinsight.models import *

# Tenant ID for your Azure Subscription
TENANT_ID = ''
# Your Service Principal App Client ID
CLIENT_ID = ''
# Your Service Principal Client Secret
CLIENT_SECRET = ''
# Your Azure Subscription ID
SUBSCRIPTION_ID = ''

credentials = ServicePrincipalCredentials(
    client_id = CLIENT_ID,
    secret = CLIENT_SECRET,
    tenant = TENANT_ID
)

client = HDInsightManagementClient(credentials, SUBSCRIPTION_ID)
```


## Gestione del Cluster

> [!NOTE]
> Questa sessione prevede di aver completato l'autenticazione e la creazione di una istanza `HDInsightManagementClient` salvata in una variabile chiamata `client`. Istruzioni per l'autenticazione e per ottenere un `HDInsightManagementClient` possono essere trovate nella precedente sezione Autenticazione.

### Creare un Cluster

Un nuovo cluster può essere creato chiamando `client.clusters.create()`. 

#### Esempio

Questo esempio mostra come creare un cluster Spark con 2 nodi principali e 1 nodo worker.

> [!NOTE]
> Bisogna prima creare un Gruppo di Risorse e uno Storage Account come spiegato di seguito. Se sono già stati creati puoi ignorare questi passi.

##### Creare un Gruppo di Risorse

Puoi creare un Gruppo di Risorse utilizzando l'[Azure Cloud Shell](https://shell.azure.com/bash) eseguendo
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### Creare uno Storage Account

Puoi creare uno storage account usando l'[Azure Cloud Shell](https://shell.azure.com/bash) eseguendo:
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
Ora esegui il seguente comando per ottenere la chiave del tuo storage account (ne avrai bisogno per creare un cluster):
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
Il seguente frammento Python crea un cluster Spark con due nodi principali e 1 nodo worker. Completa le variabili vuote come spiegato nei commenti e sentiti libero di modificare gli atri parametri a seconda delle tue necessità.

```python
# The name for the cluster you are creating
cluster_name = ""
# The name of your existing Resource Group
resource_group_name = ""
# Choose a username
username = ""
# Choose a password
password = ""
# Replace <> with the name of your storage account
storage_account = "<>.blob.core.windows.net"
# Storage account key you obtained above
storage_account_key = ""
# Choose a region
location = ""
container = "default"

params = ClusterCreateProperties(
    cluster_version="3.6",
    os_type=OSType.linux,
    tier=Tier.standard,
    cluster_definition=ClusterDefinition(
        kind="spark",
        configurations={
            "gateway": {
                "restAuthCredential.enabled_credential": "True",
                "restAuthCredential.username": username,
                "restAuthCredential.password": password
            }
        }
    ),
    compute_profile=ComputeProfile(
        roles=[
            Role(
                name="headnode",
                target_instance_count=2,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            ),
            Role(
                name="workernode",
                target_instance_count=1,
                hardware_profile=HardwareProfile(vm_size="Large"),
                os_profile=OsProfile(
                    linux_operating_system_profile=LinuxOperatingSystemProfile(
                        username=username,
                        password=password
                    )
                )
            )
        ]
    ),
    storage_profile=StorageProfile(
        storageaccounts=[StorageAccount(
            name=storage_account,
            key=storage_account_key,
            container=container,
            is_default=True
        )]
    )
)

client.clusters.create(
    cluster_name=cluster_name,
    resource_group_name=resource_group_name,
    parameters=ClusterCreateParametersExtended(
        location=location,
        tags={},
        properties=params
    ))
```

### Ottieni dettagli del Cluster

Per ottenere proprietà di uno specifico cluster:

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### Esempio

Puoi usare `get` per confermare di aver creato il tuo cluster con successo.

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

L'output dovrebbe essere simile a:

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### Elencare Clusters

#### Elencare Clusters Nelle Sottoscrizione

```python
client.clusters.list()
```
#### Elencare Clusters dal Gruppo di Risorse

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> `list()` e `list_by_resource_group()` ritornano un oggetto `ClusterPaged`. L'invocazione di `advance_page()` ritorna una lista di cluster in quella pagina e avanza l'oggetto `ClusterPaged` alla pagina successiva. Questo può essere ripetuto fino a quando una eccezione `StopIteration` viene sollevata, indicando che non sono presenti altre pagine.

#### Esempio

Il seguente esempio mostra le proprietà di tutti i cluster presenti nella sottoscrizione corrente:

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### Cancellare un Cluster

Per cancellare un cluster:

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### Aggiornare i Tag di un Cluster

Puoi aggiornare i tag di uno specifico cluster in questo modo:

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### Esempio

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### Scalare un Cluster

Puoi scalare il numero di nodi worker di uno specifico cluster specificandone una nuova dimensione in questo modo:

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## Monitoraggio di un Cluster

HDInsight Management SDK può essere anche utilizzato per gestire il monitoraggio del tuo cluster tramite l'Operations Management Suite (OMS).

### Abilitare il Monitoraggio OMS

> [!NOTE]
> Per abilitare il monitoraggio OMS, devi avere un esistente workspace Log Analytics. Se non ne hai ancora creato uno, puoi imparare come farlo qui: [CCreare un'area di lavoro di Log Analytics nel portale di Azure](https://docs.microsoft.com/it-it/azure/log-analytics/log-analytics-quick-create-workspace).

Per abilitare il monitoraggio OMS sul tuo cluster:

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### Vedere lo Stato del Monitoraggio OMS

Per avere lo stato del monitoraggio OMS sul tuo cluster:

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### Disabilitare Monitoraggio OMS

Per disabilitare OMS sul tuo cluster:

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## Azioni Script 

HDInsight fornisce un metodo di configurazione chiamato azioni script che invoca script personalizzati per modificare il cluster.
> [!NOTE]
> Informazioni aggiuntive su come usare azioni script possono essere trovati qui: [Personalizzare i cluster HDInsight basati su Linux tramite azioni script](https://docs.microsoft.com/it-it/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### Eseguire Azioni Script
Per eseguire azioni script su uno specifico cluster:

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### Cancellare Azioni Script

Per cancellare una specifica azione script persistente su un cluster:

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### Elencare Azioni Script Persistenti

> [!NOTE]
> `list()` e `list_persisted_scripts()` ritornano un oggetto `RuntimeScriptActionDetailPaged`. L'invocazione di `advance_page()` ritorna una lista di `RuntimeScriptActionDetail` nella pagina e avanza l'oggetto `RuntimeScriptActionDetailPaged` alla pagina successiva. Questo può essere ripetuto fino a quando una eccezione `StopIteration` viene sollevata, indicando che non sono presenti altre pagine. Guarda l'esempio di seguito.

Per elencare tutte le azioni script persistenti su un cluster:
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### Esempio

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### Elencare lo Storico di Esecuzione di Tutti gli Script

Per elencare tutto lo storico di esecuzione di tutti gli script per un cluster:

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### Esempio

Questo esempio mostra tutti i dettagli delle esecuzioni precedenti degli script.

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
