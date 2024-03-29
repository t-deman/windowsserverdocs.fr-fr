---
title: Déployer un serveur DirectAccess individuel à l’aide de l’Assistant Mise en route
description: Cette rubrique fait partie du guide de déployer un serveur DirectAccess unique à l’aide de la prise en main Assistant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eb0cf464-0668-40f8-8222-feb6bae6d3d5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: deb220ad5ee83e2849f962c588d6150892d990d6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281789"
---
# <a name="deploy-a-single-directaccess-server-using-the-getting-started-wizard"></a>Déployer un serveur DirectAccess individuel à l’aide de l’Assistant Mise en route

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit une introduction au scénario DirectAccess qui utilise un serveur DirectAccess unique et vous permet de déployer DirectAccess en quelques étapes simples.  
  
## <a name="before-you-begin-deploying-see-the-list-of-unsupported-configurations-known-issues-and-prerequisites"></a>Avant de commencer le déploiement, consultez la liste des configurations non prises en charge, des problèmes connus et des configurations requises  
Vous pouvez utiliser les rubriques suivantes pour passer en revue les conditions préalables et les autres informations avant de déployer DirectAccess.  
  
-   [Configurations non prises en charge DirectAccess](../../../remote-access/directaccess/DirectAccess-Unsupported-Configurations.md)  
  
-   [Conditions préalables pour le déploiement de DirectAccess](../../../remote-access/directaccess/Prerequisites-for-Deploying-DirectAccess.md)  
  
## <a name="BKMK_OVER"></a>Description du scénario  
Dans ce scénario, un ordinateur unique exécutant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, est configuré comme un serveur DirectAccess avec les paramètres par défaut en quelques étapes de l’Assistant facile, sans avoir à configurer les paramètres d’infrastructure telles en tant qu’une autorité de certification (CA) ou les groupes de sécurité Active Directory.  
  
