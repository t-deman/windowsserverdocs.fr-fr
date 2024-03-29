---
ms.assetid: f15c02d7-1cbd-4eba-a571-0ea34ab93ef4
title: Exécution de la déduplication des données
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 09/15/2016
ms.openlocfilehash: 2e2e4975c4ab9ebb7ec68834f380255292426393
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447226"
---
# <a name="running-data-deduplication"></a>Exécution de la déduplication des données

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

## <a id="running-dedup-jobs-manually"></a>Exécution manuelle de travaux de déduplication des données

Chaque travail planifié de déduplication des données peut être exécuté manuellement à l’aide des applets de commande PowerShell suivantes :
* [`Start-DedupJob`](https://technet.microsoft.com/library/hh848442.aspx): Démarre un nouveau travail de la déduplication des données
* [`Stop-DedupJob`](https://technet.microsoft.com/library/hh848439.aspx): Arrête un travail de la déduplication des données déjà en cours d’exécution (ou le supprime de la file d’attente)
* [`Get-DedupJob`](https://technet.microsoft.com/library/hh848452.aspx): Affiche tous les travaux de déduplication des données actives et en file d’attente

Tous les [paramètres qui sont disponibles lorsque vous planifiez un travail de déduplication des données](advanced-settings.md#modifying-job-schedules-available-settings) sont également disponibles lorsque vous démarrez un travail manuellement, à l’exception des paramètres spécifiques de planification. Par exemple, pour démarrer un travail d’[optimisation](understand.md#job-info-optimization) manuellement avec une priorité élevée et une utilisation maximale du processeur et de la mémoire, exécutez la commande PowerShell suivante avec des privilèges d’administrateur :

```PowerShell
Start-DedupJob -Type Optimization -Volume <Your-Volume-Here> -Memory 100 -Cores 100 -Priority High
```

## <a id="monitoring-dedup"></a>Surveillance de la déduplication des données

### <a id="monitoring-dedup-job-successes"></a>Réussite des travaux

Étant donné que la déduplication des données utilise un modèle de post-traitement, il est important que les [travaux de déduplication des données](understand.md#job-info) réussissent. Un moyen simple de vérifier l’état du dernier travail consiste à utiliser l’applet de commande PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx). Vérifiez régulièrement les champs suivants :

* Pour le [travail d’optimisation](understand.md#job-info-optimization), examinez `LastOptimizationResult` (0 = succès), `LastOptimizationResultMessage` et `LastOptimizationTime` (cette valeur doit être récente).
* Pour le [travail de nettoyage de la mémoire](understand.md#job-info-gc), examinez `LastGarbageCollectionResult` (0 = succès), `LastGarbageCollectionResultMessage` et `LastGarbageCollectionTime` (cette valeur doit être récente).
* Pour le [travail de nettoyage d’intégrité](understand.md#job-info-scrubbing), examinez `LastScrubbingResult` (0 = succès), `LastScrubbingResultMessage` et `LastScrubbingTime` (cette valeur doit être récente).

> [!Note]  
> Vous trouverez plus d’informations sur les échecs et réussites des travaux dans l’Observateur d’événements Windows sous `\Applications and Services Logs\Windows\Deduplication\Operational`.

### <a id="monitoring-dedup-optimization-rates"></a>Taux d’optimisation

Un taux d’optimisation qui tend vers le bas indique un échec du [travail d’optimisation](understand.md#job-info-optimization), qui peut signifier que le travail d’optimisation ne parvient pas à suivre le rythme des modifications (ou l’activité). Vous pouvez vérifier le taux d’optimisation à l’aide de l’applet de commande PowerShell [`Get-DedupStatus`](https://technet.microsoft.com/library/hh848437.aspx).

> [!Important]
> `Get-DedupStatus` comporte deux champs qui sont pertinents pour le taux d’optimisation : `OptimizedFilesSavingsRate` et `SavingsRate`. Ce sont les deux valeurs importantes à suivre, mais chacune a une signification unique.
> - `OptimizedFilesSavingsRate` s’applique uniquement aux fichiers qui sont « stratégie » pour l’optimisation (`space used by optimized files after optimization / logical size of optimized files`).
> - `SavingsRate` s’applique à la totalité du volume (`space used by optimized files after optimization / total logical size of the optimization`).

## <a id="disabling-dedup"></a>La désactivation de la déduplication des données
Pour désactiver la déduplication des données, exécutez le [travail d’annulation de l’optimisation](understand.md#job-info-unoptimization). Pour annuler l’optimisation du volume, exécutez la commande suivante :

```PowerShell
Start-DedupJob -Type Unoptimization -Volume <Desired-Volume>
```

> [!Important]  
> Le travail d’annulation de l’optimisation échoue si le volume ne dispose pas de suffisamment d’espace pour contenir les données non optimisées.

## <a id="faq"></a>Forum aux Questions
**Existe-t-il un System Center Operations Manager Management Pack disponibles pour surveiller la déduplication des données ?**  
Oui. La déduplication des données peut être surveillée par le pack d’administration System Center pour serveur de fichiers. Pour plus d’informations, consultez le document [Guide for System Center Management Pack for File Server 2012 R2](https://download.microsoft.com/download/6/F/7/6F7A33B9-9383-48ED-9252-23C2C8AD1BDA/MPGuide_FileServer2012R2.doc) (Guide pour le pack d’administration System Center pour serveur de fichiers 2012 R2).
