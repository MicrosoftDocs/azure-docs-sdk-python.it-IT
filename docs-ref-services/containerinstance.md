---
title: Librerie di Istanze di Azure Container per Python
description: Informazioni di riferimento sulle librerie di Istanze di Azure Container per Python
keywords: Azure, Python, SDK, API, Istanze di contenitore di Azure, contenitore, istanze
author: mmacy
manager: jeconnoc
ms.date: 06/04/2018
ms.author: marsma
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: container-instances
ms.openlocfilehash: 95571e0da6ef82ef045d8c9ba0a5beb0abe9b63a
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52273017"
---
# <a name="azure-container-instances-libraries-for-python"></a>Librerie di Istanze di Azure Container per Python

Usare le librerie di Istanze di Container di Microsoft Azure per Python per creare e gestire Istanze di Azure Container. Per altre informazioni, vedere [Panoramica di Istanze di Azure Container](/azure/container-instances/container-instances-overview).

## <a name="management-apis"></a>API di gestione

È possibile usare la libreria di gestione per creare e gestire Istanze di Azure Container in Azure.

Installare il pacchetto di gestione tramite pip:

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a>Codice sorgente di esempio

Se si preferisce visualizzarli nel contesto, gli esempi di codice riportati di seguito sono disponibili nel repository GitHub seguente:

[Azure-Samples/aci-docs-sample-python](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a>Authentication

Per autenticare i client SDK, come i client di Resource Manager e Istanze di Azure Container nell'esempio seguente, uno dei metodi più semplici è l'[autenticazione basata su file](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file). Con l'autenticazione basata su file vengono eseguite query sulla variabile di ambiente `AZURE_AUTH_LOCATION` per individuare il percorso di un file delle credenziali. Per usare l'autenticazione basata su file:

1. Creare un file delle credenziali con l'[interfaccia della riga di comando di Azure](/cli/azure) o [Cloud Shell](https://shell.azure.com/):

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   Se si usa [Cloud Shell](https://shell.azure.com/) per generare il file delle credenziali, copiarne il contenuto in un file locale a cui l'applicazione Python può accedere.

2. Impostare la variabile di ambiente `AZURE_AUTH_LOCATION` sul percorso completo del file delle credenziali generato. Ad esempio, in Bash:

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

Dopo aver creato il file delle credenziali e aver popolato la variabile di ambiente `AZURE_AUTH_LOCATION`, usare il metodo `get_client_from_auth_file` del modulo [client_factory][client_factory] per inizializzare gli oggetti [ResourceManagementClient][ResourceManagementClient] e [ContainerInstanceManagementClient][ContainerInstanceManagementClient].

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]

Per altri dettagli sui metodi di autenticazione disponibili nelle librerie di gestione Python per Azure, vedere [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](/python/azure/python-sdk-azure-authenticate).

## <a name="create-container-group---single-container"></a>Creare un gruppo di contenitori: singolo contenitore

Questo esempio crea un gruppo di contenitori con un singolo contenitore.

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]

## <a name="create-container-group---multiple-containers"></a>Creare un gruppo di contenitori: più contenitori

Questo esempio crea un gruppo di contenitori con due contenitori: un contenitore di applicazioni e un contenitore collaterale.

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]

## <a name="create-task-based-container-group"></a>Creare un gruppo di contenitori basato su attività

Questo esempio crea un gruppo di contenitori con un singolo contenitore basato su attività. L'esempio illustra diverse funzionalità:

* [Override della riga di comando](/azure/container-instances/container-instances-restart-policy#command-line-override). Viene specificata una riga di comando personalizzata, diversa da quella specificata nella riga `CMD` del Dockerfile del contenitore. L'override della riga di comando consente di specificare una riga di comando personalizzata da eseguire all'avvio del contenitore in sostituzione di quella predefinita incorporata nel contenitore. Per l'esecuzione di più comandi all'avvio del contenitore, si applica quanto segue:

   Se si vuole eseguire un **singolo comando** con diversi argomenti della riga di comando, ad esempio `echo FOO BAR`, è necessario specificare gli argomenti come elenco di stringhe nella proprietà `command` del [contenitore][Container]. Ad esempio: 

   `command = ['echo', 'FOO', 'BAR']`

   Se invece si vogliono eseguire **più comandi** con (potenzialmente) più argomenti, è necessario eseguire una shell e passare i comandi concatenati come argomento. Questo codice, ad esempio, esegue i comandi `echo` e `tail`:

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* [Variabili di ambiente](/azure/container-instances/container-instances-environment-variables). Per il contenitore nel gruppo di contenitori vengono specificate due variabili di ambiente. Usare le variabili di ambiente per modificare il comportamento degli script o delle applicazioni in fase di esecuzione o in alternativa passare informazioni dinamiche a un'applicazione eseguita nel contenitore.
* [Criteri di riavvio](/azure/container-instances/container-instances-restart-policy). Il contenitore viene configurato con il criterio di riavvio "Mai", utile per i contenitori basati su attività eseguiti come parte di un processo batch.
* Polling dell'operazione con [AzureOperationPoller][AzureOperationPoller]. Dopo la chiamata del metodo di creazione, viene eseguito il polling dell'operazione per determinare se è stata completata e se è possibile ottenere i log del gruppo di contenitori.

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]

