---
title: Configurazione delle operazioni di Azure SDK per Python
description: Codice C generato da Azure SDK per Python
keywords: Azure, Python, SDK, API, eccezioni
author: rloutlaw
ms.author: routlaw
manager: douge
ms.date: 03/07/2018
ms.topic: article
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: adeb6aa8a2c363c3119e97503df9536fb0633b4c
ms.sourcegitcommit: 434186988284e0a8268a9de11645912a81226d6b
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/29/2019
ms.locfileid: "66376868"
---
# <a name="operation-config"></a><span data-ttu-id="759cc-104">Configurazione delle operazioni</span><span class="sxs-lookup"><span data-stu-id="759cc-104">Operation config</span></span> 

<span data-ttu-id="759cc-105">I metodi per le operazioni includono parametri aggiuntivi che possono essere forniti in `kwargs`.</span><span class="sxs-lookup"><span data-stu-id="759cc-105">Methods on operations have extra parameters which can be provided in the `kwargs`.</span></span> <span data-ttu-id="759cc-106">Questo approccio è definito operation_config.</span><span class="sxs-lookup"><span data-stu-id="759cc-106">This is called operation_config.</span></span>

<span data-ttu-id="759cc-107">Le opzioni per la configurazione delle operazioni sono le seguenti:</span><span class="sxs-lookup"><span data-stu-id="759cc-107">The options for operation configuration are:</span></span>

|<span data-ttu-id="759cc-108">Nome parametro</span><span class="sxs-lookup"><span data-stu-id="759cc-108">Parameter name</span></span>|<span data-ttu-id="759cc-109">Type</span><span class="sxs-lookup"><span data-stu-id="759cc-109">Type</span></span>|<span data-ttu-id="759cc-110">Ruolo</span><span class="sxs-lookup"><span data-stu-id="759cc-110">Role</span></span>|
|----------------------|------|---------------|
| <span data-ttu-id="759cc-111">verify</span><span class="sxs-lookup"><span data-stu-id="759cc-111">verify</span></span> |`bool`|<span data-ttu-id="759cc-112">Indica se verificare il certificato SSL.</span><span class="sxs-lookup"><span data-stu-id="759cc-112">Whether to verify the SSL certificate.</span></span> <span data-ttu-id="759cc-113">Il valore predefinito è true.</span><span class="sxs-lookup"><span data-stu-id="759cc-113">Default is True.</span></span>|
|  <span data-ttu-id="759cc-114">cert</span><span class="sxs-lookup"><span data-stu-id="759cc-114">cert</span></span> |`str`| <span data-ttu-id="759cc-115">Percorso per il certificato locale per la verifica lato client.</span><span class="sxs-lookup"><span data-stu-id="759cc-115">Path to local certificate for client side verification.</span></span>|
|  <span data-ttu-id="759cc-116">timeout</span><span class="sxs-lookup"><span data-stu-id="759cc-116">timeout</span></span> |`int`| <span data-ttu-id="759cc-117">Timeout in secondi per stabilire una connessione al server.</span><span class="sxs-lookup"><span data-stu-id="759cc-117">Timeout for establishing a server connection in seconds.</span></span>|
|  <span data-ttu-id="759cc-118">allow_redirects</span><span class="sxs-lookup"><span data-stu-id="759cc-118">allow_redirects</span></span> |`bool` | <span data-ttu-id="759cc-119">Indica se consentire i reindirizzamenti.</span><span class="sxs-lookup"><span data-stu-id="759cc-119">Whether to allow redirects.</span></span>|
|  <span data-ttu-id="759cc-120">max_redirects</span><span class="sxs-lookup"><span data-stu-id="759cc-120">max_redirects</span></span>  |`int`| <span data-ttu-id="759cc-121">Numero massimo di reindirizzamenti consentiti.</span><span class="sxs-lookup"><span data-stu-id="759cc-121">Maimum number of allowed redirects.</span></span>|
|  <span data-ttu-id="759cc-122">proxies</span><span class="sxs-lookup"><span data-stu-id="759cc-122">proxies</span></span>  |`dict` |<span data-ttu-id="759cc-123">Impostazioni del server proxy.</span><span class="sxs-lookup"><span data-stu-id="759cc-123">Proxy server settings.</span></span>|
|  <span data-ttu-id="759cc-124">use_env_proxies</span><span class="sxs-lookup"><span data-stu-id="759cc-124">use_env_proxies</span></span> |`bool` |<span data-ttu-id="759cc-125">Indica se leggere le impostazioni del proxy dalle variabili di ambiente locali.</span><span class="sxs-lookup"><span data-stu-id="759cc-125">Whether to read proxy settings from local environment variables.</span></span>|
|  <span data-ttu-id="759cc-126">retries</span><span class="sxs-lookup"><span data-stu-id="759cc-126">retries</span></span>  |`int` | <span data-ttu-id="759cc-127">Numero totale di tentativi.</span><span class="sxs-lookup"><span data-stu-id="759cc-127">Total number of retry attempts.</span></span>|
|  <span data-ttu-id="759cc-128">enable_http_logger</span><span class="sxs-lookup"><span data-stu-id="759cc-128">enable_http_logger</span></span> | `bool`| <span data-ttu-id="759cc-129">Abilita i log di HTTP in modalità di debug (False per impostazione predefinita).</span><span class="sxs-lookup"><span data-stu-id="759cc-129">Enable logs of HTTP in debug mode (False by default).</span></span>|
