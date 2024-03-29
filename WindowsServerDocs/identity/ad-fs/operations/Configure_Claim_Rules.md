---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurer des règles de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e7eedd907c07c2aaef1670c5db3a6892ca3e650d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189628"
---
# <a name="configure-claim-rules"></a>Configurer les règles de revendication

Dans un revendications\-modèle d’identité basé sur la fonction d’Active Directory Federation Services (AD FS) en tant que services de fédération consiste à émettre un jeton qui contient un ensemble de revendications. Règles de revendications régissent les décisions en ce qui concerne les revendications AD FS émet. Les règles de revendication et toutes les données de configuration de serveur sont stockées dans la base de données de configuration AD FS.  
  
AD FS prend des décisions d’émission qui reposent sur les informations d’identité qui sont fournies à ce dernier sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, AD FS fonctionne qu’un processeur de règles en effectuant l’une des revendications en tant qu’entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en tant que sortie. 

Les rubriques suivantes vous aidera à créer les règles qui traitera les services AD FS : 
  
-   [Créer une règle pour passer ou filtrer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer les attributs LDAP en tant que revendications](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
