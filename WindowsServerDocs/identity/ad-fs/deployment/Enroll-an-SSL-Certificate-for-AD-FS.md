---
ms.assetid: 3095e6a7-b562-4c6a-bf29-13b32c133cac
title: Inscrire un certificat SSL pour AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: e80927f2670614d2949f4e67cc158319f05c5fa0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192154"
---
# <a name="enroll-an-ssl-certificate-for-ad-fs"></a>Inscrire un certificat SSL pour AD FS

Active Directory Federation Services \(AD FS\) nécessite un certificat pour le protocole SSL (Secure Socket Layer) \(SSL\) authentification serveur sur chaque serveur de fédération dans votre batterie de serveurs de fédération. Le même certificat peut être utilisé sur chaque serveur de fédération dans une batterie de serveurs. Le certificat et sa clé privée doivent être disponibles. Par exemple, si le certificat et sa clé privée se trouvent dans un fichier .pfx, vous pouvez importer le fichier directement dans l’Assistant Configuration des services AD FS (Active Directory Federation Services). Ce certificat SSL doit contenir les éléments suivants :  
  
1.  Le nom de sujet et autre nom d’objet doivent contenir votre nom de service de fédération, tel que fs.contoso.com.  
  
2.  L’autre nom de sujet doit contenir la valeur **enterpriseregistration** est suivi par le nom d’utilisateur Principal \(UPN\) suffixe de votre organisation, par exemple,  **enterpriseregistration.corp.contoso.com**.  
  
    > [!WARNING]  
    > Spécifiez le champ autre nom si vous prévoyez d’activer le Service Device Registration \(DRS\) pour la jonction.  
  
> [!IMPORTANT]  
> Si votre organisation utilise plusieurs suffixes UPN, et que vous prévoyez d’activer le service DRS, le certificat SSL doit contenir une entrée d’autre nom de l’objet pour chaque suffixe.  
  
## <a name="see-also"></a>Voir aussi
[Déploiement d’AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guide de déploiement de Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Déploiement d’une batterie de serveurs de fédération](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
  