> [!NOTE]  
> Si vous souhaitez configurer un déploiement avancé avec des paramètres personnalisés, voir [Deploy a Single DirectAccess Server with Advanced Settings](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Pour configurer un serveur DirectAccess de base, plusieurs étapes de planification et de déploiement sont nécessaires.  
  
### <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   Le Pare-feu Windows doit être activé sur tous les profils.  
  
-   Ce scénario est uniquement pris en charge vos ordinateurs clients exécutent Windows 10, Windows 8.1 ou Windows 8.  
  
-   Le protocole ISATAP n'est pas pris en charge sur le réseau d'entreprise. Si vous utilisez le protocole ISATAP, vous devez le supprimer et utiliser le protocole IPv6 natif.  
  
-   Une infrastructure à clé publique (PKI) n’est pas requise.  
  
-   Ce scénario n’est pas pris en charge pour le déploiement de l’authentification à deux facteurs. Les informations d’identification de domaine sont requises pour l’authentification.  
  
-   Déploie automatiquement DirectAccess sur tous les ordinateurs portables présents dans le domaine actuel.  
  
-   Le trafic vers Internet ne passe pas par le tunnel DirectAccess. La configuration de tunneling forcé n’est pas prise en charge.  
  
-   Le serveur DirectAccess est le serveur Emplacement réseau.  
  
-   La protection d'accès réseau (NAP) n'est pas prise en charge.  
  
-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande PowerShell n’est pas prise en charge.  
  
-   Déploiement multisite, maintenant ou à l’avenir, tout d’abord [déployer un serveur DirectAccess unique avec des paramètres avancés](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
### <a name="planning-steps"></a>Étapes de planification  
La planification comporte les deux phases suivantes :  
  
1.  Planification de l’infrastructure DirectAccess. Cette phase décrit la planification requise pour configurer l'infrastructure réseau avant de commencer le déploiement de DirectAccess. Elle inclut la planification de la topologie du réseau et du serveur, et celle du serveur Emplacement réseau.  
  
2.  Planification du déploiement de DirectAccess. Cette phase décrit les étapes de planification requises pour préparer le déploiement de DirectAccess. Elle inclut la planification des ordinateurs clients DirectAccess, de la configuration requise pour l'authentification des serveurs et clients, des paramètres VPN, des serveurs d'infrastructure, et des serveurs d'applications et d'administration.  
  
Pour les étapes de planification détaillées, consultez [planifier un déploiement avancé de DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
### <a name="deployment-steps"></a>Étapes de déploiement  
Le déploiement comporte les trois phases suivantes :  
  
1.  La configuration de la phase de ce à l’infrastructure DirectAccess inclut la configuration du réseau et routage, des paramètres de pare-feu si nécessaire, configuration des certificats, les serveurs DNS, les paramètres Active Directory et GPO et l’emplacement réseau DirectAccess serveur.  
  
2.  Configuration des paramètres du serveur DirectAccess. Cette phase inclut les étapes requises pour configurer les ordinateurs clients DirectAccess, le serveur DirectAccess, les serveurs d'infrastructure et les serveurs d'applications et d'administration.  
  
3.  Vérification du déploiement. Cette phase inclut les étapes pour vérifier que le déploiement fonctionne comme prévu.  
  
Pour connaître les étapes de déploiement détaillées, voir [Install and Configure Basic DirectAccess](../../../remote-access/directaccess/single-server-wizard/Install-and-Configure-Basic-DirectAccess.md).  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Le déploiement d’un serveur d’accès à distance individuel présente les caractéristiques suivantes :  
  
-   Facilité d’accès. Vous pouvez configurer les ordinateurs clients gérés exécutant Windows 10, Windows 8.1, Windows 8 ou Windows 7, en tant que clients DirectAccess. Ces clients peuvent accéder aux ressources réseau internes via DirectAccess chaque fois qu’ils se trouvent sur Internet sans avoir à se connecter via une connexion VPN. Les ordinateurs clients qui n’exécutent pas l’un de ces systèmes d’exploitation peuvent se connecter au réseau interne via des connexions VPN classiques.  
  
-   Facilité de gestion. Les ordinateurs clients DirectAccess qui se trouvent sur Internet peuvent être gérés à distance par des administrateurs d'accès à distance via DirectAccess, même si ces ordinateurs ne figurent pas dans le réseau interne de l'entreprise. Les ordinateurs clients qui ne répondent pas aux spécifications de l’entreprise peuvent être automatiquement mis à jour par les serveurs d’administration. DirectAccess et VPN sont gérés dans la même console et avec le même jeu d’Assistants. En outre, un ou plusieurs serveurs d’accès à distance peuvent être gérés à partir d’une seule console de gestion de l’accès à distance.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|Rôle Accès à distance|Le rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur ou de Windows PowerShell. Ce rôle englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et le service Routage et accès distant qui était auparavant un service de rôle sous le rôle de serveur Services de stratégie et d’accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />1.  DirectAccess et le routage et accès distant VPN (RRAS) de Services. DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />2.  Routage RRAS. Les fonctionnalités de routage RRAS sont gérées dans la console Routage et accès distant héritée.<br /><br />Le rôle de serveur d’accès à distance dépend des rôles/fonctionnalités de serveur suivants :<br /><br />-Internet Information Services (IIS) serveur Web - cette fonctionnalité est nécessaire pour configurer le serveur emplacement réseau sur le serveur d’accès à distance et la sonde web par défaut.<br />-Base de données interne Windows. utilisée pour la gestion locale des comptes sur le serveur d'accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Il est installé par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface d’utilisateur de console de gestion à distance et les applets de commande Windows PowerShell.<br />-Il peut éventuellement être installé sur un serveur n'exécutant pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Accès distant GUI<br />-Module d’accès distant pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion de stratégie de groupe<br />-Kit d’Administration RAS Gestionnaire de connexions (CMAK)<br />-Windows PowerShell 3.0<br />-Infrastructure et des outils de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Configuration requise du serveur :  
  
    -   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
    -   Sur le serveur, au moins une carte réseau doit être installée, activée et connectée au réseau interne. Lorsque deux cartes sont utilisées, une carte doit être connectée au réseau interne de l’entreprise et l’autre carte doit être connectée au réseau externe (Internet ou un réseau privé).  
  
    -   Au moins un contrôleur de domaine. Le serveur d’accès à distance et les clients DirectAccess doivent être membres d’un domaine.  
  
-   Configuration requise du client :  
  
    -   Un ordinateur client doit exécuter Windows 10, Windows 8.1 ou Windows 8.  
  
        > [!IMPORTANT]  
        > Si certains ou tous vos ordinateurs clients exécutent Windows 7, vous devez utiliser l’Assistant Installation avancée. L’Assistant Mise en route le programme d’installation décrites dans ce document ne prend pas en charge les ordinateurs clients qui exécutent Windows 7. Consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/../../../remote-access/directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) pour obtenir des instructions sur l’utilisation des clients Windows 7 avec DirectAccess.  
  
        > [!NOTE]  
        > Seuls les systèmes d'exploitation suivants peuvent être utilisés en tant que clients DirectAccess : Windows 10 entreprise, Windows 8.1 Enterprise, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 8 Enterprise, Windows Server 2008 R2, Windows 7 entreprise et Édition intégrale de Windows 7.  
  
-   Configuration requise en termes d’infrastructure et de serveurs d’administration :  
  
    -   Lorsque le réseau VPN est activé, vous devez déployer un serveur DHCP pour allouer automatiquement des adresses IP aux clients VPN si un pool d’adresses IP statiques n’est pas configuré.  
  
-   Un serveur DNS exécutant Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 SP2 ou Windows Server 2008 R2 est requis.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  
  
-   Configuration requise du serveur :  
  
    -   Le serveur d’accès à distance doit être membre d’un domaine. Le serveur peut être déployé en périphérie du réseau interne ou derrière un pare-feu de périmètre ou un autre périphérique.  
  
    -   Si le serveur d’accès à distance se trouve derrière un pare-feu de périmètre ou un périphérique NAT, ce périphérique doit être configuré de manière à autoriser le trafic en direction et en provenance du serveur d’accès à distance.  
  
    -   La personne qui déploie l’accès à distance sur le serveur doit disposer des autorisations d’administrateur local sur le serveur, ainsi que des autorisations d’utilisateur de domaine. L’administrateur doit également disposer des autorisations pour les objets de stratégie de groupe utilisés dans le déploiement de DirectAccess. L’utilisation des fonctionnalités qui limitent le déploiement de DirectAccess sur les ordinateurs portables uniquement nécessite de disposer des autorisations permettant de créer un filtre WMI sur le contrôleur de domaine.  
  
-   Configuration requise pour les clients d’accès à distance :  
  
    -   Les clients DirectAccess doivent appartenir au domaine. Les domaines contenant des clients peuvent appartenir à la même forêt que celle du serveur d’accès à distance ou ils peuvent avoir une relation d’approbation bidirectionnelle avec la forêt du serveur d’accès à distance.  
  
    -   Un groupe de sécurité Active Directory est requis afin de contenir les ordinateurs qui seront configurés en tant que clients DirectAccess. Si aucun groupe de sécurité n’est spécifié lors de la configuration des paramètres des clients DirectAccess, le client GPO est appliqué par défaut sur tous les ordinateurs portables inclus dans le groupe de sécurité Ordinateurs du domaine. Seuls les systèmes d'exploitation suivants peuvent être utilisés en tant que clients DirectAccess :  Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 Enterprise, Windows 7 entreprise et Édition intégrale de Windows 7.  
  
## <a name="BKMK_LINKS"></a>Voir aussi  
Le tableau suivant fournit des liens vers des ressources supplémentaires.  
  
|Type de contenu|Références|  
|--------|-------|  
|**Accès à distance sur TechNet**|[TechCenter d’accès à distance](https://technet.microsoft.com/network/bb545655.aspx)|  
|**Outils et paramètres**|[Applets de commande PowerShell pour l’accès à distance](https://technet.microsoft.com/library/hh918399.aspx)|  
|**Ressources de la communauté**|[Entrées DirectAccess Wiki](https://go.microsoft.com/fwlink/?LinkId=236871)|  
|**Technologies connexes**|[Fonctionnement d’IPv6](https://technet.microsoft.com/library/cc781672(v=WS.10).aspx)|  
  


