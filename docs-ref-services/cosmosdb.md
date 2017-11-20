---
title: Librerie di Azure Cosmos DB per Python
description: Documentazione di riferimento per le librerie client Python per Cosmos DB
keywords: Azure, Python, SDK, API, SQL, database, PostGres,Cosmos DB, NoSQL
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 08/11/2017
ms.topic: article
ms.devlang: python
ms.service: cosmosdb
ms.openlocfilehash: 5382779d659f7c85e5ecbd304920e00b78a08a49
ms.sourcegitcommit: 3617d0db0111bbc00072ff8161de2d76606ce0ea
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/18/2017
---
# <a name="azure-cosmosdb-libraries-for-python"></a><span data-ttu-id="884b7-104">Librerie di Azure Cosmos DB per Python</span><span class="sxs-lookup"><span data-stu-id="884b7-104">Azure CosmosDB libraries for Python</span></span>

## <a name="overview"></a><span data-ttu-id="884b7-105">Panoramica</span><span class="sxs-lookup"><span data-stu-id="884b7-105">Overview</span></span>

<span data-ttu-id="884b7-106">Usare Cosmos DB nelle applicazioni Python per l'archiviazione e l'esecuzione di query nei documenti JSON in un archivio dati NoSQL.</span><span class="sxs-lookup"><span data-stu-id="884b7-106">Use CosmosDB in your Python applications to store and query JSON documents in a NoSQL data store.</span></span>

<span data-ttu-id="884b7-107">Altre informazioni su [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span><span class="sxs-lookup"><span data-stu-id="884b7-107">Learn more about [Azure CosmosDB](https://docs.microsoft.com/azure/cosmos-db/introduction).</span></span>

## <a name="client-library"></a><span data-ttu-id="884b7-108">Libreria client</span><span class="sxs-lookup"><span data-stu-id="884b7-108">Client library</span></span>
 ```bash
pip install pydocumentdb
 ```

## <a name="management-library"></a><span data-ttu-id="884b7-109">Libreria di gestione</span><span class="sxs-lookup"><span data-stu-id="884b7-109">Management library</span></span>
```bash
pip install azure-mgmt-cosmosdb
```

### <a name="example"></a><span data-ttu-id="884b7-110">Esempio</span><span class="sxs-lookup"><span data-stu-id="884b7-110">Example</span></span>

<span data-ttu-id="884b7-111">Trovare documenti corrispondenti in Cosmos DB usando un'interfaccia di query analoga a SQL:</span><span class="sxs-lookup"><span data-stu-id="884b7-111">Find matching documents in CosmosDB using a SQL-like query interface:</span></span>

```python
import pydocumentdb
import pydocumentdb.document_client as document_client

# Initialize the Python DocumentDB client
client = document_client.DocumentClient(config['ENDPOINT'], {'masterKey': config['MASTERKEY']})
# Create a database
db = client.CreateDatabase({ 'id': config['DOCUMENTDB_DATABASE'] })

# Create collection options
options = {
    'offerEnableRUPerMinuteThroughput': True,
    'offerVersion': "V2",
    'offerThroughput': 400
}

# Create a collection
collection = client.CreateCollection(db['_self'], { 'id': config['DOCUMENTDB_COLLECTION'] }, options)

# Create some documents
document1 = client.CreateDocument(collection['_self'],
    { 
        'id': 'server1',
        'Web Site': 0,
        'Cloud Service': 0,
        'Virtual Machine': 0,
        'name': 'some' 
    })

# Query them in SQL
query = { 'query': 'SELECT * FROM server s' }    

options = {} 
options['enableCrossPartitionQuery'] = True
options['maxItemCount'] = 2

result_iterable = client.QueryDocuments(collection['_self'], query, options)
results = list(result_iterable)

print(results)
```
> [!div class="nextstepaction"]
> [<span data-ttu-id="884b7-112">Esplorare le API di gestione</span><span class="sxs-lookup"><span data-stu-id="884b7-112">Explore the Management APIs</span></span>](/python/api/overview/azure/cosmosdb/managementlibrary)

## <a name="samples"></a><span data-ttu-id="884b7-113">Esempi</span><span class="sxs-lookup"><span data-stu-id="884b7-113">Samples</span></span>

<span data-ttu-id="884b7-114">[Develop a Python app using Azure Cosmos DB's DocumentDB API](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/) (Sviluppare un'app Python usando l'API DocumentDB di Azure Cosmos DB)</span><span class="sxs-lookup"><span data-stu-id="884b7-114">[Develop a Python app using Azure Cosmos DB's DocumentDB API](https://azure.microsoft.com/resources/samples/azure-cosmos-db-documentdb-python-getting-started/)</span></span>


