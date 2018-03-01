---
title: Librerie di Azure Data Lake Analytics per Python
description: Informazioni di riferimento sulle librerie di Azure Data Lake Analytics per Python
keywords: Azure, Python, SDK, API, Data Lake Analytics
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/04/2017
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: d779aca1f3a9e14f275385f93054a8e2f9c0c689
ms.sourcegitcommit: 41e90fe75de03d397079a276cdb388305290e27e
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/23/2018
---
# <a name="azure-data-lake-analytics-libraries-for-python"></a><span data-ttu-id="cac9b-104">Librerie di Azure Data Lake Analytics per Python</span><span class="sxs-lookup"><span data-stu-id="cac9b-104">Azure Data Lake Analytics libraries for python</span></span>

## <a name="overview"></a><span data-ttu-id="cac9b-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="cac9b-105">Overview</span></span>
<span data-ttu-id="cac9b-106">[Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview) consente di eseguire processi di analisi di Big Data scalabili fino a set di dati di dimensioni molto grandi.</span><span class="sxs-lookup"><span data-stu-id="cac9b-106">Run big data analysis jobs that scale to massive data sets with [Azure Data Lake Analytics](/azure/data-lake-analytics/data-lake-analytics-overview).</span></span>

## <a name="install-the-libraries"></a><span data-ttu-id="cac9b-107">Installare le librerie</span><span class="sxs-lookup"><span data-stu-id="cac9b-107">Install the libraries</span></span>

## <a name="management-api"></a><span data-ttu-id="cac9b-108">API di gestione</span><span class="sxs-lookup"><span data-stu-id="cac9b-108">Management API</span></span>
<span data-ttu-id="cac9b-109">Usare l'API di gestione per gestire account, processi, criteri e cataloghi di Data Lake Analytics.</span><span class="sxs-lookup"><span data-stu-id="cac9b-109">Use the management API to manage Data Lake Analytics accounts, jobs, policies, and catalogs.</span></span>

```bash
pip install azure-mgmt-datalake-analytics
```

### <a name="example"></a><span data-ttu-id="cac9b-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="cac9b-110">Example</span></span>
<span data-ttu-id="cac9b-111">Questo esempio illustra come creare un account Data Lake Analytics e come inviare un processo.</span><span class="sxs-lookup"><span data-stu-id="cac9b-111">This is an example of how to create a Data Lake Analytics account and submit a job.</span></span> 

```python
## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adls = '<Azure Data Lake Analytics Account Name>'

# Create the clients
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')

# Create resource group
armGroupResult = resourceClient.resource_groups.create_or_update(rg, ResourceGroup(location=location))

# Create a store account
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()

# Create an ADLA account
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()

# Submit a job
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

> [!div class="nextstepaction"]
> [<span data-ttu-id="cac9b-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="cac9b-112">Explore the Management APIs</span></span>](/python/api/overview/azure/datalakeanalytics/management)

## <a name="samples"></a><span data-ttu-id="cac9b-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="cac9b-113">Samples</span></span>
[<span data-ttu-id="cac9b-114">Gestire Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="cac9b-114">Manage Azure Data Lake Anyalytics</span></span>](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-manage-use-python-sdk)