---
title: Librerie delle app Web di Azure per Python
description: ''
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
ms.openlocfilehash: 8e8dd78cbc2d5887308361a47a9571ce242aee6e
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
ms.locfileid: "29479224"
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
> [Esplorare le API di gestione](/python/api/overview/azure/webapps/management)

## <a name="samples"></a>Esempi 

* [Manage Azure websites with python][1] (Gestire siti Web di Azure con Python)
* [Creare un flusso di lavoro di app per la logica][2]
 
Visualizzare l'[elenco completo](https://azure.microsoft.com/en-us/resources/samples/?platform=python&term=web-app) di esempi di applicazioni Web.

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md