---
ms.assetid: 38c9bcd3-c6f8-4153-8e42-5fd31568c65a
title: 'Liste de vérification : configuration d’un serveur Proxy de fédération'
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: c8cd5cbe2ce0c7985a56f8444edfa7c71ee17c2d
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192348"
---
# <a name="checklist-setting-up-a-federation-server-proxy"></a>Liste de vérification : configuration d'un serveur de fédération proxy

Cette liste comprend les tâches de déploiement pour la préparation d’un serveur exécutant Windows Server® 2012 pour le rôle du serveur proxy de fédération dans Active Directory Federation Services \(AD FS\).  
  
> [!NOTE]  
> Effectuez dans l’ordre les tâches répertoriées dans cette liste de vérification. Quand un lien de référence vous mène à une procédure, revenez à cette rubrique après avoir terminé les étapes décrites dans cette procédure afin que vous pouvez poursuivre les tâches restantes dans cette liste de vérification.  
  
![configuration d’un serveur proxy fédéré](media/2b05dce3-938f-4168-9b8f-1f4398cbdb9b.gif)**liste de vérification : Configuration d’un serveur proxy de fédération**  
  
||Tâche|Référence|  
|-|--------|-------------|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Avant de commencer le déploiement de vos serveurs proxy de fédération AD FS, passez en revue les types de topologie de déploiement AD FS et leur placement de serveur associé et des recommandations de mise en réseau.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[déterminer votre topologie de déploiement AD FS](https://technet.microsoft.com/library/gg982491.aspx)<br /><br />![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification Federation Server Proxy Placement](https://technet.microsoft.com/library/dd807130.aspx)<br /><br />![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[où placer un serveur Proxy de fédération](https://technet.microsoft.com/library/dd807048.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Passez en revue les conseils sur la planification de capacité AD FS pour déterminer le nombre approprié de serveurs proxy de fédération que vous devez utiliser dans votre environnement de production.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[planification de la capacité de Proxy de serveur de fédération](https://technet.microsoft.com/library/gg749898.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Déterminer si un serveur proxy de fédération unique ou une batterie de serveurs du proxy du serveur de fédération est préférable pour votre déploiement. **Remarque :** Serveurs de fédération également effectuer des responsabilités de proxy de serveur de fédération.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quand créer un serveur Proxy de fédération](https://technet.microsoft.com/library/dd807032.aspx)<br /><br />![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[quand créer une batterie de serveurs Proxy de serveur de fédération](https://technet.microsoft.com/library/dd807082.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Déterminer si ce nouveau serveur proxy de fédération sera créé dans le réseau de périmètre de l’organisation partenaire de compte ou de l’organisation partenaire ressource.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[revue du rôle de serveur Proxy de fédération du partenaire de compte](https://technet.microsoft.com/library/dd807109.aspx)<br /><br />![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[revue du rôle de serveur Proxy de fédération du partenaire de ressource](https://technet.microsoft.com/library/dd807052.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Avant d’installer AD FS sur un ordinateur qui deviendra un serveur proxy de fédération, découvrez l’importance de l’obtention d’un serveur de certificat d’authentification : fédération batteries de serveurs proxy, ajout ou le partage de certificats sur tous les serveurs dans une batterie de serveurs.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[configuration requise des certificats pour les serveurs proxy de fédération](https://technet.microsoft.com/library/dd807054.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Passez en revue les informations dans le Guide de conception AD FS sur comment mettre à jour de système de nom de domaine \(DNS\) dans le réseau de périmètre, que la résolution de nom réussi pour les serveurs de fédération et les serveurs proxy de fédération peut donc se produire.|![configuration d’un serveur proxy fédéré](media/faa393df-4856-4431-9eda-4f4e5be72a90.gif)[Name Resolution Requirements for Federation Server Proxies](https://technet.microsoft.com/library/dd807055.aspx)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Déterminer si le serveur proxy de fédération doit être joint à un domaine. Bien que les serveurs proxy de fédération n’ont pas d’être joint à un domaine, ils sont plus faciles à gérer avec l’administration à distance et les fonctionnalités de stratégie de groupe lorsqu’ils sont associés à un domaine.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[joindre un ordinateur à un domaine](Join-a-Computer-to-a-Domain.md)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Selon la configuration de l’infrastructure DNS dans votre réseau de périmètre, effectuez l’une des procédures dans les rubriques sur la droite avant de déployer un serveur proxy de fédération dans votre organisation. **Remarque :** N’effectuez pas les deux procédures. Lecture [Name Resolution Requirements for Federation Server Proxies](https://technet.microsoft.com/library/dd807055.aspx) pour déterminer quelle procédure meilleures adapté à la configuration requise de votre organisation.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurer la résolution de noms pour un serveur Proxy de fédération dans une Zone DNS qui sert uniquement le réseau de périmètre](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Only-the-Perimeter-Network.md)<br /><br />![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurer la résolution de noms pour un serveur Proxy de fédération dans un DNS Zone que sert à la fois le réseau de périmètre et les Clients Internet](Configure-Name-Resolution-for-a-Federation-Server-Proxy-in-a-DNS-Zone-That-Serves-Both-the-Perimeter-Network-and-Internet-Clients.md)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Après avoir obtenu un serveur de certificat d’authentification, vous devez l’installer dans Internet Information Services \(IIS\) sur le site Web par défaut du serveur proxy de fédération.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[importer un certificat d’authentification serveur pour le Site Web par défaut](Import-a-Server-Authentication-Certificate-to-the-Default-Web-Site.md)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|\(Facultatif\) comme alternative à l’obtention d’un serveur de certificat d’authentification auprès d’une autorité de certification \(autorité de certification\), vous pouvez utiliser IIS pour obtenir un exemple de certificat pour votre serveur proxy de fédération.<br /><br />Étant donné que IIS génère un libre-service\-certificat auto-signé qui n’a pas été proviennent d’une source approuvée, utilisez-le pour créer un\-signé certificat uniquement dans les scénarios suivants :<br /><br />-Lorsque vous devez créer un Secure Sockets Layer \(SSL\) canal entre votre serveur et un groupe limité et connu d’utilisateurs<br />-Lorsque vous avez besoin de résoudre troisième\-des problèmes de certificat de tiers **attention :** Il n’est pas une meilleure pratique de sécurité pour déployer un serveur proxy de fédération dans un environnement de production à l’aide d’un libre-service\-signé, certificat d’authentification serveur.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[IIS : Créer un\-signé le certificat de serveur](https://go.microsoft.com/fwlink/?LinkID=108271)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Installer le service de rôle Proxy du Service de fédération sur l’ordinateur qui deviendra le serveur proxy de fédération.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[installer le service de rôle Proxy du Service de fédération](Install-the-Federation-Service-Proxy-Role-Service.md)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|Configurer le logiciel AD FS sur l’ordinateur d’agir dans le rôle de proxy de serveur de fédération à l’aide de l’Assistant de Configuration AD FSFederation serveur Proxy.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[configurer un ordinateur pour le rôle de Proxy de serveur de fédération](Configure-a-Computer-for-the-Federation-Server-Proxy-Role.md)|  
|![configuration d’un serveur proxy fédéré](media/icon_checkboxo.gif)|À l’aide de l’Observateur d’événements, vérifiez que le service du serveur proxy de fédération a démarré.|![configuration d’un serveur proxy fédéré](media/15dd35b6-6cc6-421f-93f8-7109920e7144.gif)[Vérifiez qu’une fédération de serveur Proxy est opérationnel](Verify-That-a-Federation-Server-Proxy-Is-Operational.md)|  