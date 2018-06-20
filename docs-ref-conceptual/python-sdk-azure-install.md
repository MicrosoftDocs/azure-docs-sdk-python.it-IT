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
ms.openlocfilehash: 792feac12f8328e2467017530065350e347c59b7
ms.sourcegitcommit: 757bf84535fd9d8299c4b51ec92a5ab1926cb671
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 02/27/2018
ms.locfileid: "29565820"
---
# <a name="installation"></a><span data-ttu-id="b4e85-104">Installazione</span><span class="sxs-lookup"><span data-stu-id="b4e85-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="b4e85-105">Tipo e versione di Python da utilizzare</span><span class="sxs-lookup"><span data-stu-id="b4e85-105">Which Python and which version to use</span></span>
<span data-ttu-id="b4e85-106">Sono disponibili diversi interpreti Python, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="b4e85-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="b4e85-107">CPython: l'interprete Python standard e più comunemente usato</span><span class="sxs-lookup"><span data-stu-id="b4e85-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="b4e85-108">PyPy - implementazione alternativa rapida e conforme a CPython</span><span class="sxs-lookup"><span data-stu-id="b4e85-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="b4e85-109">IronPython: interprete Python eseguito in .NET/CLR</span><span class="sxs-lookup"><span data-stu-id="b4e85-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="b4e85-110">Jython: interprete Python eseguito sulla macchina virtuale Java</span><span class="sxs-lookup"><span data-stu-id="b4e85-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="b4e85-111">**CPython** versione v2.7 o v3.4+ e PyPy 5.4.0 sono testati e supportati per Python Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="b4e85-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="b4e85-112">Dove è possibile reperire Python</span><span class="sxs-lookup"><span data-stu-id="b4e85-112">Where to get Python?</span></span>
<span data-ttu-id="b4e85-113">È possibile ottenere CPython in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="b4e85-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="b4e85-114">Direttamente da [Python](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="b4e85-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="b4e85-115">Da un distributore attendibile, ad esempio [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) o [ActiveState](https://www.activestate.com/)</span><span class="sxs-lookup"><span data-stu-id="b4e85-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="b4e85-116">Compilandolo da codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="b4e85-116">Build from source!</span></span>

<span data-ttu-id="b4e85-117">Salvo esigenze specifiche, si consiglia di scegliere le prime due opzioni.</span><span class="sxs-lookup"><span data-stu-id="b4e85-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="b4e85-118">Installazione con pip</span><span class="sxs-lookup"><span data-stu-id="b4e85-118">Installation with pip</span></span>

<span data-ttu-id="b4e85-119">È possibile installare singolarmente la libreria di ogni servizio di Azure:</span><span class="sxs-lookup"><span data-stu-id="b4e85-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="b4e85-120">I pacchetti di anteprima possono essere installati mediante il flag `--pre` :</span><span class="sxs-lookup"><span data-stu-id="b4e85-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="b4e85-121">È inoltre possibile installare un set di librerie di Azure in una singola riga utilizzando il meta pacchetto `azure` .</span><span class="sxs-lookup"><span data-stu-id="b4e85-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="b4e85-122">Viene pubblicata una versione di anteprima del pacchetto, a cui è possibile accedere tramite il flag --pre:</span><span class="sxs-lookup"><span data-stu-id="b4e85-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="b4e85-123">Eseguire l'installazione da GitHub</span><span class="sxs-lookup"><span data-stu-id="b4e85-123">Install from GitHub</span></span>

<span data-ttu-id="b4e85-124">Se si vuole installare `azure` dall'origine:</span><span class="sxs-lookup"><span data-stu-id="b4e85-124">If you want to install `azure` from source:</span></span>

    git clone git://github.com/Azure/azure-sdk-for-python.git
    cd azure-sdk-for-python
    python setup.py install