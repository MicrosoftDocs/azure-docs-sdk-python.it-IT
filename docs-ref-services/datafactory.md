---
title: Librerie di Azure Data Factory per Python
description: Informazioni di riferimento sulle librerie di Azure Data Factory per Python
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 05/10/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 05d93f7d1838c110c3ba77f7abd3967f7870774b
ms.sourcegitcommit: d65549030a0edb50d75482f79aac0962d1dacb57
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/05/2018
ms.locfileid: "34759048"
---
# <a name="azure-data-factory-libraries-for-python"></a><span data-ttu-id="b8977-103">Librerie di Azure Data Factory per Python</span><span class="sxs-lookup"><span data-stu-id="b8977-103">Azure Data Factory libraries for Python</span></span>

<span data-ttu-id="b8977-104">[Azure Data Factory](/azure/data-factory/) consente di comporre servizi di archiviazione, spostamento ed elaborazione di dati in pipeline di dati automatizzate.</span><span class="sxs-lookup"><span data-stu-id="b8977-104">Compose data storage, movement, and processing services into automated data pipelines with [Azure Data Factory](/azure/data-factory/)</span></span>

<span data-ttu-id="b8977-105">Vedere [altre informazioni](/azure/data-factory/introduction) su Data Factory e iniziare con la guida introduttiva [Creare una data factory e una pipeline con Python](/azure/data-factory/quickstart-create-data-factory-python).</span><span class="sxs-lookup"><span data-stu-id="b8977-105">[Learn more](/azure/data-factory/introduction) about Data Factory and get started with the [Create a data factory and pipeline using Python quickstart](/azure/data-factory/quickstart-create-data-factory-python).</span></span> 

## <a name="management-module"></a><span data-ttu-id="b8977-106">Modulo di gestione</span><span class="sxs-lookup"><span data-stu-id="b8977-106">Management module</span></span>

<span data-ttu-id="b8977-107">Creare e gestire istanze di Data Factory nella sottoscrizione con il modulo di gestione.</span><span class="sxs-lookup"><span data-stu-id="b8977-107">Create and manage Data Factory instances in your subscription with the management module.</span></span>

### <a name="installation"></a><span data-ttu-id="b8977-108">Installazione</span><span class="sxs-lookup"><span data-stu-id="b8977-108">Installation</span></span>

<span data-ttu-id="b8977-109">Installare il pacchetto usando [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b8977-109">Install the package with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```bash
pip install azure-mgmt-datafactory 
```

### <a name="example"></a><span data-ttu-id="b8977-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="b8977-110">Example</span></span> 

<span data-ttu-id="b8977-111">Creare una data factory nella sottoscrizione nell'area Stati Uniti orientali.</span><span class="sxs-lookup"><span data-stu-id="b8977-111">Create a Data Factory in your subscription on the East US region.</span></span>

```python
from azure.common.credentials import ServicePrincipalCredentials
from azure.mgmt.resource import ResourceManagementClient
from azure.mgmt.datafactory import DataFactoryManagementClient
from azure.mgmt.datafactory.models import *
import time

#Create a data factory
subscription_id = '<Specify your Azure Subscription ID>'
credentials = ServicePrincipalCredentials(client_id='<Active Directory application/client ID>', secret='<client secret>', tenant='<Active Directory tenant ID>')
adf_client = DataFactoryManagementClient(credentials, subscription_id)

rg_params = {'location':'eastus'}
df_params = {'location':'eastus'}  

df_resource = Factory(location='eastus')
df = adf_client.factories.create_or_update(rg_name, df_name, df_resource)
print_item(df)
while df.provisioning_state != 'Succeeded':
    df = adf_client.factories.get(rg_name, df_name)
    time.sleep(1)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="b8977-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="b8977-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datafactory/management)