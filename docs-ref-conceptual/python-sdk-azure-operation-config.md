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
# <a name="operation-config"></a>Configurazione delle operazioni 

I metodi per le operazioni includono parametri aggiuntivi che possono essere forniti in `kwargs`. Questo approccio è definito operation_config.

Le opzioni per la configurazione delle operazioni sono le seguenti:

|Nome parametro|Type|Ruolo|
|----------------------|------|---------------|
| verify |`bool`|Indica se verificare il certificato SSL. Il valore predefinito è true.|
|  cert |`str`| Percorso per il certificato locale per la verifica lato client.|
|  timeout |`int`| Timeout in secondi per stabilire una connessione al server.|
|  allow_redirects |`bool` | Indica se consentire i reindirizzamenti.|
|  max_redirects  |`int`| Numero massimo di reindirizzamenti consentiti.|
|  proxies  |`dict` |Impostazioni del server proxy.|
|  use_env_proxies |`bool` |Indica se leggere le impostazioni del proxy dalle variabili di ambiente locali.|
|  retries  |`int` | Numero totale di tentativi.|
|  enable_http_logger | `bool`| Abilita i log di HTTP in modalità di debug (False per impostazione predefinita).|
