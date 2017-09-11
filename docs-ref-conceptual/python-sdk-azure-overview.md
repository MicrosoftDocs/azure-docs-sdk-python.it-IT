---
title: Librerie di Azure per Python
description: Panoramica delle librerie di gestione e di servizi di Azure per Python
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 68074d445a21a38fe6ffb6f5f7b7cbd8f24d87a3
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-libraries-for-python"></a><span data-ttu-id="cf0f7-104">Librerie di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="cf0f7-104">Azure libraries for Python</span></span>

<span data-ttu-id="cf0f7-105">Le librerie di Azure per Python consentono di usare i servizi di Azure e di gestire le risorse di Azure dal codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-105">The Azure libraries for Python let you use Azure services and manage Azure resources from your application code.</span></span> <span data-ttu-id="cf0f7-106">Le librerie sono disponibili in [PyPI](python-sdk-azure-install.md) per l'uso nei progetti Python.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-106">The libraries are available in [PyPI](python-sdk-azure-install.md) for use in your Python projects.</span></span>

## <a name="manage-azure-resources"></a><span data-ttu-id="cf0f7-107">Gestire le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="cf0f7-107">Manage Azure resources</span></span>

<span data-ttu-id="cf0f7-108">Creare e gestire le risorse di Azure dalle applicazioni Python usando le librerie di Azure per Python.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-108">Create and manage Azure resources from Python applications using the Azure libraries for Python.</span></span>

<span data-ttu-id="cf0f7-109">Ad esempio, per creare un'istanza di SQL Server, è possibile usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="cf0f7-109">For example, to create a SQL Server instance, you can use the following code:</span></span>

```python
sql_client = SqlManagementClient(
    credentials,
    subscription_id
)

server = sql_client.servers.create_or_update(
    'myresourcegroup',
    'myservername',
    {
        'location': 'eastus',
        'version': '12.0', # Required for create
        'administrator_login': 'mysecretname', # Required for create
        'administrator_login_password': 'HusH_Sec4et' # Required for create
    }
)
```

<span data-ttu-id="cf0f7-110">Vedere le [istruzioni di installazione](python-sdk-azure-install.md) per ottenere un elenco completo delle librerie e informazioni sull'importazione delle librerie nei progetti, quindi leggere l'[articolo introduttivo](python-sdk-azure-get-started.md) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-110">Review the [install instructions](python-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the the [get started article](python-sdk-azure-get-started.md) to set up your authentication and run sample code against your own Azure subscription.</span></span>

## <a name="connect-to-azure-services"></a><span data-ttu-id="cf0f7-111">Connettersi ai servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="cf0f7-111">Connect to Azure services</span></span>

<span data-ttu-id="cf0f7-112">Oltre a usare le librerie Python per creare e gestire risorse in Azure, è possibile usarle per connettersi a tali risorse e usarle nelle app.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-112">In addition to using Python libraries to create and manage resources within Azure, you can also use Python libraries to connect and use those resources in your apps.</span></span> <span data-ttu-id="cf0f7-113">È ad esempio possibile aggiornare una tabella di un database SQL o archiviare file in Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-113">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="cf0f7-114">Selezionare la libreria necessaria per un servizio specifico dall'elenco completo di librerie e passare al centro per sviluppatori Python per ottenere esercitazioni e codice di esempio utili per scoprire come usare le librerie nelle app.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-114">Select the library you need for a particular service from the complete list of libraries and visit the Python developer center for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="cf0f7-115">Per caricare ad esempio una semplice pagina HTML in un BLOB e ottenere l'URL:</span><span class="sxs-lookup"><span data-stu-id="cf0f7-115">For example, to upload a simple HTML page on a blob and get the Url:</span></span>

```python
storage_client = CloudStorageAccount(storage_account_name, storage_key)
blob_service = storage_client.create_block_blob_service()

blob_service.create_container(
    'mycontainername',
    public_access=PublicAccess.Blob
)

blob_service.create_blob_from_bytes(
    'mycontainername',
    'myblobname',
    b'<center><h1>Hello World!</h1></center>',
    content_settings=ContentSettings('text/html')
)

print(blob_service.make_blob_url('mycontainername', 'myblobname'))
```

## <a name="sample-code-and-reference"></a><span data-ttu-id="cf0f7-116">Codice di esempio e informazioni di riferimento</span><span class="sxs-lookup"><span data-stu-id="cf0f7-116">Sample code and reference</span></span>
<span data-ttu-id="cf0f7-117">Gli esempi seguenti sono relativi ad attività di automazione comuni con le librerie di gestione di Azure per Python e includono codice pronto per l'uso nelle app:</span><span class="sxs-lookup"><span data-stu-id="cf0f7-117">The following samples cover common automation tasks with the Azure management libraries for Python and have code ready to use in your own apps:</span></span>
- [<span data-ttu-id="cf0f7-118">Macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="cf0f7-118">Virtual Machines</span></span>](python-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="cf0f7-119">App Web</span><span class="sxs-lookup"><span data-stu-id="cf0f7-119">Web apps</span></span>](python-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="cf0f7-120">Database SQL</span><span class="sxs-lookup"><span data-stu-id="cf0f7-120">SQL Database</span></span>](python-sdk-azure-sql-database-samples.md)

<span data-ttu-id="cf0f7-121">Le [informazioni di riferimento](/python/api/overview/azure) sono disponibili per tutti i pacchetti nelle librerie di gestione e di servizi.</span><span class="sxs-lookup"><span data-stu-id="cf0f7-121">A [reference](/python/api/overview/azure) is available for all packages in both the service an management libraries.</span></span> <span data-ttu-id="cf0f7-122">Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](python-sdk-azure-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="cf0f7-122">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](python-sdk-azure-release-notes.md).</span></span> 

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="cf0f7-123">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="cf0f7-123">Get help and give feedback</span></span>

<span data-ttu-id="cf0f7-124">Pubblicare domande alla community su [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) e segnalare problemi relativi all'SDK nel [progetto di GitHub](https://github.com/Azure/azure-sdk-for-python).</span><span class="sxs-lookup"><span data-stu-id="cf0f7-124">Post questions to the community on [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) and open issues against the SDK on the [project GitHub](https://github.com/Azure/azure-sdk-for-python).</span></span>
