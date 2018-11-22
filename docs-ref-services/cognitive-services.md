---
title: Moduli di Servizi cognitivi di Azure per Python
description: Informazioni di riferimento sui moduli di Servizi cognitivi di Azure per Python
keywords: Azure, Python, SDK, API, Servizi cognitivi
author: rloutlaw
ms.author: routlaw
manager: angerobe
ms.date: 04/04/2018
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 736b2dd747842caa50418afc8219dafae655db39
ms.sourcegitcommit: f439ba940d5940359c982015db7ccfb82f9dffd9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277458"
---
# <a name="azure-cognitive-services-modules-for-python"></a><span data-ttu-id="b1019-104">Moduli di Servizi cognitivi di Azure per Python</span><span class="sxs-lookup"><span data-stu-id="b1019-104">Azure Cognitive Services modules for Python</span></span>

<span data-ttu-id="b1019-105">Aggiungere il riconoscimento di immagini e volti, l'analisi della lingua e funzionalità di ricerca ad app, siti Web e strumenti Python usando i moduli di Servizi cognitivi di Azure per Python.</span><span class="sxs-lookup"><span data-stu-id="b1019-105">Add image and face recognition, language analysis, and search to your Python apps, websites, and tools using the Azure Cognitive Services modules for Python.</span></span>

## <a name="vision-modules"></a><span data-ttu-id="b1019-106">Moduli di Visione</span><span class="sxs-lookup"><span data-stu-id="b1019-106">Vision modules</span></span>

### <a name="computer-vision"></a><span data-ttu-id="b1019-107">Visione artificiale</span><span class="sxs-lookup"><span data-stu-id="b1019-107">Computer Vision</span></span> 

<span data-ttu-id="b1019-108">Restituisce informazioni sul contenuto visivo presente in un'immagine:</span><span class="sxs-lookup"><span data-stu-id="b1019-108">Returns information about visual content found in an image:</span></span>

- <span data-ttu-id="b1019-109">Usare l'aggiunta di tag, le descrizioni e i modelli specifici di un dominio per identificare i contenuti ed etichettarli in tutta sicurezza.</span><span class="sxs-lookup"><span data-stu-id="b1019-109">Use tagging, descriptions, and domain-specific models to identify content and label it with confidence.</span></span>
- <span data-ttu-id="b1019-110">Applicare le impostazioni relative ai contenuti per adulti per limitare automaticamente i contenuti di questo tipo.</span><span class="sxs-lookup"><span data-stu-id="b1019-110">Apply adult/racy settings to enable automated restriction of adult content.</span></span>
- <span data-ttu-id="b1019-111">Identificare i tipi di immagine e le combinazioni colori nelle immagini.</span><span class="sxs-lookup"><span data-stu-id="b1019-111">Identify image types and color schemes in pictures.</span></span>

<span data-ttu-id="b1019-112">[Provare Visione artificiale](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) gratuitamente nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-112">[Try Computer Vision](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) for free in your browser.</span></span>

<span data-ttu-id="b1019-113">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-113">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-computervision
```

<span data-ttu-id="b1019-114">Vedere [altre informazioni](/azure/cognitive-services/computer-vision/home) sull'API Visione artificiale e la [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python) (Guida introduttiva all'API Visione artificiale per Python).</span><span class="sxs-lookup"><span data-stu-id="b1019-114">[Learn more](/azure/cognitive-services/computer-vision/home) about the Computer Vision API and get started with the [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python).</span></span>

### <a name="content-moderator"></a><span data-ttu-id="b1019-115">Content Moderator</span><span class="sxs-lookup"><span data-stu-id="b1019-115">Content Moderator</span></span>

<span data-ttu-id="b1019-116">Moderazione di testo, video e immagini con supporto di computer, ottimizzata da strumenti di revisione umana.</span><span class="sxs-lookup"><span data-stu-id="b1019-116">Machine-assisted moderation of text, video and images, augmented with human review tools.</span></span>

<span data-ttu-id="b1019-117">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-117">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-computervision
```

<span data-ttu-id="b1019-118">[Altre informazioni](/azure/cognitive-services/content-moderator/overview) sul servizio Content Moderator.</span><span class="sxs-lookup"><span data-stu-id="b1019-118">[Learn more](/azure/cognitive-services/content-moderator/overview) about the Content Moderator service.</span></span>

### <a name="custom-vision-service"></a><span data-ttu-id="b1019-119">Servizio visione artificiale personalizzato</span><span class="sxs-lookup"><span data-stu-id="b1019-119">Custom Vision Service</span></span>

