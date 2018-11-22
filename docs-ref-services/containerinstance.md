---
title: Librerie di Istanze di contenitore di Azure per Python
description: Informazioni di riferimento sulle librerie di Istanze di contenitore di Azure per Python
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
# <a name="azure-container-instances-libraries-for-python"></a><span data-ttu-id="edb64-104">Librerie di Istanze di contenitore di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="edb64-104">Azure Container Instances libraries for Python</span></span>

<span data-ttu-id="edb64-105">Usare le librerie di Istanze di contenitore di Microsoft Azure per Python per creare e gestire istanze di contenitore di Azure.</span><span class="sxs-lookup"><span data-stu-id="edb64-105">Use the Microsoft Azure Container Instances libraries for Python to create and manage Azure container instances.</span></span> <span data-ttu-id="edb64-106">Per altre informazioni, vedere [Panoramica di Istanze di contenitore di Azure](/azure/container-instances/container-instances-overview).</span><span class="sxs-lookup"><span data-stu-id="edb64-106">Learn more by reading the [Azure Container Instances overview](/azure/container-instances/container-instances-overview).</span></span>

## <a name="management-apis"></a><span data-ttu-id="edb64-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="edb64-107">Management APIs</span></span>

<span data-ttu-id="edb64-108">Usare la libreria di gestione per creare e gestire istanze di contenitore di Azure in Azure.</span><span class="sxs-lookup"><span data-stu-id="edb64-108">Use the management library to create and manage Azure container instances in Azure.</span></span>

<span data-ttu-id="edb64-109">Installare il pacchetto di gestione tramite pip:</span><span class="sxs-lookup"><span data-stu-id="edb64-109">Install the management package via pip:</span></span>

```bash
pip install azure-mgmt-containerinstance
```

## <a name="example-source"></a><span data-ttu-id="edb64-110">Codice sorgente di esempio</span><span class="sxs-lookup"><span data-stu-id="edb64-110">Example source</span></span>

<span data-ttu-id="edb64-111">Se si preferisce visualizzarli nel contesto, gli esempi di codice riportati di seguito sono disponibili nel repository GitHub seguente:</span><span class="sxs-lookup"><span data-stu-id="edb64-111">If you'd like to see the following code examples in context, you can find them in the following GitHub repository:</span></span>

