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
ms.openlocfilehash: 1f1dbaa77ee02a1da9386642e001f0a1a63a6599
ms.sourcegitcommit: 61cc12fd4bb1a3ad5f9b79d1c616f005bc21c5d9
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/06/2018
---
# <a name="azure-cognitive-services-modules-for-python"></a>Moduli di Servizi cognitivi di Azure per Python

Aggiungere il riconoscimento di immagini e volti, l'analisi della lingua e funzionalità di ricerca ad app, siti Web e strumenti Python usando i moduli di Servizi cognitivi di Azure per Python.

## <a name="vision-modules"></a>Moduli di Visione

### <a name="computer-vision"></a>Visione artificiale 

Restituisce informazioni sul contenuto visivo presente in un'immagine:

- Usare l'aggiunta di tag, le descrizioni e i modelli specifici di un dominio per identificare i contenuti ed etichettarli in tutta sicurezza.
- Applicare le impostazioni relative ai contenuti per adulti per limitare automaticamente i contenuti di questo tipo.
- Identificare i tipi di immagine e le combinazioni colori nelle immagini.

[Provare Visione artificiale](https://azure.microsoft.com/en-us/services/cognitive-services/computer-vision/) gratuitamente nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-vision-computervision
```

Vedere [altre informazioni](/azure/cognitive-services/computer-vision/home) sull'API Visione artificiale e la [Computer Vision API Python quickstart](/azure/cognitive-services/computer-vision/quickstarts/python) (Guida introduttiva all'API Visione artificiale per Python).

### <a name="content-moderator"></a>Content Moderator

Moderazione di testo, video e immagini con supporto di computer, ottimizzata da strumenti di revisione umana.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-vision-computervision
```

[Altre informazioni](/azure/cognitive-services/content-moderator/overview) sul servizio Content Moderator.

### <a name="custom-vision-service"></a>Servizio visione personalizzata

Caricare immagini per eseguire il training e personalizzare un modello di visione artificiale per il caso d'uso specifico. Dopo aver eseguito il training del modello, è possibile usare l'API per contrassegnare le immagini con il modello e valutare i risultati per migliorare il classificatore.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-vision-customvision
```

Vedere [altre informazioni](/azure/cognitive-services/Custom-Vision-Service/home) sul servizio Visione artificiale personalizzato e la [Custom Vision Python tutorial](/azure/cognitive-services/Custom-Vision-Service/python-tutorial) (Esercitazione sul servizio Visione artificiale personalizzato per Python)

### <a name="face-api"></a>API Viso

Rilevare, identificare, analizzare, organizzare e contrassegnare con tag i visi nelle foto. 

[Provare l'API Viso](https://azure.microsoft.com/en-us/services/cognitive-services/face/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install cognitive-face
```

Vedere [altre informazioni](/azure/cognitive-services/face/overview) sull'API Viso e la [Guida introduttiva all'API Viso per Python](/azure/cognitive-services/Face/Tutorials/FaceAPIinPythonTutorial).

## <a name="search-modules"></a>Moduli di ricerca

### <a name="web-search"></a>Ricerca Web

Recuperare documenti Web indicizzati dall'API Ricerca Web Bing e limitare i risultati filtrandoli in base a tipo, aggiornamento e altro. 

[Provare l'API Ricerca Web](https://azure.microsoft.com/en-us/services/cognitive-services/bing-web-search-api/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-websearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-web-search/overview) sull'API Ricerca Web Bing e la [Guida introduttiva all'API Ricerca Web per Python](/azure/cognitive-services/bing-web-search/quickstarts/python).

### <a name="image-search"></a>Ricerca di immagini

Cercare immagini e ottenere anteprime, URL di immagini completi, metadati delle immagini e molto altro.

[Provare l'API Ricerca immagini](https://azure.microsoft.com/en-us/services/cognitive-services/bing-image-search-api/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-imagesearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-image-search/overview) sull'API Ricerca immagini Bing e la [Guida introduttiva all'API Ricerca immagini per Python](/azure/cognitive-services/bing-image-search/quickstarts/python).


### <a name="entity-search"></a>Ricerca entità

Cercare l'entità più pertinente (luogo, persona o oggetto) per un determinato termine di ricerca o luogo.

[Provare l'API Ricerca entità](https://azure.microsoft.com/services/cognitive-services/bing-entity-search-api/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-entitysearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-entities-search/search-the-web) sull'API Ricerca entità Bing e la [Guida introduttiva all'API Ricerca entità per Python](/azure/cognitive-services/bing-entities-search/quickstarts/python).

### <a name="custom-search"></a>Ricerca personalizzata

Compilare una ricerca Web personalizzata in funzione del dominio di ricerca specifico.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-customsearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-custom-search/) sul servizio Ricerca personalizzata Bing e la [Guida introduttiva all'API Ricerca personalizzata per Python](/azure/cognitive-services/bing-custom-search/call-endpoint-python) per iniziare a eseguire query di ricerca personalizzata da Python.

### <a name="video-search"></a>Ricerca video

Trovare video nel Web e ottenere risultati con metadati relativi ad autore, codifica, durata e conteggio delle visualizzazioni.

[Provare l'API Ricerca video](https://azure.microsoft.com/services/cognitive-services/bing-video-search-api/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-videosearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-video-search/search-the-web) sull'API Ricerca video Bing e la [Guida introduttiva all'API Ricerca video per Python](/azure/cognitive-services/bing-video-search/python).


### <a name="news-search"></a>Ricerca notizie

Cercare articoli di notizie sul Web e usare i metadati relativi ad articoli, notizie correlate, immagini e informazioni del provider.

[Provare l'API Ricerca notizie](https://azure.microsoft.com/services/cognitive-services/bing-news-search-api/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-search-newssearch
```

Vedere [altre informazioni](/azure/cognitive-services/bing-news-search/search-the-web) sull'API Ricerca notizie Bing e la [Guida introduttiva all'API Ricerca notizie per Python](//azure/cognitive-services/bing-news-search/python).


## <a name="language-modules"></a>Moduli per la lingua

### <a name="text-analytics"></a>Text Analytics 

L'API Analisi del testo è un servizio basato su cloud che fornisce l'elaborazione in linguaggio naturale su testo non elaborato. L'API include tre funzioni principali:

- Analisi del sentiment
- Estrazione di frasi chiave
- Rilevamento della lingua

[Provare l'API Analisi del testo](https://azure.microsoft.com/en-us/services/cognitive-services/text-analytics/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-language-textanalytics
```

Vedere [altre informazioni](/azure/cognitive-services/text-analytics/overview) sull'API Analisi del testo e la [Guida introduttiva all'API Analisi del testo per Python](/azure/cognitive-services/text-analytics/quickstarts/python).


### <a name="spell-check"></a>Controllo ortografico

Eseguire il controllo grammaticale e ortografico contestuale con l'API Controllo ortografico Bing.

[Provare l'API Controllo ortografico](https://azure.microsoft.com/en-us/services/cognitive-services/spell-check/) nel browser.

Ottenere il modulo Python con [pip](https://pip.pypa.io/en/stable/quickstart/):

```
pip install azure-cognitiveservices-language-spellcheck
```

Vedere [altre informazioni](/azure/cognitive-services/bing-spell-check/proof-text) sull'API Controllo ortografico e la [Guida introduttiva all'API Controllo ortografico per Python](/azure/cognitive-services/bing-spell-check/quickstarts/python).