## <a name="list-container-groups"></a>Elencare i gruppi di contenitori

Questo esempio elenca i gruppi di contenitori in un gruppo di risorse e quindi stampa alcune proprietà.

Quando si elencano i gruppi di contenitori, il valore della proprietà [instance_view][instance_view] di ogni gruppo restituito è `None`. Per ottenere i dettagli dei contenitori all'interno di un gruppo, è quindi necessario eseguire un'operazione [get][containergroupoperations_get] sul gruppo di contenitori, che restituirà il gruppo con la proprietà `instance_view` popolata. Per un esempio dell'iterazione sui contenitori di un gruppo in relazione alla proprietà `instance_view`, vedere la successiva sezione [Ottenere un gruppo di contenitori esistente](#get-an-existing-container-group).

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]

## <a name="get-an-existing-container-group"></a>Ottenere un gruppo di contenitori esistente

Questo esempio ottiene un gruppo di contenitori specifico da un gruppo di risorse e quindi stampa alcune proprietà (inclusi i relativi contenitori) e i rispettivi valori.

L'[operazione get][containergroupoperations_get] restituisce un gruppo di contenitori con la proprietà [instance_view][instance_view] popolata, con cui è possibile eseguire l'iterazione su ogni contenitore del gruppo. La proprietà `instance_vew` del gruppo di contenitori viene popolata solo dall'operazione `get`. Elencando i gruppi di contenitori in una sottoscrizione o in un gruppo di risorse non viene popolata la visualizzazione dell'istanza perché si tratta di un'operazione potenzialmente dispendiosa, ad esempio se l'elenco include centinaia di gruppi di contenitori in ognuno dei quali potrebbero essere presenti più contenitori. Come indicato in precedenza nella sezione [Elencare i gruppi di contenitori](#list-container-groups), dopo un'operazione `list` è necessario eseguire un'operazione `get` su uno specifico gruppo di contenitori per ottenere i dettagli delle istanze di contenitore.

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]

## <a name="delete-a-container-group"></a>Eliminare un gruppo di contenitori

Questo esempio elimina diversi gruppi di contenitori da un gruppo di risorse, nonché il gruppo di risorse.

<!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]

## <a name="next-steps"></a>Passaggi successivi

* Il codice sorgente per gli esempi precedenti è disponibile in GitHub:

  [Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]

* Altri esempi di codice per Istanze di Azure Container:

  [Esempi di codice per Azure][samples-aci]

* Esplorare altro [codice Python di esempio][samples-python] utilizzabile nelle app.

> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/containerinstance/management)

<!-- LINKS - External -->
[aci-docs-sample-python]: https://github.com/Azure-Samples/aci-docs-sample-python
[samples-aci]: https://azure.microsoft.com/resources/samples/?sort=0&term=ACI
[samples-python]: https://azure.microsoft.com/resources/samples/?platform=python

<!-- TYPES -->
[AzureOperationPoller]: /python/api/msrestazure.azure_operation.AzureOperationPoller
[client_factory]: /python/api/azure.common.client_factory
[Container]: /python/api/azure.mgmt.containerinstance.models.container
[ContainerGroupInstanceView]: /python/api/azure.mgmt.containerinstance.models.containergrouppropertiesinstanceview
[containergroupoperations_get]: /python/api/azure.mgmt.containerinstance.operations.containergroupsoperations#get
[ContainerInstanceManagementClient]: /python/api/azure.mgmt.containerinstance.containerinstancemanagementclient
[instance_view]: /python/api/azure.mgmt.containerinstance.models.containergroup#variables
[ResourceManagementClient]: /python/api/azure.mgmt.resource.resources.resourcemanagementclient