[<span data-ttu-id="edb64-112">Azure-Samples/aci-docs-sample-python</span><span class="sxs-lookup"><span data-stu-id="edb64-112">Azure-Samples/aci-docs-sample-python</span></span>](https://github.com/Azure-Samples/aci-docs-sample-python)

## <a name="authentication"></a><span data-ttu-id="edb64-113">Authentication</span><span class="sxs-lookup"><span data-stu-id="edb64-113">Authentication</span></span>

<span data-ttu-id="edb64-114">Per autenticare i client SDK, come i client di Resource Manager e Istanze di contenitore di Azure nell'esempio seguente, uno dei metodi più semplici è l'[autenticazione basata su file](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file).</span><span class="sxs-lookup"><span data-stu-id="edb64-114">One of the easiest ways to authenticate SDK clients (like the Azure Container Instances and Resource Manager clients in the following example) is with [file-based authentication](/python/azure/python-sdk-azure-authenticate#mgmt-auth-file).</span></span> <span data-ttu-id="edb64-115">Con l'autenticazione basata su file vengono eseguite query sulla variabile di ambiente `AZURE_AUTH_LOCATION` per individuare il percorso di un file delle credenziali.</span><span class="sxs-lookup"><span data-stu-id="edb64-115">File-based authentication queries the `AZURE_AUTH_LOCATION` environment variable for the path to a credentials file.</span></span> <span data-ttu-id="edb64-116">Per usare l'autenticazione basata su file:</span><span class="sxs-lookup"><span data-stu-id="edb64-116">To use file-based authentication:</span></span>

1. <span data-ttu-id="edb64-117">Creare un file delle credenziali con l'[interfaccia della riga di comando di Azure](/cli/azure) o [Cloud Shell](https://shell.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="edb64-117">Create a credentials file with the [Azure CLI](/cli/azure) or [Cloud Shell](https://shell.azure.com/):</span></span>

   `az ad sp create-for-rbac --sdk-auth > my.azureauth`

   <span data-ttu-id="edb64-118">Se si usa [Cloud Shell](https://shell.azure.com/) per generare il file delle credenziali, copiarne il contenuto in un file locale a cui l'applicazione Python può accedere.</span><span class="sxs-lookup"><span data-stu-id="edb64-118">If you use the [Cloud Shell](https://shell.azure.com/) to generate the credentials file, copy its contents into a local file that your Python application can access.</span></span>

2. <span data-ttu-id="edb64-119">Impostare la variabile di ambiente `AZURE_AUTH_LOCATION` sul percorso completo del file delle credenziali generato.</span><span class="sxs-lookup"><span data-stu-id="edb64-119">Set the `AZURE_AUTH_LOCATION` environment variable to the full path of the generated credentials file.</span></span> <span data-ttu-id="edb64-120">Ad esempio, in Bash:</span><span class="sxs-lookup"><span data-stu-id="edb64-120">For example (in Bash):</span></span>

   ```bash
   export AZURE_AUTH_LOCATION=/home/yourusername/my.azureauth
   ```

<span data-ttu-id="edb64-121">Dopo aver creato il file delle credenziali e aver popolato la variabile di ambiente `AZURE_AUTH_LOCATION`, usare il metodo `get_client_from_auth_file` del modulo [client_factory][client_factory] per inizializzare gli oggetti [ResourceManagementClient][ResourceManagementClient] e [ContainerInstanceManagementClient][ContainerInstanceManagementClient].</span><span class="sxs-lookup"><span data-stu-id="edb64-121">Once you've created the credentials file and populated the `AZURE_AUTH_LOCATION` environment variable, use the `get_client_from_auth_file` method of the [client_factory][client_factory] module to initialize the [ResourceManagementClient][ResourceManagementClient] and [ContainerInstanceManagementClient][ContainerInstanceManagementClient] objects.</span></span>

<span data-ttu-id="edb64-122"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]</span><span class="sxs-lookup"><span data-stu-id="edb64-122"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[authenticate](~/aci-docs-sample-python/src/aci_docs_sample.py#L45-L58 "Authenticate ACI and Resource Manager clients")]</span></span>

<span data-ttu-id="edb64-123">Per altri dettagli sui metodi di autenticazione disponibili nelle librerie di gestione Python per Azure, vedere [Eseguire l'autenticazione con le librerie di gestione di Azure per Python](/python/azure/python-sdk-azure-authenticate).</span><span class="sxs-lookup"><span data-stu-id="edb64-123">For more details about the available authentication methods in the Python management libraries for Azure, see [Authenticate with the Azure Management Libraries for Python](/python/azure/python-sdk-azure-authenticate).</span></span>

## <a name="create-container-group---single-container"></a><span data-ttu-id="edb64-124">Creare un gruppo di contenitori: singolo contenitore</span><span class="sxs-lookup"><span data-stu-id="edb64-124">Create container group - single container</span></span>

<span data-ttu-id="edb64-125">Questo esempio crea un gruppo di contenitori con un singolo contenitore.</span><span class="sxs-lookup"><span data-stu-id="edb64-125">This example creates a container group with a single container</span></span>

<span data-ttu-id="edb64-126"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]</span><span class="sxs-lookup"><span data-stu-id="edb64-126"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L94-L140 "Create single-container group")]</span></span>

## <a name="create-container-group---multiple-containers"></a><span data-ttu-id="edb64-127">Creare un gruppo di contenitori: più contenitori</span><span class="sxs-lookup"><span data-stu-id="edb64-127">Create container group - multiple containers</span></span>

<span data-ttu-id="edb64-128">Questo esempio crea un gruppo di contenitori con due contenitori: un contenitore di applicazioni e un contenitore collaterale.</span><span class="sxs-lookup"><span data-stu-id="edb64-128">This example creates a container group with two containers: an application container and a sidecar container.</span></span>

<span data-ttu-id="edb64-129"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]</span><span class="sxs-lookup"><span data-stu-id="edb64-129"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_multi](~/aci-docs-sample-python/src/aci_docs_sample.py#L143-L196 "Create multi-container group")]</span></span>

## <a name="create-task-based-container-group"></a><span data-ttu-id="edb64-130">Creare un gruppo di contenitori basato su attività</span><span class="sxs-lookup"><span data-stu-id="edb64-130">Create task-based container group</span></span>

<span data-ttu-id="edb64-131">Questo esempio crea un gruppo di contenitori con un singolo contenitore basato su attività.</span><span class="sxs-lookup"><span data-stu-id="edb64-131">This example creates a container group with a single task-based container.</span></span> <span data-ttu-id="edb64-132">L'esempio illustra diverse funzionalità:</span><span class="sxs-lookup"><span data-stu-id="edb64-132">This example demonstrates several features:</span></span>

* <span data-ttu-id="edb64-133">[Override della riga di comando](/azure/container-instances/container-instances-restart-policy#command-line-override). Viene specificata una riga di comando personalizzata, diversa da quella specificata nella riga `CMD` del Dockerfile del contenitore.</span><span class="sxs-lookup"><span data-stu-id="edb64-133">[Command line override](/azure/container-instances/container-instances-restart-policy#command-line-override) - A custom command line, different from that which is specified in the container's Dockerfile `CMD` line, is specified.</span></span> <span data-ttu-id="edb64-134">L'override della riga di comando consente di specificare una riga di comando personalizzata da eseguire all'avvio del contenitore in sostituzione di quella predefinita incorporata nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="edb64-134">Command line override allows you to specify a custom command line to execute at container startup, overriding the default command line baked-in to the container.</span></span> <span data-ttu-id="edb64-135">Per l'esecuzione di più comandi all'avvio del contenitore, si applica quanto segue:</span><span class="sxs-lookup"><span data-stu-id="edb64-135">Regarding executing multiple commands at container startup, the following applies:</span></span>

   <span data-ttu-id="edb64-136">Se si vuole eseguire un **singolo comando** con diversi argomenti della riga di comando, ad esempio `echo FOO BAR`, è necessario specificare gli argomenti come elenco di stringhe nella proprietà `command` del [contenitore][Container].</span><span class="sxs-lookup"><span data-stu-id="edb64-136">If you want to run a **single command** with several command-line arguments, for example `echo FOO BAR`, you must supply them as a string list to the `command` property of the [Container][Container].</span></span> <span data-ttu-id="edb64-137">Ad esempio: </span><span class="sxs-lookup"><span data-stu-id="edb64-137">For example:</span></span>

   `command = ['echo', 'FOO', 'BAR']`

   <span data-ttu-id="edb64-138">Se invece si vogliono eseguire **più comandi** con (potenzialmente) più argomenti, è necessario eseguire una shell e passare i comandi concatenati come argomento.</span><span class="sxs-lookup"><span data-stu-id="edb64-138">If, however, you want to run **multiple commands** with (potentially) multiple arguments, you must execute a shell and pass the chained commands as an argument.</span></span> <span data-ttu-id="edb64-139">Questo codice, ad esempio, esegue i comandi `echo` e `tail`:</span><span class="sxs-lookup"><span data-stu-id="edb64-139">For example, this executes both an `echo` and a `tail` command:</span></span>

   `command = ['/bin/sh', '-c', 'echo FOO BAR && tail -f /dev/null']`
* <span data-ttu-id="edb64-140">[Variabili di ambiente](/azure/container-instances/container-instances-environment-variables). Per il contenitore nel gruppo di contenitori vengono specificate due variabili di ambiente.</span><span class="sxs-lookup"><span data-stu-id="edb64-140">[Environment variables](/azure/container-instances/container-instances-environment-variables) - Two environment variables are specified for the container in the container group.</span></span> <span data-ttu-id="edb64-141">Usare le variabili di ambiente per modificare il comportamento degli script o delle applicazioni in fase di esecuzione o in alternativa passare informazioni dinamiche a un'applicazione eseguita nel contenitore.</span><span class="sxs-lookup"><span data-stu-id="edb64-141">Use environment variables to modify script or application behavior at runtime, or otherwise pass dynamic information to an application running in the container.</span></span>
* <span data-ttu-id="edb64-142">[Criteri di riavvio](/azure/container-instances/container-instances-restart-policy). Il contenitore viene configurato con il criterio di riavvio "Mai", utile per i contenitori basati su attività eseguiti come parte di un processo batch.</span><span class="sxs-lookup"><span data-stu-id="edb64-142">[Restart policy](/azure/container-instances/container-instances-restart-policy) - The container is configured with a restart policy of "Never," useful for task-based containers that are executed as part of a batch job.</span></span>
* <span data-ttu-id="edb64-143">Polling dell'operazione con [AzureOperationPoller][AzureOperationPoller]. Dopo la chiamata del metodo di creazione, viene eseguito il polling dell'operazione per determinare se è stata completata e se è possibile ottenere i log del gruppo di contenitori.</span><span class="sxs-lookup"><span data-stu-id="edb64-143">Operation polling with [AzureOperationPoller][AzureOperationPoller] - After the create method is invoked, the operation is polled to determine when it has completed and the container group's logs can be obtained.</span></span>

<span data-ttu-id="edb64-144"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]</span><span class="sxs-lookup"><span data-stu-id="edb64-144"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[create_container_group_task](~/aci-docs-sample-python/src/aci_docs_sample.py#L199-L275 "Run a task-based container")]</span></span>

## <a name="list-container-groups"></a><span data-ttu-id="edb64-145">Elencare i gruppi di contenitori</span><span class="sxs-lookup"><span data-stu-id="edb64-145">List container groups</span></span>

<span data-ttu-id="edb64-146">Questo esempio elenca i gruppi di contenitori in un gruppo di risorse e quindi stampa alcune proprietà.</span><span class="sxs-lookup"><span data-stu-id="edb64-146">This example lists the container groups in a resource group and then prints a few of their properties.</span></span>

<span data-ttu-id="edb64-147">Quando si elencano i gruppi di contenitori, il valore della proprietà [instance_view][instance_view] di ogni gruppo restituito è `None`.</span><span class="sxs-lookup"><span data-stu-id="edb64-147">When you list container groups,the [instance_view][instance_view] of each returned group is `None`.</span></span> <span data-ttu-id="edb64-148">Per ottenere i dettagli dei contenitori all'interno di un gruppo, è quindi necessario eseguire un'operazione [get][containergroupoperations_get] sul gruppo di contenitori, che restituirà il gruppo con la proprietà `instance_view` popolata.</span><span class="sxs-lookup"><span data-stu-id="edb64-148">To get the details of the containers within a container group, you must then [get][containergroupoperations_get] the container group, which returns the group with its `instance_view` property populated.</span></span> <span data-ttu-id="edb64-149">Per un esempio dell'iterazione sui contenitori di un gruppo in relazione alla proprietà `instance_view`, vedere la successiva sezione [Ottenere un gruppo di contenitori esistente](#get-an-existing-container-group).</span><span class="sxs-lookup"><span data-stu-id="edb64-149">See the next section, [Get an existing container group](#get-an-existing-container-group), for an example of iterating over a container group's containers in its `instance_view`.</span></span>

<span data-ttu-id="edb64-150"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]</span><span class="sxs-lookup"><span data-stu-id="edb64-150"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[list_container_groups](~/aci-docs-sample-python/src/aci_docs_sample.py#L278-L292 "List container groups")]</span></span>

## <a name="get-an-existing-container-group"></a><span data-ttu-id="edb64-151">Ottenere un gruppo di contenitori esistente</span><span class="sxs-lookup"><span data-stu-id="edb64-151">Get an existing container group</span></span>

<span data-ttu-id="edb64-152">Questo esempio ottiene un gruppo di contenitori specifico da un gruppo di risorse e quindi stampa alcune proprietà (inclusi i relativi contenitori) e i rispettivi valori.</span><span class="sxs-lookup"><span data-stu-id="edb64-152">This example gets a specific container group from a resource group, and then prints a few of its properties (including its containers) and their values.</span></span>

<span data-ttu-id="edb64-153">L'[operazione get][containergroupoperations_get] restituisce un gruppo di contenitori con la proprietà [instance_view][instance_view] popolata, con cui è possibile eseguire l'iterazione su ogni contenitore del gruppo.</span><span class="sxs-lookup"><span data-stu-id="edb64-153">The [get operation][containergroupoperations_get] returns a container group with its [instance_view][instance_view] populated, which allows you to iterate over each container in the group.</span></span> <span data-ttu-id="edb64-154">La proprietà `instance_vew` del gruppo di contenitori viene popolata solo dall'operazione `get`. Elencando i gruppi di contenitori in una sottoscrizione o in un gruppo di risorse non viene popolata la visualizzazione dell'istanza perché si tratta di un'operazione potenzialmente dispendiosa, ad esempio se l'elenco include centinaia di gruppi di contenitori in ognuno dei quali potrebbero essere presenti più contenitori.</span><span class="sxs-lookup"><span data-stu-id="edb64-154">Only the `get` operation populates the `instance_vew` property of the container group--listing the container groups in a subscription or resource group doesn't populate the instance view due to the potentially expensive nature of the operation (for example, when listing hundreds of container groups, each potentially containing multiple containers).</span></span> <span data-ttu-id="edb64-155">Come indicato in precedenza nella sezione [Elencare i gruppi di contenitori](#list-container-groups), dopo un'operazione `list` è necessario eseguire un'operazione `get` su uno specifico gruppo di contenitori per ottenere i dettagli delle istanze di contenitore.</span><span class="sxs-lookup"><span data-stu-id="edb64-155">As mentioned previously in the [List container groups](#list-container-groups) section, after a `list`, you must subsequently `get` a specific container group to obtain its container instance details.</span></span>

<span data-ttu-id="edb64-156"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]</span><span class="sxs-lookup"><span data-stu-id="edb64-156"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[get_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L295-L324 "Get container group")]</span></span>

