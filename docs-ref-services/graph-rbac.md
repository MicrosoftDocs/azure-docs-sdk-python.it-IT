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
ms.openlocfilehash: e9b0aba7998565284ae18e0036da96d033b2b05f
ms.sourcegitcommit: 46bebbf5dd558750043ce5afadff2ec3714a54e6
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2019
ms.locfileid: "67534272"
---
# <a name="azure-active-directory-graph-libraries-for-python"></a>Librerie di Azure Active Directory Graph per Python

> [!IMPORTANT]
>
> A partire da febbraio 2019 è stato avviato il processo per deprecare alcune versioni precedenti di API Graph di Azure Active Directory e sostituirle con l'API Microsoft Graph. 
>
> Per altre informazioni, aggiornamenti e tempistiche, vedere [Microsoft Graph o Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) su Office Dev Center.
>
> In futuro le applicazioni dovranno usare l'API Microsoft Graph. 

## <a name="overview"></a>Panoramica 

Con [Active Directory Graph](/azure/active-directory/develop/active-directory-graph-api) è possibile consentire l'accesso degli utenti e controllare l'accesso ad applicazioni e API.    

## <a name="client-library"></a>Libreria client   

 ```bash    
pip install azure-graphrbac 
``` 

### <a name="example"></a>Esempio 
> [!NOTE]   
> È necessario modificare il parametro della risorsa in https://graph.windows.net durante la creazione dell'istanza delle credenziali    
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
Il codice seguente consente di creare un utente, di ottenerlo direttamente e tramite l'applicazione di filtri all'elenco e quindi di eliminarlo.   
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