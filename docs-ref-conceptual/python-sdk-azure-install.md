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
ms.openlocfilehash: 9fd11cbc7b987b970ceee85c7b11b22e3d6299ea
ms.sourcegitcommit: 31d7df367b15ec09a5a610eb333295bba0f6b351
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/26/2019
ms.locfileid: "67395442"
---
# <a name="installation"></a><span data-ttu-id="d8656-104">Installazione</span><span class="sxs-lookup"><span data-stu-id="d8656-104">Installation</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="d8656-105">Tipo e versione di Python da utilizzare</span><span class="sxs-lookup"><span data-stu-id="d8656-105">Which Python and which version to use</span></span>

<span data-ttu-id="d8656-106">Sono disponibili diversi interpreti Python, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="d8656-106">There are several Python interpreters available - examples include:</span></span>

* <span data-ttu-id="d8656-107">CPython: l'interprete Python standard e più comunemente usato</span><span class="sxs-lookup"><span data-stu-id="d8656-107">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="d8656-108">PyPy - implementazione alternativa rapida e conforme a CPython</span><span class="sxs-lookup"><span data-stu-id="d8656-108">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="d8656-109">IronPython: interprete Python eseguito in .NET/CLR</span><span class="sxs-lookup"><span data-stu-id="d8656-109">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="d8656-110">Jython: interprete Python eseguito sulla macchina virtuale Java</span><span class="sxs-lookup"><span data-stu-id="d8656-110">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="d8656-111">**CPython** versione v2.7 o v3.4+ e PyPy 5.4.0 sono testati e supportati per Python Azure SDK.</span><span class="sxs-lookup"><span data-stu-id="d8656-111">**CPython** v2.7 or v3.4+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="d8656-112">Dove è possibile reperire Python</span><span class="sxs-lookup"><span data-stu-id="d8656-112">Where to get Python?</span></span>

<span data-ttu-id="d8656-113">È possibile ottenere CPython in diversi modi:</span><span class="sxs-lookup"><span data-stu-id="d8656-113">There are several ways to get CPython:</span></span>

