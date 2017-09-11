---
title: Librerie delle app Web di Azure per Python
description: 
keywords: Azure, Python, SDK, API, app Web, servizio app
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/12/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: appservice
ms.openlocfilehash: 05f6ad173f4ec051654b5eb2a986b2c2a93cc33a
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-web-apps-libraries-for-python"></a>Librerie delle app Web di Azure per Python

## <a name="overview"></a>Panoramica

Distribuire e ridimensionare siti Web, applicazioni Web, servizi e API REST con il [servizio app di Azure](/azure/app-service).

Per iniziare a usare il servizio app di Azure, vedere [Creare un'app Web Python in Azure](/azure/app-service-web/app-service-web-get-started-python).

## <a name="management-api"></a>API di gestione

Distribuire, gestire e ridimensionare gli elementi ospitati nel servizio app di Azure con l'API di gestione.

Installare la libreria tramite pip.

```bash
pip install azure-mgmt-web
```

### <a name="example"></a>Esempio

Distribuire un'app Web da un repository di GitHub in un'app Web di Azure.

```python
siteConfiguration = SiteConfig(
    python_version='3.4'
)

# create a web app
web_client.web_apps.create_or_update(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    Site(
        location='eastus',
        server_farm_id=SERVICE_PLAN_ID,
        site_config=siteConfiguration
    )
)

# continuous deployment with GitHub
source_control_async_operation = web_client.web_apps.create_or_update_source_control(
    RESOURCE_GROUP_NAME,
    WEB_APP_NAME,
    SiteSourceControl(
        location='GitHub',
        repo_url='https://github.com/lisawong19/python-docs-hello-world',
        branch='master'
    )
)
```
> [!div class="nextstepaction"]
> [Esplorare le API di gestione](/python/api/overview/azure/webapps/managementlibrary)

## <a name="samples"></a>Esempi 

* [Manage Azure websites with python][1] (Gestire siti Web di Azure con Python)
* [Creare un flusso di lavoro di app per la logica][2]
 
Visualizzare l'[elenco completo](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=web-app) di esempi di applicazioni Web.

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md