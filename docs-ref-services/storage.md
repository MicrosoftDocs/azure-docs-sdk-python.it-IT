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
# <a name="azure-storage-libraries-for-python"></a>Librerie di Archiviazione di Azure per Python

## <a name="overview"></a>Panoramica
- Leggere e scrivere oggetti e file dall'[archivio BLOB di Azure](https://docs.microsoft.com/en-us/azure/storage/storage-python-how-to-use-blob-storage)
- Inviare e ricevere messaggi tra applicazioni connesse al cloud con l'[archiviazione code di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-queue-storage)
- Leggere e scrivere dati strutturati di grandi dimensioni con l'[archiviazione tabelle di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-table-storage) 
- Condividere le risorse di archiviazione tra le app con l'[archiviazione file di Azure](https://docs.microsoft.com/azure/storage/storage-python-how-to-use-file-storage)

Creare, aggiornare e gestire gli account di archiviazione di Azure Storage, eseguire query e rigenerare chiavi di accesso dal codice Python con le librerie di gestione.

## <a name="install-the-libraries"></a>Installare le librerie

### <a name="client"></a>Client

Le librerie client di Archiviazione di Azure sono costituite da 4 pacchetti: BLOB, File, Coda e Tabella. Per installare il pacchetto BLOB eseguire:

```bash
pip install azure-storage-blob
```

### <a name="management"></a>Gestione

```bash
pip install azure-mgmt-storage
```

## <a name="example"></a>Esempio
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

## <a name="samples"></a>Esempi

| | |
|--|--|
| [Introduzione all'archiviazione BLOB di Azure con Python](https://docs.microsoft.com/en-us/azure/storage/blobs/storage-python-how-to-use-blob-storage) | Creare, leggere, aggiornare, limitare l'accesso ed eliminare file e oggetti nell'Archiviazione di Azure. |
| [Introduzione all'archiviazione code di Azure in Python](https://docs.microsoft.com/en-us/azure/storage/queues/storage-python-how-to-use-queue-storage) | Inserire, visualizzare l'anteprima, recuperare ed eliminare messaggi dalle code di Archiviazione di Azure. | 
| [Gestire gli account di archiviazione di Azure](https://azure.microsoft.com/resources/samples/storage-python-manage) | Creare, aggiornare ed eliminare gli account di archiviazione. Recuperare e rigenerare le chiavi di accesso degli account di archiviazione.

Esplorare altro [codice Python di esempio](https://azure.microsoft.com/resources/samples/?platform=python) da usare nelle app.