* <span data-ttu-id="d8656-114">Direttamente da [Python](https://www.python.org/)</span><span class="sxs-lookup"><span data-stu-id="d8656-114">Directly from [Python](https://www.python.org/)</span></span>
* <span data-ttu-id="d8656-115">Da un distributore attendibile, ad esempio [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) o [ActiveState](https://www.activestate.com/)</span><span class="sxs-lookup"><span data-stu-id="d8656-115">From a reputable distro such as [Anaconda](https://www.anaconda.com/), [Enthought](https://www.enthought.com/) or [ActiveState](https://www.activestate.com/)</span></span>
* <span data-ttu-id="d8656-116">Compilandolo da codice sorgente.</span><span class="sxs-lookup"><span data-stu-id="d8656-116">Build from source!</span></span>

<span data-ttu-id="d8656-117">Salvo esigenze specifiche, si consiglia di scegliere le prime due opzioni.</span><span class="sxs-lookup"><span data-stu-id="d8656-117">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="installation-with-pip"></a><span data-ttu-id="d8656-118">Installazione con pip</span><span class="sxs-lookup"><span data-stu-id="d8656-118">Installation with pip</span></span>

<span data-ttu-id="d8656-119">È possibile installare singolarmente la libreria di ogni servizio di Azure:</span><span class="sxs-lookup"><span data-stu-id="d8656-119">You can install each Azure service's library individually:</span></span>

```bash
pip install azure-batch          # Install the latest Batch runtime library
pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="d8656-120">I pacchetti di anteprima possono essere installati mediante il flag `--pre` :</span><span class="sxs-lookup"><span data-stu-id="d8656-120">Preview packages can be installed using the `--pre` flag:</span></span>

```bash
pip install --pre azure-mgmt-compute # will install only the latest Compute Management library
```

<span data-ttu-id="d8656-121">È inoltre possibile installare un set di librerie di Azure in una singola riga utilizzando il meta pacchetto `azure` .</span><span class="sxs-lookup"><span data-stu-id="d8656-121">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span>

```bash
pip install azure
```

<span data-ttu-id="d8656-122">Viene pubblicata una versione di anteprima del pacchetto, a cui è possibile accedere tramite il flag --pre:</span><span class="sxs-lookup"><span data-stu-id="d8656-122">We publish a preview version of this package, which you can access using the --pre flag:</span></span>

```bash
pip install --pre azure
```

## <a name="install-from-github"></a><span data-ttu-id="d8656-123">Eseguire l'installazione da GitHub</span><span class="sxs-lookup"><span data-stu-id="d8656-123">Install from GitHub</span></span>

<span data-ttu-id="d8656-124">Se si vuole installare `azure` dall'origine:</span><span class="sxs-lookup"><span data-stu-id="d8656-124">If you want to install `azure` from source:</span></span>

```bash
git clone git://github.com/Azure/azure-sdk-for-python.git
cd azure-sdk-for-python
python setup.py install
```

## <a name="install-an-older-version-with-pip"></a><span data-ttu-id="d8656-125">Installare una versione precedente con pip</span><span class="sxs-lookup"><span data-stu-id="d8656-125">Install an older version with pip</span></span>
<span data-ttu-id="d8656-126">È possibile installare una versione precedente di `azure` specificando 'azure==3.0.0' come dettagli della versione.</span><span class="sxs-lookup"><span data-stu-id="d8656-126">You can install an older version of `azure` by specifying 'azure==3.0.0' version details.</span></span>
```bash
pip install azure==3.0.0 
```
## <a name="check-sdk-installation-details-with-pip"></a><span data-ttu-id="d8656-127">Controllare i dettagli dell'installazione dell'SDK con pip</span><span class="sxs-lookup"><span data-stu-id="d8656-127">Check SDK installation details with pip</span></span>
<span data-ttu-id="d8656-128">È possibile controllare il percorso di installazione e i dettagli della versione dell'SDK per `azure`, oltre ad altre informazioni.</span><span class="sxs-lookup"><span data-stu-id="d8656-128">You can check `azure` SDK installation location, version details etc.</span></span>
```bash
pip show azure # Show installed version, location details etc.
pip freeze     # Output installed packages in requirements format.
pip list       # List installed packages, including editables.
```
## <a name="to-uninstall-with-pip"></a><span data-ttu-id="d8656-129">Per eseguire la disinstallazione con pip</span><span class="sxs-lookup"><span data-stu-id="d8656-129">To uninstall with pip</span></span>
<span data-ttu-id="d8656-130">È possibile disinstallare un set di librerie di Azure in una singola riga usando il meta pacchetto `azure`.</span><span class="sxs-lookup"><span data-stu-id="d8656-130">You can uninstall all Azure libraries in a single line using the `azure` meta-package.</span></span>
```bash
pip uninstall azure 
```
> [!NOTE]
> <span data-ttu-id="d8656-131">Il comando `pip uninstall azure` consente di rimuovere il meta pacchetto `azure`, lasciando indietro i singoli pacchetti `azure-*`, oltre ad altri, quali `adal` e `msrest`.</span><span class="sxs-lookup"><span data-stu-id="d8656-131">`pip uninstall azure`removes the `azure` meta-package but leaves the individual `azure-*` packages behind (and others, like `adal` and `msrest` ).</span></span> <span data-ttu-id="d8656-132">Una peculiarità di Python e pip è che se sono presenti pacchetti con dipendenze, disinstallando il pacchetto iniziale le dipendenze non verranno disinstallate.</span><span class="sxs-lookup"><span data-stu-id="d8656-132">An aspect of Python and pip is that for all packages that have dependencies, uninstalling the initial package does not uninstall the dependencies.</span></span> <span data-ttu-id="d8656-133">Per rimuovere `azure-` e i pacchetti di supporto, eseguire il comando `pip freeze | grep 'azure-' | xargs pip uninstall -y`, quindi eseguire separatamente le disinstallazioni di adal, msrest e msrestazure.</span><span class="sxs-lookup"><span data-stu-id="d8656-133">To remove `azure-` and its supporting packages, run the command `pip freeze | grep 'azure-' | xargs pip uninstall -y` (and then perform individual uninstalls for adal, msrest, and msrestazure).</span></span>

