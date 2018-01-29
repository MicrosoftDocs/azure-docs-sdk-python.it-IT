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
ms.openlocfilehash: e00e821ff3e806a994fa8d96aae50c35eeeb8392
ms.sourcegitcommit: 5ab15a7214082d16f339a13e4ae7735b3a57a9aa
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 01/19/2018
---
# <a name="azure-storage-libraries-for-python"></a><span data-ttu-id="36f6a-103">Librerie di Archiviazione di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="36f6a-103">Azure Storage libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="36f6a-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="36f6a-104">Overview</span></span>
- <span data-ttu-id="36f6a-105">Leggere e scrivere oggetti e file dall'[archivio BLOB di Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span><span class="sxs-lookup"><span data-stu-id="36f6a-105">Read and write objects and files from [Azure Blob storage](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)</span></span>
- <span data-ttu-id="36f6a-106">Inviare e ricevere messaggi tra applicazioni connesse al cloud con l'[archiviazione code di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span><span class="sxs-lookup"><span data-stu-id="36f6a-106">Send and receive messages between cloud-connected applications with [Azure Queue storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)</span></span>
- <span data-ttu-id="36f6a-107">Leggere e scrivere dati strutturati di grandi dimensioni con l'[archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span><span class="sxs-lookup"><span data-stu-id="36f6a-107">Read and write large structured data with [Azure Table storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage)</span></span> 
- <span data-ttu-id="36f6a-108">Condividere le risorse di archiviazione tra le app con l'[archiviazione file di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span><span class="sxs-lookup"><span data-stu-id="36f6a-108">Share storage between apps with [Azure File storage](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)</span></span>

<span data-ttu-id="36f6a-109">Creare, aggiornare e gestire gli account di archiviazione di Azure Storage, eseguire query e rigenerare chiavi di accesso dal codice Python con le librerie di gestione.</span><span class="sxs-lookup"><span data-stu-id="36f6a-109">Create, update, and manage Azure Storage accounts and query and regenerate access keys from your Python code with the management libraries.</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="36f6a-110">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="36f6a-110">Install the libraries</span></span>

### <a name="client"></a><span data-ttu-id="36f6a-111">Client</span><span class="sxs-lookup"><span data-stu-id="36f6a-111">Client</span></span>

<span data-ttu-id="36f6a-112">Le librerie client di Archiviazione di Azure sono costituite da 4 pacchetti: BLOB, File, Coda e Tabella.</span><span class="sxs-lookup"><span data-stu-id="36f6a-112">Azure Storage Client Libraries consist of 4 packages: Blob, File, Queue and Table.</span></span> <span data-ttu-id="36f6a-113">Per installare il pacchetto BLOB eseguire:</span><span class="sxs-lookup"><span data-stu-id="36f6a-113">To install the blob package, run:</span></span>

```bash
pip install azure-storage-blob
```

### <a name="management"></a><span data-ttu-id="36f6a-114">Gestione</span><span class="sxs-lookup"><span data-stu-id="36f6a-114">Management</span></span>

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a><span data-ttu-id="36f6a-115">Esempio</span><span class="sxs-lookup"><span data-stu-id="36f6a-115">Example</span></span>
```python
blob_service = BlockBlobService(account_name, account_key)

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

## <a name="samples"></a><span data-ttu-id="36f6a-116">Esempi</span><span class="sxs-lookup"><span data-stu-id="36f6a-116">Samples</span></span>

| | |
|--|--|
| [<span data-ttu-id="36f6a-117">Introduzione all'archiviazione BLOB di Azure con Python</span><span class="sxs-lookup"><span data-stu-id="36f6a-117">Get started with Azure Blob Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | <span data-ttu-id="36f6a-118">Creare, leggere, aggiornare, limitare l'accesso ed eliminare file e oggetti nell'Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="36f6a-118">Create, read, update, restrict access, and delete files and objects in Azure Storage.</span></span> |
| [<span data-ttu-id="36f6a-119">Introduzione all'archiviazione code di Azure in Python</span><span class="sxs-lookup"><span data-stu-id="36f6a-119">Get started with Azure Queue Storage in Python</span></span>](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | <span data-ttu-id="36f6a-120">Inserire, visualizzare l'anteprima, recuperare ed eliminare messaggi dalle code di Archiviazione di Azure.</span><span class="sxs-lookup"><span data-stu-id="36f6a-120">Insert, peek, retrieve and delete messages from Azure Storage queues.</span></span> | 
| [<span data-ttu-id="36f6a-121">Gestire gli account di archiviazione di Azure</span><span class="sxs-lookup"><span data-stu-id="36f6a-121">Manage Azure Storage accounts</span></span>](https://azure.microsoft.com/resources/samples/storage-python-manage) | <span data-ttu-id="36f6a-122">Creare, aggiornare ed eliminare gli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="36f6a-122">Create, update , and delete storage accounts.</span></span> <span data-ttu-id="36f6a-123">Recuperare e rigenerare le chiavi di accesso degli account di archiviazione.</span><span class="sxs-lookup"><span data-stu-id="36f6a-123">Retrieve and regenerate storage account access keys.</span></span>

<span data-ttu-id="36f6a-124">Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app.</span><span class="sxs-lookup"><span data-stu-id="36f6a-124">Explore more [sample Python code](https://azure.microsoft.com/resources/samples/?platform=python) you can use in your apps.</span></span>
