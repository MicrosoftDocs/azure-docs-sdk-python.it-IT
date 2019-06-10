---
title: Librerie del Controllo degli accessi in base al ruolo di Graph per Python
description: Informazioni di riferimento sulle librerie del Controllo degli accessi in base al ruolo di Graph per Python
keywords: Azure, Python, SDK, API, Controllo degli accessi in base al ruolo di Graph
author: rloutlaw
ms.author: routlaw
manager: jfriend
ms.date: 05/10/2019
ms.topic: article
ms.prod: azure
ms.technology: azure
ms.devlang: python
ms.service: multiple
ms.openlocfilehash: 27238e00463ae30ec0e47e8c18497ffb9edac62c
ms.sourcegitcommit: 253c8d4b3dbc2bb76d1a273a757ab96ba37617a1
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2019
ms.locfileid: "65731537"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a><span data-ttu-id="c11b7-104">Librerie di Azure Active Directory Graph per Python</span><span class="sxs-lookup"><span data-stu-id="c11b7-104">Azure Active Directory Graph libraries for Python</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="c11b7-105">A partire da febbraio 2019 è stato avviato il processo per deprecare alcune versioni precedenti di API Graph di Azure Active Directory e sostituirle con l'API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c11b7-105">As of February 2019, we started the process to deprecate some earlier versions of Azure Active Directory Graph API in favor of the Microsoft Graph API.</span></span> 
>
> <span data-ttu-id="c11b7-106">Per altre informazioni, aggiornamenti e tempistiche, vedere [Microsoft Graph o Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) su Office Dev Center.</span><span class="sxs-lookup"><span data-stu-id="c11b7-106">For details, updates, and time frames, see [Microsoft Graph or the Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) in the Office Dev Center.</span></span>
>
> <span data-ttu-id="c11b7-107">In futuro le applicazioni dovranno usare l'API Microsoft Graph.</span><span class="sxs-lookup"><span data-stu-id="c11b7-107">Moving forward, applications should use the Microsoft Graph API.</span></span> 

## <a name="overview"></a><span data-ttu-id="c11b7-108">Panoramica</span><span class="sxs-lookup"><span data-stu-id="c11b7-108">Overview</span></span> 

<span data-ttu-id="c11b7-109">Con [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-apis) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.</span><span class="sxs-lookup"><span data-stu-id="c11b7-109">Sign-on users and control access to applications and APIs with [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-apis).</span></span>   

## <a name="client-library"></a><span data-ttu-id="c11b7-110">Libreria client</span><span class="sxs-lookup"><span data-stu-id="c11b7-110">Client library</span></span>   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a><span data-ttu-id="c11b7-111">Esempio</span><span class="sxs-lookup"><span data-stu-id="c11b7-111">Example</span></span> 
> [!NOTE]   
> <span data-ttu-id="c11b7-112">È necessario modificare il parametro della risorsa in https://graph.windows.net durante la creazione dell'istanza delle credenziali</span><span class="sxs-lookup"><span data-stu-id="c11b7-112">You need to change the resource parameter to https://graph.windows.net while creating the credentials instance</span></span>    
 ```python  
from azure.graphrbac import GraphRbacManagementClient   
from azure.common.credentials import UserPassCredentials    
 credentials = UserPassCredentials( 
            'user@domain.com',      # Your user 
            'my_password',          # Your password 
            resource="https://graph.windows.net"    
    )   
 tenant_id = "myad.onmicrosoft.com" 
 graphrbac_client = GraphRbacManagementClient(  
    credentials,    
    tenant_id   
)   
``` 
<span data-ttu-id="c11b7-113">Il codice seguente consente di creare un utente, di ottenerlo direttamente e tramite l'applicazione di filtri all'elenco e quindi di eliminarlo.</span><span class="sxs-lookup"><span data-stu-id="c11b7-113">The following code creates a user, get it directly and by list filtering, and then delete it.</span></span>   
```python   
from azure.graphrbac.models import UserCreateParameters, PasswordProfile    
 user = graphrbac_client.users.create(  
    UserCreateParameters(   
        user_principal_name="testbuddy@{}".format(MY_AD_DOMAIN),    
        account_enabled=False,  
        display_name='Test Buddy',  
        mail_nickname='testbuddy',  
        password_profile=PasswordProfile(   
            password='MyStr0ngP4ssword',    
            force_change_password_next_login=True   
        )   
    )   
)   
# user is a User instance   
self.assertEqual(user.display_name, 'Test Buddy')   
 user = graphrbac_client.users.get(user.object_id)  
self.assertEqual(user.display_name, 'Test Buddy')   
 for user in graphrbac_client.users.list(filter="displayName eq 'Test Buddy'"): 
    self.assertEqual(user.display_name, 'Test Buddy')   
 graphrbac_client.users.delete(user.object_id)  
```