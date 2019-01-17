---
title: Anteprima di Azure HDInsight Python SDK
description: Informazioni di riferimento per Azure HDInsight Python SDK. HDInsight Python SDK offre classi e metodi che consentono di gestire i cluster HDInsight.
ms.service: hdinsight
author: tylerfox
ms.author: tyfox
ms.date: 09/18/2018
ms.topic: reference
ms.devlang: python
ms.openlocfilehash: 9447d50fd734bd9221accbf470a456210bb57a7f
ms.sourcegitcommit: e2e4b1ecfac9804a72973477634128061c1ec990
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 12/17/2018
ms.locfileid: "53455108"
---
# <a name="hdinsight-python-management-sdk-preview"></a><span data-ttu-id="efe47-104">Anteprima di HDInsight Python Management SDK</span><span class="sxs-lookup"><span data-stu-id="efe47-104">HDInsight Python Management SDK Preview</span></span>

## <a name="overview"></a><span data-ttu-id="efe47-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="efe47-105">Overview</span></span>

<span data-ttu-id="efe47-106">HDInsight Python SDK offre classi e metodi che consentono di gestire i cluster HDInsight.</span><span class="sxs-lookup"><span data-stu-id="efe47-106">The HDInsight Python SDK provides classes and methods that allow you to manage your HDInsight clusters.</span></span> <span data-ttu-id="efe47-107">Include operazioni per creare, eliminare, aggiornare, elencare, ridimensionare, eseguire azioni di script, monitorare, ottenere le proprietà dei cluster di HDInsight e altro ancora.</span><span class="sxs-lookup"><span data-stu-id="efe47-107">It includes operations to create, delete, update, list, resize, execute script actions, monitor, get properties of HDInsight clusters, and more.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="efe47-108">Prerequisiti</span><span class="sxs-lookup"><span data-stu-id="efe47-108">Prerequisites</span></span>

