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
ms.openlocfilehash: d7be7cd76d019c6741d93c04458376a9352e363b
ms.sourcegitcommit: 41e6e6b5469271f4ec497a322b460e2a2af2c73d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
ms.locfileid: "30204260"
---
# <a name="operation-config"></a>Configurazione delle operazioni 

I metodi per le operazioni includono parametri aggiuntivi che possono essere forniti in kwarg. Questo approccio è definito operation_config.

Le opzioni per la configurazione delle operazioni sono le seguenti:

|Nome parametro|type|Ruolo|
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
