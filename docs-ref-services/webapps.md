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
ms.openlocfilehash: 4870394c6ee39cde546d090fa1a0d136609851b3
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376696"
---
# <a name="azure-web-apps-libraries-for-python"></a><span data-ttu-id="5f474-103">Librerie delle app Web di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="5f474-103">Azure Web Apps libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="5f474-104">Panoramica</span><span class="sxs-lookup"><span data-stu-id="5f474-104">Overview</span></span>

<span data-ttu-id="5f474-105">Distribuire e ridimensionare siti Web, applicazioni Web, servizi e API REST con il [servizio app di Azure](/azure/app-service).</span><span class="sxs-lookup"><span data-stu-id="5f474-105">Deploy and scale websites, web applications, services, and REST APIs with [Azure App Service](/azure/app-service).</span></span>

<span data-ttu-id="5f474-106">Per iniziare a usare il servizio app di Azure, vedere [Creare un'app Web Python in Azure](/azure/app-service-web/app-service-web-get-started-python).</span><span class="sxs-lookup"><span data-stu-id="5f474-106">To get started with Azure App Service, see [Create a Python web app in Azure](/azure/app-service-web/app-service-web-get-started-python).</span></span>

## <a name="management-api"></a><span data-ttu-id="5f474-107">API di gestione</span><span class="sxs-lookup"><span data-stu-id="5f474-107">Management API</span></span>

<span data-ttu-id="5f474-108">Distribuire, gestire e ridimensionare gli elementi ospitati nel servizio app di Azure con l'API di gestione.</span><span class="sxs-lookup"><span data-stu-id="5f474-108">Deploy, manage, and scale elements hosted in the Azure App Service with the management API.</span></span>

<span data-ttu-id="5f474-109">Installare la libreria tramite pip.</span><span class="sxs-lookup"><span data-stu-id="5f474-109">Install the library via pip.</span></span>

```bash
pip install azure-mgmt-web
```

### <a name="example"></a><span data-ttu-id="5f474-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="5f474-110">Example</span></span>

<span data-ttu-id="5f474-111">Distribuire un'app Web da un repository di GitHub in un'app Web di Azure.</span><span class="sxs-lookup"><span data-stu-id="5f474-111">Deploy a webapp from a GitHub repository into Azure Web App.</span></span>

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
> [<span data-ttu-id="5f474-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="5f474-112">Explore the Management APIs</span></span>](/python/api/overview/azure/webapps/management)

## <a name="samples"></a><span data-ttu-id="5f474-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="5f474-113">Samples</span></span>

* <span data-ttu-id="5f474-114">[Manage Azure websites with python][1] (Gestire siti Web di Azure con Python)</span><span class="sxs-lookup"><span data-stu-id="5f474-114">[Manage Azure websites with python][1]</span></span>
* <span data-ttu-id="5f474-115">[Creare un flusso di lavoro di app per la logica][2]</span><span class="sxs-lookup"><span data-stu-id="5f474-115">[Create a Logic App workflow][2]</span></span>

<span data-ttu-id="5f474-116">Visualizzare l'[elenco completo](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app) di esempi di applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="5f474-116">View the [complete list](https://azure.microsoft.com/resources/samples/?platform=python&term=web-app) of web application samples.</span></span>

[1]: https://azure.microsoft.com/resources/samples/app-service-web-python-manage
[2]: ../docs-ref-conceptual/python-sdk-azure-samples-logic-app-workflow.md
