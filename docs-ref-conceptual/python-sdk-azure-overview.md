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
# <a name="azure-libraries-for-python"></a>Librerie di Azure per Python

Le librerie di Azure per Python consentono di usare i servizi di Azure e di gestire le risorse di Azure dal codice dell'applicazione. Le librerie sono disponibili in [PyPI](python-sdk-azure-install.md) per l'uso nei progetti Python.

## <a name="manage-azure-resources"></a>Gestire le risorse di Azure

Creare e gestire le risorse di Azure dalle applicazioni Python usando le librerie di Azure per Python.

Ad esempio, per creare un'istanza di SQL Server, è possibile usare il codice seguente:

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

Vedere le [istruzioni di installazione](python-sdk-azure-install.md) per ottenere un elenco completo delle librerie e informazioni sull'importazione delle librerie nei progetti, quindi leggere l'[articolo introduttivo](python-sdk-azure-get-started.md) per configurare l'autenticazione ed eseguire il codice di esempio nella propria sottoscrizione di Azure.

## <a name="connect-to-azure-services"></a>Connettersi ai servizi di Azure

Oltre a usare le librerie Python per creare e gestire risorse in Azure, è possibile usarle per connettersi a tali risorse e usarle nelle app. È ad esempio possibile aggiornare una tabella di un database SQL o archiviare file in Archiviazione di Azure. Selezionare la libreria necessaria per un servizio specifico dall'elenco completo di librerie e passare al centro per sviluppatori Python per ottenere esercitazioni e codice di esempio utili per scoprire come usare le librerie nelle app.

Per caricare ad esempio una semplice pagina HTML in un BLOB e ottenere l'URL:

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

## <a name="sample-code-and-reference"></a>Codice di esempio e informazioni di riferimento
Gli esempi seguenti sono relativi ad attività di automazione comuni con le librerie di gestione di Azure per Python e includono codice pronto per l'uso nelle app:
- [Macchine virtuali](python-sdk-azure-virtual-machine-samples.md)
- [App Web](python-sdk-azure-web-apps-samples.md)
- [Database SQL](python-sdk-azure-sql-database-samples.md)

Le [informazioni di riferimento](/python/api/overview/azure) sono disponibili per tutti i pacchetti nelle librerie di gestione e di servizi. Le nuove funzionalità, le modifiche di rilievo e le istruzioni per la migrazione dalle versioni precedenti sono disponibili nelle [note sulla versione](python-sdk-azure-release-notes.md). 

## <a name="get-help-and-give-feedback"></a>Ottenere supporto e inviare commenti

Pubblicare domande alla community su [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-sdk-python) e segnalare problemi relativi all'SDK nel [progetto di GitHub](https://github.com/Azure/azure-sdk-for-python).