<span data-ttu-id="b1019-120">Caricare immagini per eseguire il training e personalizzare un modello di visione artificiale per il caso d'uso specifico.</span><span class="sxs-lookup"><span data-stu-id="b1019-120">Upload images to train and customize a computer vision model for your specific use case.</span></span> <span data-ttu-id="b1019-121">Dopo aver eseguito il training del modello, è possibile usare l'API per contrassegnare le immagini con il modello e valutare i risultati per migliorare il classificatore.</span><span class="sxs-lookup"><span data-stu-id="b1019-121">Once the model is trained, you can use the API to tag images using the model and evaluate the results to improve your classifier.</span></span>

<span data-ttu-id="b1019-122">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-122">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-vision-customvision
```

<span data-ttu-id="b1019-123">Vedere [altre informazioni](/azure/cognitive-services/Custom-Vision-Service/home) sul servizio Visione artificiale personalizzato e la [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial) (Esercitazione sul servizio Visione artificiale personalizzato per Python)</span><span class="sxs-lookup"><span data-stu-id="b1019-123">[Learn more](/azure/cognitive-services/Custom-Vision-Service/home) about the Custom Vision service and get started with the [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial)</span></span>

### <a name="face-api"></a><span data-ttu-id="b1019-124">API Viso</span><span class="sxs-lookup"><span data-stu-id="b1019-124">Face API</span></span>

<span data-ttu-id="b1019-125">Rilevare, identificare, analizzare, organizzare e contrassegnare con tag i visi nelle foto.</span><span class="sxs-lookup"><span data-stu-id="b1019-125">Detect, identify, analyze, organize, and tag faces in photos.</span></span> 

<span data-ttu-id="b1019-126">[Provare l'API Viso](https://azure.microsoft.com/en-us/services/cognitive-services/face/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-126">[Try the Face API](https://azure.microsoft.com/en-us/services/cognitive-services/face/) in your browser.</span></span>

<span data-ttu-id="b1019-127">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-127">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install cognitive-face
```

<span data-ttu-id="b1019-128">Vedere [altre informazioni](/azure/cognitive-services/face/overview) sull'API Viso e la [Guida introduttiva all'API Viso per Python](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).</span><span class="sxs-lookup"><span data-stu-id="b1019-128">[Learn more](/azure/cognitive-services/face/overview) about the Face API and get started with the [Face API Python quickstart](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).</span></span>

## <a name="search-modules"></a><span data-ttu-id="b1019-129">Moduli di ricerca</span><span class="sxs-lookup"><span data-stu-id="b1019-129">Search modules</span></span>

### <a name="web-search"></a><span data-ttu-id="b1019-130">Ricerca Web</span><span class="sxs-lookup"><span data-stu-id="b1019-130">Web search</span></span>

<span data-ttu-id="b1019-131">Recuperare documenti Web indicizzati dall'API Ricerca Web Bing e limitare i risultati filtrandoli in base a tipo, aggiornamento e altro.</span><span class="sxs-lookup"><span data-stu-id="b1019-131">Retrieve web documents indexed by the Bing Web Search API and narrow down the results by result type, freshness and more.</span></span> 

