---
title: Installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé par le point de connexion de service
description: Ce guide fournit des instructions sur le déploiement de BranchCache en mode de cache hébergé sur les ordinateurs exécutant Windows Server 2016 et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: article
ms.assetid: 9adf420b-5a58-4e59-9906-71bd58f757fd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6619b09df0d4c161148d22091337a5039c7ea3af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849650"
---
# <a name="install-the-branchcache-feature-and-configure-the-hosted-cache-server-by-service-connection-point"></a>Installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé par le point de connexion de service

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez utiliser cette procédure pour installer la fonctionnalité BranchCache sur votre serveur de cache hébergé, HCS1 et pour configurer le serveur pour inscrire un Point de connexion de Service \(SCP\) dans les Services de domaine Active Directory \(AD DS\).

Lorsque vous inscrivez les serveurs de cache hébergé avec un SCP dans AD DS, le SCP permet aux ordinateurs clients qui sont configurés correctement pour découvrir automatiquement les serveurs de cache hébergé en interrogeant les services AD DS pour le SCP. Instructions sur la façon de configurer des ordinateurs clients pour effectuer cette action sont fournies plus loin dans ce guide.

>[!IMPORTANT]
>Avant d’effectuer cette procédure, vous devez joindre l’ordinateur au domaine et configurez l’ordinateur avec une adresse IP statique.

Pour effectuer cette procédure, vous devez être membre du groupe Administrateurs.

## <a name="to-install-the-branchcache-feature-and-configure-the-hosted-cache-server"></a>Pour installer la fonctionnalité BranchCache et configurer le serveur de cache hébergé  

1. Sur l’ordinateur serveur, exécutez Windows PowerShell en tant qu’administrateur. Tapez la commande suivante et appuyez sur ENTRÉE.

    ``` 
    Install-WindowsFeature BranchCache
    ```

2.  Pour configurer l’ordinateur en tant que serveur de cache hébergé une fois que la fonctionnalité BranchCache est installée et pour inscrire un Point de connexion de Service dans AD DS, tapez la commande suivante dans Windows PowerShell, puis appuyez sur ENTRÉE.

    ```  
    Enable-BCHostedServer -RegisterSCP
    ```  

3. Pour vérifier la configuration de serveur de cache hébergé, tapez la commande suivante et appuyez sur ENTRÉE.

    ```  
    Get-BCStatus  
    ```  
  
    Les résultats de la commande affichent l’état de tous les aspects de votre installation de BranchCache. Voici quelques-uns des paramètres BranchCache et la valeur correcte pour chaque élément :  
  
    -   BranchCacheIsEnabled: True

    -   HostedCacheServerIsEnabled: True

    -   HostedCacheScpRegistrationEnabled: True

4. Pour préparer l’étape de copie de vos packages de données à partir de vos serveurs de contenu à vos serveurs de cache hébergé, identifier un partage existant sur le serveur de cache hébergé, ou créez un dossier et partagez le dossier afin qu’il soit accessible à partir de vos serveurs de contenu. Une fois que vous créez vos packages de données sur vos serveurs de contenu, vous allez copier les packages de données sur ce dossier partagé sur le serveur de cache hébergé.
  
5. Si vous déployez plusieurs serveurs de cache hébergé, répétez cette procédure sur chaque serveur.

Pour continuer avec ce guide, consultez [déplacez et redimensionnez le Cache hébergé &#40;facultatif&#41;](6-Bc-Move-Resize-Cache.md).
