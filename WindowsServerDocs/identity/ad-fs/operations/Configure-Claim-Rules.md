---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurer des règles de revendications
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d0dd8528e5fbd6829b313a3e6bc47f5a17f6a12f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189719"
---
# <a name="configure-claim-rules"></a>Configurer les règles de revendication

Dans un revendications\-modèle d’identité basé sur, la fonction d’Active Directory Federation Services \(AD FS\) federation services consiste à émettre un jeton qui contient un ensemble de revendications. Règles de revendications régissent les décisions en ce qui concerne les revendications AD FS émet. Les règles de revendication et toutes les données de configuration de serveur sont stockées dans la base de données de configuration AD FS.  
  
AD FS prend des décisions d’émission qui reposent sur les informations d’identité qui sont fournies à ce dernier sous la forme de revendications et d’autres informations contextuelles. À un niveau élevé, AD FS fonctionne qu’un processeur de règles en effectuant l’une des revendications en tant qu’entrée, effectue un certain nombre de transformations, puis retourne un ensemble différent de revendications en tant que sortie. 

Les rubriques suivantes vous aidera à créer les règles qui traitera les services AD FS : 
  
-   [Créer une règle pour passer ou filtrer une revendication entrante](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Créer une règle pour autoriser tous les utilisateurs](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Créer une règle pour autoriser ou refuser des utilisateurs en fonction d’une demande entrante](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer les attributs LDAP en tant que revendications](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Créer une règle pour envoyer l’appartenance à un groupe en tant que revendication](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Créer une règle pour transformer une revendication entrante](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Créer une règle pour envoyer une revendication de méthode d’authentification](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Créer une règle pour envoyer une revendication Compatible de AD FS 1.x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Créer une règle pour envoyer des revendications à l’aide d’une règle personnalisée](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
