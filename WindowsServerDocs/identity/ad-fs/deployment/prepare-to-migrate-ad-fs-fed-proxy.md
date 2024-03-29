---
title: Préparer la migration du serveur proxy de fédération AD FS 2.0
description: Fournit des informations sur la préparation migrer le serveur proxy AD FS vers Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b2275af0934413fa2de02de720d609feda7392c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444449"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Préparer la migration du serveur proxy de fédération AD FS 2.0

Pour préparer la migration d’un serveur proxy de fédération 2.0 AD FS vers Windows Server 2012, vous devez exporter et sauvegarder les données de configuration AD FS à partir de ce serveur proxy.  Les étapes décrites dans cette rubrique s’appliquent à un scénario avec un ou plusieurs serveurs proxy de fédération.  
  
 Pour exporter les données de configuration AD FS, effectuez les tâches suivantes :  
  
-   [Étape 1 : Exporter les paramètres de service de proxy](#step-1-export-proxy-service-settings)  
  
-   [Étape 2 : Sauvegarder les personnalisations de page Web](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>Étape 1 : exporter les paramètres de service proxy  
 Pour exporter les paramètres de service proxy de serveur de fédération, effectuez la procédure suivante :  
  
### <a name="to-export-proxy-service-settings"></a>Pour exporter les paramètres de service proxy  
  
1.  Exporter le certificat SSL et sa clé privée dans un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Cette étape est facultative, car ce certificat est préservé pendant la mise à niveau du système d’exploitation.  
  
2. Exportez les propriétés du proxy AD FS 2.0 de fédération vers un fichier. Pour ce faire, utilisez Windows PowerShell.  
  
Ouvrez Windows PowerShell et exécutez la commande suivante pour ajouter les applets de commande AD FS à votre session Windows PowerShell : `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Ensuite, exécutez la commande suivante pour exporter les propriétés de proxy de fédération vers un fichier : `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3. Vérifiez que vous connaissez les informations d’identification d’un compte administrateur du serveur de fédération AD FS ou d’un compte de service sous lequel s’exécute le service de fédération AD FS.  Ces informations sont requises pour la configuration d’approbation de proxy.  
  
   La réalisation de cette étape permet de rassembler les informations suivantes requises pour configurer votre serveur proxy de fédération AD FS :  
  
-   Nom du service de fédération AD FS  
  
-   Nom du compte de domaine requis pour la configuration d’approbation de proxy  
  
-   Adresse et port du proxy HTTP (s’il existe un proxy HTTP entre le serveur proxy de fédération AD FS et les serveurs de fédération AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Étape 2 : sauvegarder les personnalisations de page web  
 Pour sauvegarder les personnalisations de page web, copiez les pages web proxy AD FS et le fichier **web.config** depuis le répertoire qui est mappé sur le chemin d’accès virtuel **“/adfs/ls”** dans IIS.  L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  
  
## <a name="next-steps"></a>Étapes suivantes
 [Préparer la migration du serveur AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-server.md)   
 [Préparer la migration du serveur Proxy pour AD FS 2.0 de fédération](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrer le serveur AD FS 2.0 de fédération](migrate-the-ad-fs-fed-server.md)   
 [Migrer le serveur Proxy AD FS 2.0 de fédération](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrer les agents web AD FS 1.1](migrate-the-ad-fs-web-agent.md)