---
title: Librerie di Archiviazione di Azure per Python
description: 
keywords: Azure, Python, SDK, API, archiviazione
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: storage
ms.openlocfilehash: 64465964d32934a6a45dec44cb92a0a8a84b9c37
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="25037-103">Librerie di Archiviazione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="25037-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="25037-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="25037-104">Overview</span></span>
- <span data-ttu-id="25037-105">Leggere e scrivere oggetti e file dall'[archiviazione BLOB di Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span><span class="sxs-lookup"><span data-stu-id="25037-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="25037-106">Inviare e ricevere messaggi tra applicazioni connesse al cloud con l'[archiviazione code di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span><span class="sxs-lookup"><span data-stu-id="25037-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="25037-107">Leggere e scrivere dati strutturati di grandi dimensioni con l'[archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span><span class="sxs-lookup"><span data-stu-id="25037-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="25037-108">Condividere le risorse di archiviazione tra le app con l'[archiviazione file di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span><span class="sxs-lookup"><span data-stu-id="25037-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="25037-109">Creare, aggiornare e gestire gli account di archiviazione di Azure Storage, eseguire query e rigenerare chiavi di accesso dal codice Python con le librerie di gestione.</span><span class="sxs-lookup"><span data-stu-id="25037-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="25037-110">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="25037-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="25037-111">Client</span><span class="sxs-lookup"><span data-stu-id="25037-111">Client</span></span>

```bash
pip install azure-storage
```

### <a name="management"></a><span data-ttu-id="25037-112">gestione</span><span class="sxs-lookup"><span data-stu-id="25037-112">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="25037-113">Esempio</span><span class="sxs-lookup"><span data-stu-id="25037-113">Example</span></span>
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

## <a name="samples"></a><span data-ttu-id="25037-114">Esempi</span><span class="sxs-lookup"><span data-stu-id="25037-114">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="25037-115">Introduzione all'archiviazione BLOB di Azure con Python</span><span class="sxs-lookup"><span data-stu-id="25037-115">Get started with Azure Blob Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | <span data-ttu-id="25037-116">Creare, leggere, aggiornare, limitare l'accesso ed eliminare file e oggetti nell'Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="25037-116">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="25037-117">Introduzione all'archiviazione code di Azure in Python</span><span class="sxs-lookup"><span data-stu-id="25037-117">Get started with Azure Queue Storage in Python</span></span>](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | <span data-ttu-id="25037-118">Inserire, visualizzare l'anteprima, recuperare ed eliminare messaggi dalle code di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="25037-118">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="25037-119">Gestire gli account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="25037-119">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="25037-120">Creare, aggiornare ed eliminare gli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="25037-120">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="25037-121">Recuperare e rigenerare le chiavi di accesso degli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="25037-121">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="25037-122">Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app.</span><span class="sxs-lookup"><span data-stu-id="25037-122">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>