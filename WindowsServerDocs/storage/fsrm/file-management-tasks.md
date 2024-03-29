---
title: Tâches de gestion de fichiers
description: Cet article décrit le processus d’automatisation des tâches de gestion de fichiers
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d8d798611a00e29337a5d45979947a51f03bcdee
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475891"
---
# <a name="file-management-tasks"></a>Tâches de gestion de fichiers

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Les tâches de gestion de fichiers automatisent le processus de recherche des sous-ensembles de fichiers sur un serveur et d’application de commandes simples. Ces tâches peuvent être planifiées pour se produire régulièrement, afin de réduire les coûts répétitifs. Les fichiers qui seront traités par une tâche de gestion de fichiers peuvent être définis via l'une des propriétés suivantes :

-   Location
-   Propriétés de classification
-   Date de création
-   Date de modification
-   Date du dernier accès

Les tâches de gestion de fichiers peuvent également être configurées pour avertir les propriétaires de fichiers des stratégies imminentes qui vont être appliquées à leurs fichiers.

> [!Note]
> Chaque tâche de gestion des fichiers est exécutée selon une planification indépendante.

<br />
Cette section comprend les rubriques suivantes :

-   [Créer une tâche d'expiration de fichiers](create-file-expiration-task.md)
-   [Créer une tâche de gestion de fichiers personnalisée](create-custom-file-management-task.md)

> [!Note]
> Pour définir des notifications par courrier électronique et certaines fonctionnalités de création de rapports, vous devez d’abord configurer les options générales du Gestionnaire de ressources du serveur de fichiers.

## <a name="see-also"></a>Voir aussi

-   [Définition des options du Gestionnaire de ressources du serveur de fichiers](setting-file-server-resource-manager-options.md)


