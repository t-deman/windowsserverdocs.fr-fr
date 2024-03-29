---
title: Utiliser des bureaux de session personnels avec les services Bureau à distance
description: Apprenez à partager des appareils de bureau personnalisés affectés par le biais des services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 09/16/2016
manager: dongill
ms.openlocfilehash: 723e68bad79e7723aaa0690c312e20ccf47c47b0
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63743506"
---
# <a name="use-personal-session-desktops-with-remote-desktop-services"></a>Utiliser des bureaux de session personnels avec les services Bureau à distance

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez déployer des appareils de bureau personnels basés sur serveur dans un environnement de cloud computing à l'aide de bureaux de session personnels.  (Un environnement de cloud computing sépare les serveurs d'infrastructure Hyper-V des machines virtuelles invitées, comme Microsoft Azure Cloud ou la plateforme Microsoft Cloud.) La fonctionnalité de bureau de session personnel étend le scénario de déploiement de bureau basé sur une session aux services Bureau à distance pour créer un nouveau type de collection de sessions dans lequel chaque utilisateur est affecté à son propre hôte de session personnel avec des droits d'administration. 

Utilisez les informations suivantes pour créer et gérer une collection de bureaux de session personnels.

## <a name="create-a-personal-session-desktop-collection"></a>Créer une collection de bureaux de session personnels

Utilisez l'applet de commande New-RDSessionCollection pour créer une collection de bureaux de session personnels. Les trois paramètres suivants fournissent les informations de configuration requises pour les bureaux de session personnels :

- **-PersonalUnmanaged** : spécifie le type de collection de sessions qui vous permet d'affecter des utilisateurs à un serveur hôte de session personnelle. Si vous ne spécifiez pas ce paramètre, la collection est créée comme une collection traditionnelle d'hôtes de session Bureau à distance, où les utilisateurs sont affectés à l'hôte de session disponible suivant lorsqu'ils se connectent.
- **-GrantAdministrativePrivilege** : si vous utilisez **-PersonalUnmanaged**, spécifie que l'utilisateur affecté à l'hôte de session doit disposer de privilèges d'administration. Si vous n'utilisez pas ce paramètre, les utilisateurs bénéficient seulement de privilèges d'utilisateur standard.
- **-AutoAssignUser** : si vous utilisez **-PersonalUnmanaged**, spécifie que les nouveaux utilisateurs qui se connectent via le service Broker pour les connexions Bureau à distance sont automatiquement affectés à un hôte de session non attribué. En l'absence d'hôte de session non attribué dans la collection, l'utilisateur reçoit un message d'erreur. Si vous n'utilisez pas ce paramètre, vous devez [affecter manuellement les utilisateurs à un hôte de session](#manually-assign-a-user-to-a-personal-session-host) avant qu'ils ne se connectent.

## <a name="manually-assign-a-user-to-a-personal-session-host"></a>Affectation manuelle d'un utilisateur à un hôte de session personnel
Utilisez l'applet de commande **Set-RDPersonalSessionDesktopAssignment** pour affecter manuellement un utilisateur à un serveur hôte de session personnel dans la collection. L'applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\> 

-User \<chaîne\>

-Name \<chaîne\>

- **– CollectionName** : spécifie le nom de la collection de bureaux de session personnels. Ce paramètre est obligatoire.
- **– ConnectionBroker** : spécifie le serveur du service Broker pour les connexions Bureau à distance de votre déploiement Bureau à distance. Si vous ne fournissez aucune valeur, l'applet de commande utilise le nom de domaine complet (FQDN) de l'ordinateur local.
- **–User** : spécifie le compte d'utilisateur à associer au bureau de session personnel, au format DOMAINE\Utilisateur. Ce paramètre est obligatoire.
- **–Name** : spécifie le nom du serveur hôte de session. Ce paramètre est obligatoire. L'hôte de session identifié ici doit être membre de la collection spécifiée par le paramètre **-CollectionName**.

L'applet de commande **Import-RDPersonalSessionDesktopAssignment** importe les associations entre les comptes d'utilisateur et les bureaux de session personnels à partir d'un fichier texte. L'applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne>

**–Path** spécifie le chemin d'accès et le nom d'un fichier à importer.
 
## <a name="removing-a-user-assignment-from-a-personal-session-host"></a>Suppression d'une affectation d'utilisateur à partir d'un hôte de session personnel
Utilisez l'applet de commande **Remove-RDPersonalSessionDesktopAssignment** pour supprimer l'association entre un bureau de session personnel et un utilisateur. L'applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Force

-Name \<chaîne\>

-User \<chaîne\>

**–Force** : force l'exécution de la commande sans demander la confirmation de l'utilisateur.

## <a name="query-user-assignments"></a>Interroger les affectations d'utilisateur
Utilisez l'applet de commande **Get-RDPersonalSessionDesktopAssignment** pour obtenir la liste des bureaux de session personnels et des comptes d'utilisateurs associés. L'applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-User \<chaîne\>

-Name \<chaîne\>

Vous pouvez exécuter l'applet de commande pour lancer une interrogation par nom de collection, par nom d'utilisateur ou par nom de bureau de session. Si vous spécifiez uniquement le paramètre **–CollectionName**, l'applet de commande renvoie la liste des hôtes de session et des utilisateurs associés. Si vous spécifiez également le paramètre **–User**, l'hôte de session associé à cet utilisateur est renvoyé. Si vous fournissez le paramètre **–Name**, l'utilisateur associé à cet hôte de session est renvoyé. 


L'applet de commande **Export-RDPersonalPersonalDesktopAssignment** exporte les associations actuelles entre les utilisateurs et les bureaux virtuels personnels dans un fichier texte. L'applet de commande prend en charge les paramètres suivants :

-CollectionName \<chaîne\>

-ConnectionBroker \<chaîne\>

-Path \<chaîne\>


Les nouvelles applets de commande prennent toutes en charge les paramètres courants : -Verbose, -Debug, -ErrorAction, -ErrorVariable, -OutBuffer et -OutVariable. Pour plus d'informations, consultez [about_CommonParameters](https://go.microsoft.com/fwlink/p/?LinkID=113216).

## <a name="hardware-accelerated-graphics"></a>Graphiques à accélération matérielle
Windows Server 2016 étend la technologie de la carte graphique RemoteFX vGPU 3D pour prendre en charge OpenGL, et prend en charge les machines virtuelles mono-utilisateur Windows Server 2016 invitées. Vous pouvez combiner des bureaux de session personnels avec les nouvelles fonctionnalités vGPU pour prendre en charge les applications hébergées nécessitant un affichage graphique accéléré. Sinon, vous pouvez combiner des bureaux de session personnels avec la nouvelle fonctionnalité DDA (Discrete Device Assignment) pour prendre en charge les applications hébergées nécessitant un affichage graphique accéléré.