<span data-ttu-id="b1019-132">[Provare l'API Ricerca Web](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-132">[Try the Web Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) in your browser.</span></span>

<span data-ttu-id="b1019-133">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-133">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-websearch
```

<span data-ttu-id="b1019-134">Vedere [altre informazioni](/azure/cognitive-services/bing-web-search/overview) sull'API Ricerca Web Bing e la [Guida introduttiva all'API Ricerca Web per Python](/azure/cognitive-services/bing-web-search/quickstarts/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-134">[Learn more](/azure/cognitive-services/bing-web-search/overview) about the Bing Web Search API and get started with the [Web Search API Python quickstart](/azure/cognitive-services/bing-web-search/quickstarts/python).</span></span>

### <a name="image-search"></a><span data-ttu-id="b1019-135">Ricerca di immagini</span><span class="sxs-lookup"><span data-stu-id="b1019-135">Image search</span></span>

<span data-ttu-id="b1019-136">Cercare immagini e ottenere anteprime, URL di immagini completi, metadati delle immagini e molto altro.</span><span class="sxs-lookup"><span data-stu-id="b1019-136">Search for images and get thumbnails, full image URLs, image metadata and more in your results.</span></span>

<span data-ttu-id="b1019-137">[Provare l'API Ricerca immagini](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-137">[Try the Image Search API](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) in your browser.</span></span>

<span data-ttu-id="b1019-138">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-138">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-imagesearch
```

<span data-ttu-id="b1019-139">Vedere [altre informazioni](/azure/cognitive-services/bing-image-search/overview) sull'API Ricerca immagini Bing e la [Guida introduttiva all'API Ricerca immagini per Python](/azure/cognitive-services/bing-image-search/quickstarts/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-139">[Learn more](/azure/cognitive-services/bing-image-search/overview) about the Bing Image Search API and get started with the [Image Search API Python quickstart](/azure/cognitive-services/bing-image-search/quickstarts/python).</span></span>


### <a name="entity-search"></a><span data-ttu-id="b1019-140">Ricerca entità</span><span class="sxs-lookup"><span data-stu-id="b1019-140">Entity search</span></span>

<span data-ttu-id="b1019-141">Cercare l'entità più pertinente (luogo, persona o oggetto) per un determinato termine di ricerca o luogo.</span><span class="sxs-lookup"><span data-stu-id="b1019-141">Search for the most relevant entity (place, person, or thing) for a given search term or location.</span></span>

<span data-ttu-id="b1019-142">[Provare l'API Ricerca entità](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-142">[Try the Entity Search API](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) in your browser.</span></span>

<span data-ttu-id="b1019-143">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-143">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-entitysearch
```

<span data-ttu-id="b1019-144">Vedere [altre informazioni](/azure/cognitive-services/bing-entities-search/search-the-web) sull'API Ricerca entità Bing e la [Guida introduttiva all'API Ricerca entità per Python](/azure/cognitive-services/bing-entities-search/quickstarts/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-144">[Learn more](/azure/cognitive-services/bing-entities-search/search-the-web) about the Bing Entity Search API and get started with the [Entity Search API Python quickstart](/azure/cognitive-services/bing-entities-search/quickstarts/python).</span></span>

### <a name="custom-search"></a><span data-ttu-id="b1019-145">Ricerca personalizzata</span><span class="sxs-lookup"><span data-stu-id="b1019-145">Custom search</span></span>

<span data-ttu-id="b1019-146">Compilare una ricerca Web personalizzata in funzione del dominio di ricerca specifico.</span><span class="sxs-lookup"><span data-stu-id="b1019-146">Build and a custom web search that meets your specific search domain.</span></span>

<span data-ttu-id="b1019-147">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-147">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-customsearch
```

<span data-ttu-id="b1019-148">Vedere [altre informazioni](/azure/cognitive-services/bing-custom-search/) sul servizio Ricerca personalizzata Bing e la [Guida introduttiva all'API Ricerca personalizzata per Python](/azure/cognitive-services/bing-custom-search/call-endpoint-python) per iniziare a eseguire query di ricerca personalizzata da Python.</span><span class="sxs-lookup"><span data-stu-id="b1019-148">[Learn more](/azure/cognitive-services/bing-custom-search/) about the Bing Custom Search service and get started with querying your custom search from Python with the [Custom Search API Python quickstart](/azure/cognitive-services/bing-custom-search/call-endpoint-python).</span></span>

### <a name="video-search"></a><span data-ttu-id="b1019-149">Ricerca video</span><span class="sxs-lookup"><span data-stu-id="b1019-149">Video search</span></span>

<span data-ttu-id="b1019-150">Trovare video nel Web e ottenere risultati con metadati relativi ad autore, codifica, durata e conteggio delle visualizzazioni.</span><span class="sxs-lookup"><span data-stu-id="b1019-150">Find videos across the web and get results with creator, encoding, length, and view count metadata.</span></span>

<span data-ttu-id="b1019-151">[Provare l'API Ricerca video](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-151">[Try the Video Search API](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) in your browser.</span></span>

<span data-ttu-id="b1019-152">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-152">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-videosearch
```

<span data-ttu-id="b1019-153">Vedere [altre informazioni](/azure/cognitive-services/bing-video-search/search-the-web) sull'API Ricerca video Bing e la [Guida introduttiva all'API Ricerca video per Python](/azure/cognitive-services/bing-video-search/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-153">[Learn more](/azure/cognitive-services/bing-video-search/search-the-web) about the Bing Video Search service and get started with the [Video Search API Python quickstart](/azure/cognitive-services/bing-video-search/python).</span></span>


### <a name="news-search"></a><span data-ttu-id="b1019-154">Ricerca notizie</span><span class="sxs-lookup"><span data-stu-id="b1019-154">News search</span></span>

<span data-ttu-id="b1019-155">Cercare articoli di notizie sul Web e usare i metadati relativi ad articoli, notizie correlate, immagini e informazioni del provider.</span><span class="sxs-lookup"><span data-stu-id="b1019-155">Search the web for news articles and work with article, related news, images, and provider info metadata.</span></span>

<span data-ttu-id="b1019-156">[Provare l'API Ricerca notizie](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-156">[Try the News Search API](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) in your browser.</span></span>

<span data-ttu-id="b1019-157">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-157">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-search-newssearch
```

<span data-ttu-id="b1019-158">Vedere [altre informazioni](/azure/cognitive-services/bing-news-search/search-the-web) sull'API Ricerca notizie Bing e la [Guida introduttiva all'API Ricerca notizie per Python](//azure/cognitive-services/bing-news-search/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-158">[Learn more](/azure/cognitive-services/bing-news-search/search-the-web) about the Bing News Search service and get started with the [News Search API Python quickstart](//azure/cognitive-services/bing-news-search/python).</span></span>


## <a name="language-modules"></a><span data-ttu-id="b1019-159">Moduli per la lingua</span><span class="sxs-lookup"><span data-stu-id="b1019-159">Language modules</span></span>

### <a name="text-analytics"></a><span data-ttu-id="b1019-160">Text Analytics</span><span class="sxs-lookup"><span data-stu-id="b1019-160">Text Analytics</span></span> 

<span data-ttu-id="b1019-161">L'API Analisi del testo è un servizio basato su cloud che fornisce l'elaborazione in linguaggio naturale su testo non elaborato.</span><span class="sxs-lookup"><span data-stu-id="b1019-161">The Text Analytics API is a cloud-based service that provides  natural language processing over raw text.</span></span> <span data-ttu-id="b1019-162">L'API include tre funzioni principali:</span><span class="sxs-lookup"><span data-stu-id="b1019-162">The API includes three main functions:</span></span>

- <span data-ttu-id="b1019-163">Analisi del sentiment</span><span class="sxs-lookup"><span data-stu-id="b1019-163">Sentiment analysis</span></span>
- <span data-ttu-id="b1019-164">Estrazione di frasi chiave</span><span class="sxs-lookup"><span data-stu-id="b1019-164">Key phrase extraction</span></span>
- <span data-ttu-id="b1019-165">Rilevamento della lingua</span><span class="sxs-lookup"><span data-stu-id="b1019-165">Language detection</span></span>

<span data-ttu-id="b1019-166">[Provare l'API Analisi del testo](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-166">[Try the Text Analytics API](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) in your browser.</span></span>

<span data-ttu-id="b1019-167">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-167">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-language-textanalytics
```

<span data-ttu-id="b1019-168">Vedere [altre informazioni](/azure/cognitive-services/text-analytics/overview) sull'API Analisi del testo e la [Guida introduttiva all'API Analisi del testo per Python](/azure/cognitive-services/text-analytics/quickstarts/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-168">[Learn more](/azure/cognitive-services/text-analytics/overview) about the Text Analytics API and get started with the [Text Analytics API Python quickstart](/azure/cognitive-services/text-analytics/quickstarts/python).</span></span>


### <a name="spell-check"></a><span data-ttu-id="b1019-169">Controllo ortografico</span><span class="sxs-lookup"><span data-stu-id="b1019-169">Spell Check</span></span>

<span data-ttu-id="b1019-170">Eseguire il controllo grammaticale e ortografico contestuale con l'API Controllo ortografico Bing.</span><span class="sxs-lookup"><span data-stu-id="b1019-170">Perform contextual grammar and spell checking with the Bing Spell Check API.</span></span>

<span data-ttu-id="b1019-171">[Provare l'API Controllo ortografico](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) nel browser.</span><span class="sxs-lookup"><span data-stu-id="b1019-171">[Try the Spell Check API](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) in your browser.</span></span>

<span data-ttu-id="b1019-172">Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):</span><span class="sxs-lookup"><span data-stu-id="b1019-172">Get the Python module with [pip](https://pip.pypa.io/en/stable/quickstart/):</span></span>

```
pip install azure-cognitiveservices-language-spellcheck
```

<span data-ttu-id="b1019-173">Vedere [altre informazioni](/azure/cognitive-services/bing-spell-check/proof-text) sull'API Controllo ortografico e la [Guida introduttiva all'API Controllo ortografico per Python](/azure/cognitive-services/bing-spell-check/quickstarts/python).</span><span class="sxs-lookup"><span data-stu-id="b1019-173">[Learn more](/azure/cognitive-services/bing-spell-check/proof-text) about the Spell Check API and get started with the [Spell Check API Python quickstart](/azure/cognitive-services/bing-spell-check/quickstarts/python).</span></span>