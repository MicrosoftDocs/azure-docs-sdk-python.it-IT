---
title: Installazione
description: Come installare Azure Python SDK
keywords: Azure, Python, SDK, API
author: lisawong19
ms.author: liwong
manager: douge
ms.date: 06/05/2017
ms.topic: install
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.assetid: 
ms.openlocfilehash: 1ee0c110673f908832c1c9e8fd6dac4c90c16e8e
ms.sourcegitcommit: ce2921d9a6f21d58fdf77cbc843c9b4af0ea796d
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/31/2017
---
# <a name="installation"></a><span data-ttu-id="82cd0-104">Installazione</span><span class="sxs-lookup"><span data-stu-id="82cd0-104">Installation</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="82cd0-105">Installazione con pip</span><span class="sxs-lookup"><span data-stu-id="82cd0-105">Installation with pip</span></span>

<span data-ttu-id="82cd0-106">È possibile installare singolarmente la libreria di ogni servizio di Azure:</span><span class="sxs-lookup"><span data-stu-id="82cd0-106">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="82cd0-107">I pacchetti di anteprima possono essere installati mediante il flag `--pre` :</span><span class="sxs-lookup"><span data-stu-id="82cd0-107">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="82cd0-108">È inoltre possibile installare un set di librerie di Azure in una singola riga utilizzando il meta pacchetto `azure` .</span><span class="sxs-lookup"><span data-stu-id="82cd0-108">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="82cd0-109">Viene pubblicata una versione di anteprima del pacchetto, a cui è possibile accedere tramite il flag --pre:</span><span class="sxs-lookup"><span data-stu-id="82cd0-109">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="82cd0-110">Eseguire l'installazione da GitHub</span><span class="sxs-lookup"><span data-stu-id="82cd0-110">Install from GitHub</span></span>

<span data-ttu-id="82cd0-111">Se si vuole installare `azure` dall'origine:</span><span class="sxs-lookup"><span data-stu-id="82cd0-111">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install
