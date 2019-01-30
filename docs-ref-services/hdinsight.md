---
title: Anteprima di Azure HDInsight Python SDK
description: Informazioni di riferimento per Azure HDInsight Python SDK. HDInsight Python SDK offre classi e metodi che consentono di gestire i cluster HDInsight.
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 8d081739a3984e1cd3f7bbf31fcb44d63cfb6947
ms.sourcegitcommit: fba77bdf8eb9f49621be94544d9fef88aff98c14
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/23/2019
ms.locfileid: "54747711"
---
# <a name="hdinsight-python-management-sdk-preview"></a>Anteprima di HDInsight Python Management SDK

## <a name="overview"></a>Panoramica

HDInsight Python SDK offre classi e metodi che consentono di gestire i cluster HDInsight. Include operazioni per creare, eliminare, aggiornare, elencare, ridimensionare, eseguire azioni di script, monitorare, ottenere le proprietà dei cluster di HDInsight e altro ancora.

## <a name="prerequisites"></a>Prerequisiti

* Un account Azure. Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).
* [Python](https://www.python.org/downloads/)
* [pip](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a>Installazione dell'SDK

HDInsight Python SDK è disponibile nell'[indice di pacchetti Python](https://pypi.org/project/azure-mgmt-hdinsight/) e può essere installato eseguendo questo comando: 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a>Authentication

L'SDK deve essere prima autenticato con la sottoscrizione di Azure.  Seguire questo esempio per creare un'entità servizio e usarla per l'autenticazione. Al termine si avrà un'istanza di un `HDInsightManagementClient` che contiene molti metodi, descritti nelle sezioni seguenti, che possono essere usati per operazioni di gestione.

> [!NOTE]
> Oltre all'esempio seguente esistono altre modalità di autenticazione che possono essere più adatte alle proprie esigenze. Tutti i metodi sono descritti di seguito: [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)

### <a name="authentication-example-using-a-service-principal"></a>Esempio di autenticazione con un'entità servizio

Per prima cosa, accedere ad [Azure Cloud Shell](https://shell.azure.com/bash). Verificare che si stia attualmente usando la sottoscrizione in cui si vuole creare l'entità servizio. 

```azurecli-interactive
az account show
```

Le informazioni sulla sottoscrizione vengono visualizzate in formato JSON.

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

Se non si è eseguito l'accesso alla sottoscrizione corretta, selezionare quella corretta eseguendo: 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> Se il provider di risorse HDInsight non è già stato registrato con un altro metodo, ad esempio creando un cluster HDInsight tramite il portale di Azure, è necessario eseguire questa operazione una volta prima di poter eseguire l'autenticazione. La registrazione può essere eseguita da [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo questo comando:
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

Scegliere quindi un nome per l'entità servizio e crearla con il comando seguente:

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

Verranno visualizzate le informazioni relative all'entità servizio in formato JSON.

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
Copiare il frammento di codice Python seguente e compilare `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` e `SUBSCRIPTION_ID` con le stringhe JSON restituite dopo l'esecuzione del comando per creare l'entità servizio.

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


## <a name="cluster-management"></a>Gestione dei cluster

> [!NOTE]
> Questa sezione presuppone che l'utente abbia già eseguito l'autenticazione e abbia creato un'istanza `HDInsightManagementClient` che ha poi archiviato in una variabile chiamata `client`. Le istruzioni per l'autenticazione e l'ottenimento di un `HDInsightManagementClient` sono disponibili nella sezione Autenticazione precedente.

### <a name="create-a-cluster"></a>Creare un cluster

Un nuovo cluster può essere creato chiamando `client.clusters.create()`. 

#### <a name="example"></a>Esempio

Questo esempio illustra come creare un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro.

> [!NOTE]
> È prima necessario creare un gruppo di risorse e un account di archiviazione, come spiegato di seguito. Se sono già stati creati, è possibile ignorare questi passaggi.

##### <a name="creating-a-resource-group"></a>Creazione di un gruppo di risorse

È possibile creare un gruppo di risorse con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

È possibile creare un account di archiviazione con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo:
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
Eseguire ora questo comando per ottenere la chiave per l'account di archiviazione. Questa chiave sarà necessaria per creare un cluster:
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
Il frammento di codice Python seguente crea un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro. Inserire le variabili vuote come spiegato nei commenti. È possibile modificare altri parametri in base alle proprie esigenze.

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

### <a name="get-cluster-details"></a>Ottenere i dettagli del cluster

Per ottenere le proprietà di un dato cluster:

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Esempio

È possibile usare `get` per verificare che il cluster sia stato creato correttamente.

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

L'output sarà simile al seguente:

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a>Elencare cluster

#### <a name="list-clusters-under-the-subscription"></a>Elencare i cluster nella sottoscrizione

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a>Elencare i cluster per gruppo di risorse

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> Sia `list()` che `list_by_resource_group()` restituiscono un oggetto `ClusterPaged`. La chiamata di `advance_page()` restituisce un elenco di cluster nella pagina e determina l'avanzamento dell'oggetto `ClusterPaged` alla pagina successiva. Questa operazione può essere ripetuta fino alla generazione di un'eccezione `StopIteration`, che indica che non sono presenti altre pagine.

#### <a name="example"></a>Esempio

L'esempio seguente mostra le proprietà di tutti i cluster per la sottoscrizione corrente:

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a>Eliminare un cluster

Per eliminare un cluster:

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a>Aggiornare i tag del cluster

È possibile aggiornare i tag di un dato cluster nel modo seguente:

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a>Esempio

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a>Ridimensionare un cluster

È possibile ridimensionare il numero di nodi di ruolo di lavoro di un dato cluster specificando una nuova dimensione nel modo seguente:

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a>Monitoraggio del cluster

HDInsight Management SDK può essere usato anche per gestire il monitoraggio dei cluster tramite Operations Management Suite (OMS).

### <a name="enable-oms-monitoring"></a>Abilitare il monitoraggio di OMS

> [!NOTE]
> Per abilitare il monitoraggio di OMS è necessaria un'area di lavoro di Log Analytics esistente. Se non è già stata creata una, è possibile ottenere informazioni su come farlo qui: [Creare un'area di lavoro di Log Analytics nel portale di Azure](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).

Per abilitare il monitoraggio di OMS nel cluster:

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a>Visualizzare lo stato del monitoraggio di OMS

Per ottenere lo stato di OMS nel cluster:

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a>Disabilitare il monitoraggio di OMS

Per disabilitare OMS nel cluster:

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a>Azioni script

HDInsight offre un metodo di configurazione denominato "azioni script" che richiama script personalizzati per il cluster.
> [!NOTE]
> Altre informazioni su come usare le azioni dello script sono disponibili qui: [Personalizzare i cluster HDInsight basati su Linux tramite azioni script](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)

### <a name="execute-script-actions"></a>Eseguire azioni script
Per eseguire azioni script in un cluster specifico:

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a>Eliminare un'azione script

Per eliminare una determinata azione script persistente in un dato cluster:

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a>Elencare le azioni script persistenti

> [!NOTE]
> `list()` e `list_persisted_scripts()` restituiscono un oggetto `RuntimeScriptActionDetailPaged`. La chiamata di `advance_page()` restituisce un elenco di `RuntimeScriptActionDetail` nella pagina e determina l'avanzamento dell'oggetto `RuntimeScriptActionDetailPaged` alla pagina successiva. Questa operazione può essere ripetuta fino alla generazione di un'eccezione `StopIteration`, che indica che non sono presenti altre pagine. Vedere l'esempio seguente.

Per elencare tutte le azioni script persistenti per il cluster specificato:
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Esempio

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a>Elencare la cronologia di esecuzione di tutti gli script

Per elencare la cronologia di esecuzione di tutti gli script per il cluster specificato:

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a>Esempio

Questo esempio visualizza tutti i dettagli di tutte le precedenti esecuzioni di script.

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```