* <span data-ttu-id="efe47-109">Un account Azure.</span><span class="sxs-lookup"><span data-stu-id="efe47-109">An Azure account.</span></span> <span data-ttu-id="efe47-110">Se non è disponibile, [ottenere una versione di valutazione gratuita](https://azure.microsoft.com/free/).</span><span class="sxs-lookup"><span data-stu-id="efe47-110">If you don't have one, [get a free trial](https://azure.microsoft.com/free/).</span></span>
* [<span data-ttu-id="efe47-111">Python</span><span class="sxs-lookup"><span data-stu-id="efe47-111">Python</span></span>](https://www.python.org/downloads/)
* [<span data-ttu-id="efe47-112">pip</span><span class="sxs-lookup"><span data-stu-id="efe47-112">pip</span></span>](https://pypi.org/project/pip/#description)

## <a name="sdk-installation"></a><span data-ttu-id="efe47-113">Installazione dell'SDK</span><span class="sxs-lookup"><span data-stu-id="efe47-113">SDK Installation</span></span>

<span data-ttu-id="efe47-114">HDInsight Python SDK è disponibile nell'[indice di pacchetti Python](https://pypi.org/project/azure-mgmt-hdinsight/) e può essere installato eseguendo questo comando:</span><span class="sxs-lookup"><span data-stu-id="efe47-114">The HDInsight Python SDK can be found in the [Python Package Index](https://pypi.org/project/azure-mgmt-hdinsight/) and can be installed by running:</span></span> 

`pip install azure-mgmt-hdinsight`

## <a name="authentication"></a><span data-ttu-id="efe47-115">Authentication</span><span class="sxs-lookup"><span data-stu-id="efe47-115">Authentication</span></span>

<span data-ttu-id="efe47-116">L'SDK deve essere prima autenticato con la sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="efe47-116">The SDK first needs to be authenticated with your Azure subscription.</span></span>  <span data-ttu-id="efe47-117">Seguire questo esempio per creare un'entità servizio e usarla per l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="efe47-117">Follow the example below to create a service principal and use it to authenticate.</span></span> <span data-ttu-id="efe47-118">Al termine si avrà un'istanza di un `HDInsightManagementClient` che contiene molti metodi, descritti nelle sezioni seguenti, che possono essere usati per operazioni di gestione.</span><span class="sxs-lookup"><span data-stu-id="efe47-118">After this is done, you will have an instance of an `HDInsightManagementClient`, which contains many methods (outlined in below sections) that can be used to perform management operations.</span></span>

> [!NOTE]
> <span data-ttu-id="efe47-119">Oltre all'esempio seguente esistono altre modalità di autenticazione che possono essere più adatte alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="efe47-119">There are other ways to authenticate besides the below example that could potentially be better suited for your needs.</span></span> <span data-ttu-id="efe47-120">Tutti i metodi sono descritti di seguito: [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="efe47-120">All methods are outlined here: [Authenticate with the Azure Management Libraries for Python](https://docs.microsoft.com/en-us/python/azure/python-sdk-azure-authenticate?view=azure-python)</span></span>

### <a name="authentication-example-using-a-service-principal"></a><span data-ttu-id="efe47-121">Esempio di autenticazione con un'entità servizio</span><span class="sxs-lookup"><span data-stu-id="efe47-121">Authentication Example Using a Service Principal</span></span>

<span data-ttu-id="efe47-122">Per prima cosa, accedere ad [Azure Cloud Shell](https://shell.azure.com/bash).</span><span class="sxs-lookup"><span data-stu-id="efe47-122">First, login to [Azure Cloud Shell](https://shell.azure.com/bash).</span></span> <span data-ttu-id="efe47-123">Verificare che si stia attualmente usando la sottoscrizione in cui si vuole creare l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="efe47-123">Verify you are currently using the subscription in which you want the service principal created.</span></span> 

```azurecli-interactive
az account show
```

<span data-ttu-id="efe47-124">Le informazioni sulla sottoscrizione vengono visualizzate in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="efe47-124">Your subscription information is displayed as JSON.</span></span>

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

<span data-ttu-id="efe47-125">Se non si è eseguito l'accesso alla sottoscrizione corretta, selezionare quella corretta eseguendo:</span><span class="sxs-lookup"><span data-stu-id="efe47-125">If you're not logged into the correct subscription, select the correct one by running:</span></span> 
```azurecli-interactive
az account set -s <name or ID of subscription>
```

> [!IMPORTANT]
> <span data-ttu-id="efe47-126">Se il provider di risorse HDInsight non è già stato registrato con un altro metodo, ad esempio creando un cluster HDInsight tramite il portale di Azure, è necessario eseguire questa operazione una volta prima di poter eseguire l'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="efe47-126">If you have not already registered the HDInsight Resource Provider by another method (such as by creating an HDInsight Cluster through the Azure Portal), you need to do this once before you can authenticate.</span></span> <span data-ttu-id="efe47-127">La registrazione può essere eseguita da [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo questo comando:</span><span class="sxs-lookup"><span data-stu-id="efe47-127">This can be done from the [Azure Cloud Shell](https://shell.azure.com/bash) by running the following command:</span></span>
>```azurecli-interactive
>az provider register --namespace Microsoft.HDInsight
>```

<span data-ttu-id="efe47-128">Scegliere quindi un nome per l'entità servizio e crearla con il comando seguente:</span><span class="sxs-lookup"><span data-stu-id="efe47-128">Next, choose a name for your service principal and create it with the following command:</span></span>

```azurecli-interactive
az ad sp create-for-rbac --name <Service Principal Name> --sdk-auth
```

<span data-ttu-id="efe47-129">Verranno visualizzate le informazioni relative all'entità servizio in formato JSON.</span><span class="sxs-lookup"><span data-stu-id="efe47-129">The service principal information is displayed as JSON.</span></span>

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
<span data-ttu-id="efe47-130">Copiare il frammento di codice Python seguente e compilare `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET` e `SUBSCRIPTION_ID` con le stringhe JSON restituite dopo l'esecuzione del comando per creare l'entità servizio.</span><span class="sxs-lookup"><span data-stu-id="efe47-130">Copy the below Python snippet and fill in `TENANT_ID`, `CLIENT_ID`, `CLIENT_SECRET`, and `SUBSCRIPTION_ID` with the strings from the JSON that was returned after running the command to create the service principal.</span></span>

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


## <a name="cluster-management"></a><span data-ttu-id="efe47-131">Gestione dei cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-131">Cluster Management</span></span>

> [!NOTE]
> <span data-ttu-id="efe47-132">Questa sezione presuppone che l'utente abbia già eseguito l'autenticazione e abbia creato un'istanza `HDInsightManagementClient` che ha poi archiviato in una variabile chiamata `client`.</span><span class="sxs-lookup"><span data-stu-id="efe47-132">This section assumes you have already authenticated and constructed an `HDInsightManagementClient` instance and store it in a variable called `client`.</span></span> <span data-ttu-id="efe47-133">Le istruzioni per l'autenticazione e l'ottenimento di un `HDInsightManagementClient` sono disponibili nella sezione Autenticazione precedente.</span><span class="sxs-lookup"><span data-stu-id="efe47-133">Instructions for authenticating and obtaining an `HDInsightManagementClient` can be found in the Authentication section above.</span></span>

### <a name="create-a-cluster"></a><span data-ttu-id="efe47-134">Creare un cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-134">Create a Cluster</span></span>

<span data-ttu-id="efe47-135">Un nuovo cluster può essere creato chiamando `client.clusters.create()`.</span><span class="sxs-lookup"><span data-stu-id="efe47-135">A new cluster can be created by calling `client.clusters.create()`.</span></span> 

#### <a name="example"></a><span data-ttu-id="efe47-136">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-136">Example</span></span>

<span data-ttu-id="efe47-137">Questo esempio illustra come creare un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="efe47-137">This example demonstrates how to create a Spark cluster with 2 head nodes and 1 worker node.</span></span>

> [!NOTE]
> <span data-ttu-id="efe47-138">È prima necessario creare un gruppo di risorse e un account di archiviazione, come spiegato di seguito.</span><span class="sxs-lookup"><span data-stu-id="efe47-138">You first need to create a Resource Group and Storage Account, as explained below.</span></span> <span data-ttu-id="efe47-139">Se sono già stati creati, è possibile ignorare questi passaggi.</span><span class="sxs-lookup"><span data-stu-id="efe47-139">If you have already created these, you can skip these steps.</span></span>

##### <a name="creating-a-resource-group"></a><span data-ttu-id="efe47-140">Creazione di un gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="efe47-140">Creating a Resource Group</span></span>

<span data-ttu-id="efe47-141">È possibile creare un gruppo di risorse con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo</span><span class="sxs-lookup"><span data-stu-id="efe47-141">You can create a resource group using the [Azure Cloud Shell](https://shell.azure.com/bash) by running</span></span>
```azurecli-interactive
az group create -l <Region Name (i.e. eastus)> --n <Resource Group Name>
```
##### <a name="creating-a-storage-account"></a><span data-ttu-id="efe47-142">Creazione di un account di archiviazione</span><span class="sxs-lookup"><span data-stu-id="efe47-142">Creating a Storage Account</span></span>

<span data-ttu-id="efe47-143">È possibile creare un account di archiviazione con [Azure Cloud Shell](https://shell.azure.com/bash) eseguendo:</span><span class="sxs-lookup"><span data-stu-id="efe47-143">You can create a storage account using the [Azure Cloud Shell](https://shell.azure.com/bash) by running:</span></span>
```azurecli-interactive
az storage account create -n <Storage Account Name> -g <Existing Resource Group Name> -l <Region Name (i.e. eastus)> --sku <SKU i.e. Standard_LRS>
```
<span data-ttu-id="efe47-144">Eseguire ora questo comando per ottenere la chiave per l'account di archiviazione. Questa chiave sarà necessaria per creare un cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-144">Now run the following command to get the key for your storage account (you will need this to create a cluster):</span></span>
```azurecli-interactive
az storage account keys list -n <Storage Account Name>
```
---
<span data-ttu-id="efe47-145">Il frammento di codice Python seguente crea un cluster Spark con 2 nodi head e 1 nodo del ruolo di lavoro.</span><span class="sxs-lookup"><span data-stu-id="efe47-145">The below Python snippet creates a Spark cluster with 2 head nodes and 1 worker node.</span></span> <span data-ttu-id="efe47-146">Inserire le variabili vuote come spiegato nei commenti. È possibile modificare altri parametri in base alle proprie esigenze.</span><span class="sxs-lookup"><span data-stu-id="efe47-146">Fill in the blank variables as explained in the comments and feel free to change other parameters to suit your specific needs.</span></span>

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

### <a name="get-cluster-details"></a><span data-ttu-id="efe47-147">Ottenere i dettagli del cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-147">Get Cluster Details</span></span>

<span data-ttu-id="efe47-148">Per ottenere le proprietà di un dato cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-148">To get properties for a given cluster:</span></span>

```python
client.clusters.get("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="efe47-149">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-149">Example</span></span>

<span data-ttu-id="efe47-150">È possibile usare `get` per verificare che il cluster sia stato creato correttamente.</span><span class="sxs-lookup"><span data-stu-id="efe47-150">You can use `get` to confirm that you have successfully created your cluster.</span></span>

```python
my_cluster = client.clusters.get("<Resource Group Name>", "<Cluster Name>")
print(my_cluster)
```

<span data-ttu-id="efe47-151">L'output sarà simile al seguente:</span><span class="sxs-lookup"><span data-stu-id="efe47-151">The output should look like:</span></span>

```
{'additional_properties': {}, 'id': '/subscriptions/XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX/resourceGroups/<Resource Group Name>/providers/Microsoft.HDInsight/clusters/<Cluster Name>', 'name': '<Cluster Name>', 'type': 'Microsoft.HDInsight/clusters', 'location': '<Location>', 'tags': {}, 'etag': 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX', 'properties': <azure.mgmt.hdinsight.models.cluster_get_properties_py3.ClusterGetProperties object at 0x0000013766D68048>}
```

### <a name="list-clusters"></a><span data-ttu-id="efe47-152">Elencare cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-152">List Clusters</span></span>

#### <a name="list-clusters-under-the-subscription"></a><span data-ttu-id="efe47-153">Elencare i cluster nella sottoscrizione</span><span class="sxs-lookup"><span data-stu-id="efe47-153">List Clusters Under The Subscription</span></span>

```python
client.clusters.list()
```
#### <a name="list-clusters-by-resource-group"></a><span data-ttu-id="efe47-154">Elencare i cluster per gruppo di risorse</span><span class="sxs-lookup"><span data-stu-id="efe47-154">List Clusters By Resource Group</span></span>

```python
client.clusters.list_by_resource_group("<Resource Group Name>")
```
> [!NOTE]
> <span data-ttu-id="efe47-155">Sia `list()` che `list_by_resource_group()` restituiscono un oggetto `ClusterPaged`.</span><span class="sxs-lookup"><span data-stu-id="efe47-155">Both `list()` and `list_by_resource_group()` return an `ClusterPaged` object.</span></span> <span data-ttu-id="efe47-156">La chiamata di `advance_page()` restituisce un elenco di cluster nella pagina e determina l'avanzamento dell'oggetto `ClusterPaged` alla pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="efe47-156">Calling `advance_page()` returns the a list of clusters on that page and advances the `ClusterPaged` object to the next page.</span></span> <span data-ttu-id="efe47-157">Questa operazione può essere ripetuta fino alla generazione di un'eccezione `StopIteration`, che indica che non sono presenti altre pagine.</span><span class="sxs-lookup"><span data-stu-id="efe47-157">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span>

#### <a name="example"></a><span data-ttu-id="efe47-158">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-158">Example</span></span>

<span data-ttu-id="efe47-159">L'esempio seguente mostra le proprietà di tutti i cluster per la sottoscrizione corrente:</span><span class="sxs-lookup"><span data-stu-id="efe47-159">The following example prints the properties of all clusters for the current subscription:</span></span>

```python
clusters_paged = client.clusters.list()
while True:
  try:
    for cluster in clusters_paged.advance_page():
      print(cluster)
  except StopIteration: 
    break
```

### <a name="delete-a-cluster"></a><span data-ttu-id="efe47-160">Eliminare un cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-160">Delete a Cluster</span></span>

<span data-ttu-id="efe47-161">Per eliminare un cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-161">To delete a cluster:</span></span>

```python
client.clusters.delete("<Resource Group Name>", "<Cluster Name>")
```

### <a name="update-cluster-tags"></a><span data-ttu-id="efe47-162">Aggiornare i tag del cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-162">Update Cluster Tags</span></span>

<span data-ttu-id="efe47-163">È possibile aggiornare i tag di un dato cluster nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="efe47-163">You can update the tags of a given cluster like so:</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={<Dictionary of Tags>})
```

#### <a name="example"></a><span data-ttu-id="efe47-164">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-164">Example</span></span>

```python
client.clusters.update("<Resource Group Name>", "<Cluster Name>", tags={"tag1Name" : "tag1Value", "tag2Name" : "tag2Value"})
```

### <a name="resize-cluster"></a><span data-ttu-id="efe47-165">Ridimensionare un cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-165">Resize Cluster</span></span>

<span data-ttu-id="efe47-166">È possibile ridimensionare il numero di nodi di ruolo di lavoro di un dato cluster specificando una nuova dimensione nel modo seguente:</span><span class="sxs-lookup"><span data-stu-id="efe47-166">You can resize a given cluster's number of worker nodes by specifying a new size like so:</span></span>

```python
client.clusters.resize("<Resource Group Name>", "<Cluster Name>", target_instance_count=<Num of Worker Nodes>)
```

## <a name="cluster-monitoring"></a><span data-ttu-id="efe47-167">Monitoraggio del cluster</span><span class="sxs-lookup"><span data-stu-id="efe47-167">Cluster Monitoring</span></span>

<span data-ttu-id="efe47-168">HDInsight Management SDK può essere usato anche per gestire il monitoraggio dei cluster tramite Operations Management Suite (OMS).</span><span class="sxs-lookup"><span data-stu-id="efe47-168">The HDInsight Management SDK can also be used to manage monitoring on your clusters via the Operations Management Suite (OMS).</span></span>

### <a name="enable-oms-monitoring"></a><span data-ttu-id="efe47-169">Abilitare il monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="efe47-169">Enable OMS Monitoring</span></span>

> [!NOTE]
> <span data-ttu-id="efe47-170">Per abilitare il monitoraggio di OMS è necessaria un'area di lavoro di Log Analytics esistente.</span><span class="sxs-lookup"><span data-stu-id="efe47-170">To enable OMS Monitoring, you must have an existing Log Analytics workspace.</span></span> <span data-ttu-id="efe47-171">Se non è già stata creata una, è possibile ottenere informazioni su come farlo qui: [Creare un'area di lavoro di Log Analytics nel portale di Azure](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span><span class="sxs-lookup"><span data-stu-id="efe47-171">If you have not already created one, you can learn how to do that here: [Create a Log Analytics workspace in the Azure portal](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-quick-create-workspace).</span></span>

<span data-ttu-id="efe47-172">Per abilitare il monitoraggio di OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-172">To enable OMS Monitoring on your cluster:</span></span>

```python
client.extension.enable_monitoring("<Resource Group Name>", "<Cluster Name>", workspace_id="<Workspace Id>")
```

### <a name="view-status-of-oms-monitoring"></a><span data-ttu-id="efe47-173">Visualizzare lo stato del monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="efe47-173">View Status Of OMS Monitoring</span></span>

<span data-ttu-id="efe47-174">Per ottenere lo stato di OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-174">To get the status of OMS on your cluster:</span></span>

```python
client.extension.get_monitoring_status("<Resource Group Name", "Cluster Name")
```

### <a name="disable-oms-monitoring"></a><span data-ttu-id="efe47-175">Disabilitare il monitoraggio di OMS</span><span class="sxs-lookup"><span data-stu-id="efe47-175">Disable OMS Monitoring</span></span>

<span data-ttu-id="efe47-176">Per disabilitare OMS nel cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-176">To disable OMS on your cluster:</span></span>

```python
client.extension.disable_monitoring("<Resource Group Name>", "<Cluster Name>")
```

## <a name="script-actions"></a><span data-ttu-id="efe47-177">Azioni script</span><span class="sxs-lookup"><span data-stu-id="efe47-177">Script Actions</span></span>

<span data-ttu-id="efe47-178">HDInsight offre un metodo di configurazione denominato "azioni script" che richiama script personalizzati per il cluster.</span><span class="sxs-lookup"><span data-stu-id="efe47-178">HDInsight provides a configuration method called script actions that invokes custom scripts to customize the cluster.</span></span>
> [!NOTE]
> <span data-ttu-id="efe47-179">Altre informazioni su come usare le azioni dello script sono disponibili qui: [Personalizzare i cluster HDInsight basati su Linux tramite azioni script](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span><span class="sxs-lookup"><span data-stu-id="efe47-179">More information on how to use script actions can be found here: [Customize Linux-based HDInsight clusters using script actions](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)</span></span>

### <a name="execute-script-actions"></a><span data-ttu-id="efe47-180">Eseguire azioni script</span><span class="sxs-lookup"><span data-stu-id="efe47-180">Execute Script Actions</span></span>
<span data-ttu-id="efe47-181">Per eseguire azioni script in un cluster specifico:</span><span class="sxs-lookup"><span data-stu-id="efe47-181">To execute script actions on a given cluster:</span></span>

```python
script_action1 = RuntimeScriptAction(name="<Script Name>", uri="<URL To Script>", roles=[<List of Roles>]) #valid roles are "headnode", "workernode", "zookeepernode", and "edgenode"

client.clusters.execute_script_actions("<Resource Group Name>", "<Cluster Name>", <persist_on_success (bool)>, script_actions=[script_action1]) #add more RuntimeScriptActions to the list to execute multiple scripts
```

### <a name="delete-script-action"></a><span data-ttu-id="efe47-182">Eliminare un'azione script</span><span class="sxs-lookup"><span data-stu-id="efe47-182">Delete Script Action</span></span>

<span data-ttu-id="efe47-183">Per eliminare una determinata azione script persistente in un dato cluster:</span><span class="sxs-lookup"><span data-stu-id="efe47-183">To delete a specified persisted script action on a given cluster:</span></span>

```python
client.script_actions.delete("<Resource Group Name>", "<Cluster Name", "<Script Name>")
```

### <a name="list-persisted-script-actions"></a><span data-ttu-id="efe47-184">Elencare le azioni script persistenti</span><span class="sxs-lookup"><span data-stu-id="efe47-184">List Persisted Script Actions</span></span>

> [!NOTE]
> <span data-ttu-id="efe47-185">`list()` e `list_persisted_scripts()` restituiscono un oggetto `RuntimeScriptActionDetailPaged`.</span><span class="sxs-lookup"><span data-stu-id="efe47-185">`list()` and `list_persisted_scripts()` return a `RuntimeScriptActionDetailPaged` object.</span></span> <span data-ttu-id="efe47-186">La chiamata di `advance_page()` restituisce un elenco di `RuntimeScriptActionDetail` nella pagina e determina l'avanzamento dell'oggetto `RuntimeScriptActionDetailPaged` alla pagina successiva.</span><span class="sxs-lookup"><span data-stu-id="efe47-186">Calling `advance_page()` returns the a list of `RuntimeScriptActionDetail` on that page and advances the `RuntimeScriptActionDetailPaged` object to the next page.</span></span> <span data-ttu-id="efe47-187">Questa operazione può essere ripetuta fino alla generazione di un'eccezione `StopIteration`, che indica che non sono presenti altre pagine.</span><span class="sxs-lookup"><span data-stu-id="efe47-187">This can be repeated until a `StopIteration` exception is raised, indicating that there are no more pages.</span></span> <span data-ttu-id="efe47-188">Vedere l'esempio seguente.</span><span class="sxs-lookup"><span data-stu-id="efe47-188">See the example below.</span></span>

<span data-ttu-id="efe47-189">Per elencare tutte le azioni script persistenti per il cluster specificato:</span><span class="sxs-lookup"><span data-stu-id="efe47-189">To list all persisted script actions for the specified cluster:</span></span>
```python
client.script_actions.list_persisted_scripts("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="efe47-190">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-190">Example</span></span>

```python
scripts_paged = client.script_actions.list_persisted_scripts(resource_group_name, cluster_name)
while True:
  try:
    for script in scripts_paged.advance_page():
      print(script)
  except StopIteration:
    break
```

### <a name="list-all-scripts-execution-history"></a><span data-ttu-id="efe47-191">Elencare la cronologia di esecuzione di tutti gli script</span><span class="sxs-lookup"><span data-stu-id="efe47-191">List All Scripts' Execution History</span></span>

<span data-ttu-id="efe47-192">Per elencare la cronologia di esecuzione di tutti gli script per il cluster specificato:</span><span class="sxs-lookup"><span data-stu-id="efe47-192">To list all scripts' execution history for the specified cluster:</span></span>

```python
client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
```

#### <a name="example"></a><span data-ttu-id="efe47-193">Esempio</span><span class="sxs-lookup"><span data-stu-id="efe47-193">Example</span></span>

<span data-ttu-id="efe47-194">Questo esempio visualizza tutti i dettagli di tutte le precedenti esecuzioni di script.</span><span class="sxs-lookup"><span data-stu-id="efe47-194">This example prints all the details for all past script executions.</span></span>

```python
script_executions_paged = client.script_execution_history.list("<Resource Group Name>", "<Cluster Name>")
while True:
  try:
    for script in script_executions_paged.advance_page():            
      print(script)
    except StopIteration:       
      break
```