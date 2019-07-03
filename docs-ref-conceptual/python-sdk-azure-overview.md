---
title: Librerie di Azure per Python
description: Panoramica delle librerie di gestione e di servizi di Azure per Python
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: routlaw
manager: douge
ms.date: 06/01/2017
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: ''
ms.openlocfilehash: bb17295de2f0272f0525fda5edab87e840764478
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534463"
---
# <a name="azure-libraries-for-python"></a><span data-ttu-id="1f035-104">Librerie di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="1f035-104">Azure libraries for Python</span></span>

<span data-ttu-id="1f035-105">Le librerie di Azure per Python consentono di usare i servizi di Azure e di gestire le risorse di Azure dal codice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="1f035-105">The Azure libraries for Python let you use Azure services and manage Azure resources from your application code.</span></span> 

## <a name="manage-azure-resources"></a><span data-ttu-id="1f035-106">Gestire le risorse di Azure</span><span class="sxs-lookup"><span data-stu-id="1f035-106">Manage Azure resources</span></span>

<span data-ttu-id="1f035-107">Creare e gestire le risorse di Azure dalle applicazioni Python usando le librerie di Azure per Python.</span><span class="sxs-lookup"><span data-stu-id="1f035-107">Create and manage Azure resources from Python applications using the Azure libraries for Python.</span></span>

<span data-ttu-id="1f035-108">Ad esempio, per creare un'istanza di SQL Server, è possibile usare il codice seguente:</span><span class="sxs-lookup"><span data-stu-id="1f035-108">For example, to create a SQL Server instance, you can use the following code:</span></span>

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

<span data-ttu-id="1f035-109">Vedere le [istruzioni di installazione](python-sdk-azure-install.md) per ottenere un elenco completo delle librerie e informazioni sull'importazione delle librerie nei progetti, quindi leggere l'[articolo introduttivo](python-sdk-azure-get-started.yml) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f035-109">Review the [install instructions](python-sdk-azure-install.md) for a full list of the libraries and how to import them into your projects and then read the [get started article](python-sdk-azure-get-started.yml) to set up your authentication and run sample code against your own Azure subscription.</span></span>

## <a name="connect-to-azure-services"></a><span data-ttu-id="1f035-110">Connettersi ai servizi di Azure</span><span class="sxs-lookup"><span data-stu-id="1f035-110">Connect to Azure services</span></span>

<span data-ttu-id="1f035-111">Oltre a usare le librerie Python per creare e gestire risorse in Azure, è possibile usarle per connettersi a tali risorse e usarle nelle app.</span><span class="sxs-lookup"><span data-stu-id="1f035-111">In addition to using Python libraries to create and manage resources within Azure, you can also use Python libraries to connect and use those resources in your apps.</span></span> <span data-ttu-id="1f035-112">È ad esempio possibile aggiornare una tabella di un database SQL o archiviare file in Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="1f035-112">For example, you might update a table SQL Database or store files in Azure Storage.</span></span> <span data-ttu-id="1f035-113">Selezionare la libreria necessaria per un servizio specifico dall'elenco completo di librerie e passare al centro per sviluppatori Python per ottenere esercitazioni e codice di esempio utili per scoprire come usare le librerie nelle app.</span><span class="sxs-lookup"><span data-stu-id="1f035-113">Select the library you need for a particular service from the complete list of libraries and visit the Python developer center for tutorials and sample code for help using them in your apps.</span></span>

<span data-ttu-id="1f035-114">Per caricare ad esempio una semplice pagina HTML in un BLOB e ottenere l'URL:</span><span class="sxs-lookup"><span data-stu-id="1f035-114">For example, to upload a simple HTML page on a blob and get the Url:</span></span>

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

## <a name="sample-code-and-reference"></a><span data-ttu-id="1f035-115">Codice di esempio e informazioni di riferimento</span><span class="sxs-lookup"><span data-stu-id="1f035-115">Sample code and reference</span></span>
<span data-ttu-id="1f035-116">Gli esempi seguenti sono relativi ad attività di automazione comuni con le librerie di gestione di Azure per Python e includono codice pronto per l'uso nelle app:</span><span class="sxs-lookup"><span data-stu-id="1f035-116">The following samples cover common automation tasks with the Azure management libraries for Python and have code ready to use in your own apps:</span></span>
- [<span data-ttu-id="1f035-117">Macchine virtuali</span><span class="sxs-lookup"><span data-stu-id="1f035-117">Virtual Machines</span></span>](python-sdk-azure-virtual-machine-samples.md)
- [<span data-ttu-id="1f035-118">App Web</span><span class="sxs-lookup"><span data-stu-id="1f035-118">Web apps</span></span>](python-sdk-azure-web-apps-samples.md)
- [<span data-ttu-id="1f035-119">Database SQL</span><span class="sxs-lookup"><span data-stu-id="1f035-119">SQL Database</span></span>](python-sdk-azure-sql-database-samples.md)

<span data-ttu-id="1f035-120">Le [informazioni di riferimento](/python/api/overview/azure) sono disponibili per tutti i pacchetti nelle librerie di gestione e di servizi.</span><span class="sxs-lookup"><span data-stu-id="1f035-120">A [reference](/python/api/overview/azure) is available for all packages in both the service an management libraries.</span></span> <span data-ttu-id="1f035-121">Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](python-sdk-azure-release-notes.md).</span><span class="sxs-lookup"><span data-stu-id="1f035-121">New features, breaking changes, and migration instructions from previous versions are available in the [release notes](python-sdk-azure-release-notes.md).</span></span> 

## <a name="get-help-and-give-feedback"></a><span data-ttu-id="1f035-122">Ottenere supporto e inviare commenti</span><span class="sxs-lookup"><span data-stu-id="1f035-122">Get help and give feedback</span></span>

<span data-ttu-id="1f035-123">Pubblicare domande alla community su [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) e segnalare problemi relativi all'SDK nel [progetto di GitHub](https://github.com/Azure/azure-sdk-for-python).</span><span class="sxs-lookup"><span data-stu-id="1f035-123">Post questions to the community on [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sdk-python) and open issues against the SDK on the [project GitHub](https://github.com/Azure/azure-sdk-for-python).</span></span>
