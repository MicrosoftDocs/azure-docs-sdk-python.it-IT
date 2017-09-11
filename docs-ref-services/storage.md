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
# <a name="azure-storage-libraries-for-python"></a>Librerie di Archiviazione di Azure per Python

## <a name="overview"></a>Panoramica
- Leggere e scrivere oggetti e file dall'[archiviazione BLOB di Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)
- Inviare e ricevere messaggi tra applicazioni connesse al cloud con l'[archiviazione code di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)
- Leggere e scrivere dati strutturati di grandi dimensioni con l'[archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) 
- Condividere le risorse di archiviazione tra le app con l'[archiviazione file di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)

Creare, aggiornare e gestire gli account di archiviazione di Azure Storage, eseguire query e rigenerare chiavi di accesso dal codice Python con le librerie di gestione.

## <a name="install-the-libraries"></a>Installare le librerie

### <a name="client"></a>Client

```bash
pip install azure-storage
```

### <a name="management"></a>gestione

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>Esempio
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

## <a name="samples"></a>Esempi

| | |
|--|--|
| [Introduzione all'archiviazione BLOB di Azure con Python](https://azure.microsoft.com/resources/samples/storage-blob-python-getting-started/) | Creare, leggere, aggiornare, limitare l'accesso ed eliminare file e oggetti nell'Archiviazione di Azure. |
| [Introduzione all'archiviazione code di Azure in Python](https://azure.microsoft.com/resources/samples/storage-queue-python-getting-started/) | Inserire, visualizzare l'anteprima, recuperare ed eliminare messaggi dalle code di Archiviazione di Azure. | 
| [Gestire gli account di archiviazione di Azure](https://azure.microsoft.com/resources/samples/storage-python-manage) | Creare, aggiornare ed eliminare gli account di archiviazione. Recuperare e rigenerare le chiavi di accesso degli account di archiviazione.

Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app.