## <a name="delete-a-container-group"></a><span data-ttu-id="edb64-157">Eliminare un gruppo di contenitori</span><span class="sxs-lookup"><span data-stu-id="edb64-157">Delete a container group</span></span>

<span data-ttu-id="edb64-158">Questo esempio elimina diversi gruppi di contenitori da un gruppo di risorse, nonché il gruppo di risorse.</span><span class="sxs-lookup"><span data-stu-id="edb64-158">This example deletes several container groups from a resource group, as well as the resource group.</span></span>

<span data-ttu-id="edb64-159"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]</span><span class="sxs-lookup"><span data-stu-id="edb64-159"><!-- SOURCE REPO: https://github.com/Azure-Samples/aci-docs-sample-python --> [!code-python[delete_container_group](~/aci-docs-sample-python/src/aci_docs_sample.py#L83-L91 "Delete container groups and resource group")]</span></span>

## <a name="next-steps"></a><span data-ttu-id="edb64-160">Passaggi successivi</span><span class="sxs-lookup"><span data-stu-id="edb64-160">Next steps</span></span>

* <span data-ttu-id="edb64-161">Il codice sorgente per gli esempi precedenti è disponibile in GitHub:</span><span class="sxs-lookup"><span data-stu-id="edb64-161">The source code for the preceding examples can be found on GitHub:</span></span>

  <span data-ttu-id="edb64-162">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span><span class="sxs-lookup"><span data-stu-id="edb64-162">[Azure-Samples/aci-docs-sample-python][aci-docs-sample-python]</span></span>

* <span data-ttu-id="edb64-163">Altri esempi di codice per Istanze di contenitore di Azure:</span><span class="sxs-lookup"><span data-stu-id="edb64-163">More Azure Container Instances code samples:</span></span>

  <span data-ttu-id="edb64-164">[Esempi di codice per Azure][samples-aci]</span><span class="sxs-lookup"><span data-stu-id="edb64-164">[Azure Code Samples][samples-aci]</span></span>

* <span data-ttu-id="edb64-165">Esplorare altro [codice Python di esempio][samples-python] utilizzabile nelle app.</span><span class="sxs-lookup"><span data-stu-id="edb64-165">Explore more [sample Python code][samples-python] you can use in your apps.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="edb64-166">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="edb64-166">Explore the management APIs</span></span>](/python/api/overview/azure/containerinstance/management)

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