---
title: Paramètres du Service d’intégrité
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 569cf7ba30fd3f993394efd3735a56d116c067e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858330"
---
# <a name="health-service-settings"></a>Paramètres du Service d’intégrité
> S’applique à Windows Server 2016

Le Service de contrôle d’intégrité est une nouvelle fonctionnalité de Windows Server 2016 qui améliore l’analyse quotidienne et une expérience opérationnelle pour les clusters exécutant des espaces de stockage Direct.

La plupart des paramètres qui régissent le comportement du Service d’intégrité sont exposées en tant que paramètres. Vous pouvez modifier ces pour ajuster l’intensité des erreurs ou des actions, activer certains comportements activé/désactivé et bien plus encore.

Utilisez l’applet de commande PowerShell suivante pour définir ou modifier les paramètres.

### <a name="usage"></a>Utilisation

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name <SettingName> -Value <Value>  
```

#### <a name="example"></a>Exemple

```PowerShell
Get-StorageSubSystem Cluster* | Set-StorageHealthSetting -Name "System.Storage.Volume.CapacityThreshold.Warning" -Value 70
```

### <a name="common-settings"></a>Paramètres communs

Certains paramètres couramment modifiées sont mentionnées ci-dessous, ainsi que leurs valeurs par défaut.

#### <a name="volume-capacity-threshold"></a>Seuil de capacité de volume

```
"System.Storage.Volume.CapacityThreshold.Enabled"  = True
"System.Storage.Volume.CapacityThreshold.Warning"  = 80
"System.Storage.Volume.CapacityThreshold.Critical" = 90
```

#### <a name="pool-reserve-capacity-threshold"></a>Seuil de la capacité de réserve pool

```
"System.Storage.StoragePool.CheckPoolReserveCapacity.Enabled" = True
```

#### <a name="physical-disk-lifecycle"></a>Cycle de vie du disque physique

```
"System.Storage.PhysicalDisk.AutoPool.Enabled"                             = True
"System.Storage.PhysicalDisk.AutoRetire.OnLostCommunication.Enabled"       = True
"System.Storage.PhysicalDisk.AutoRetire.OnUnresponsive.Enabled"            = True
"System.Storage.PhysicalDisk.AutoRetire.DelayMs"                           = 900000 (i.e. 15 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountResetIntervalSeconds" = 360 (i.e. 60 minutes)
"System.Storage.PhysicalDisk.Unresponsive.Reset.CountAllowed"              = 3
```

#### <a name="supported-components-document"></a>Document de composants pris en charge

Consultez la section précédente.

#### <a name="firmware-rollout"></a>Déploiement de microprogramme

```
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.SingleDrive.Enabled"       = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.Enabled"           = True
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelaySeconds"  = 604800 (i.e. 7 days)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.ShortDelaySeconds" = 86400 (i.e. 1 day)
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.LongDelayCount"    = 1
"System.Storage.PhysicalDisk.AutoFirmwareUpdate.RollOut.FailureTolerance"  = 3
```

#### <a name="platform--quiescence"></a>Plateforme / arrêt inactive

```
"Platform.Quiescence.MinDelaySeconds" = 120 (i.e. 2 minutes)
"Platform.Quiescence.MaxDelaySeconds" = 420 (i.e. 7 minutes)
```

#### <a name="metrics"></a>Mesures

```
"System.Reports.ReportingPeriodSeconds" = 1
```

#### <a name="debugging"></a>Débogage

```
"System.LogLevel" = 4
```

## <a name="see-also"></a>Voir aussi

- [Service d’intégrité de Windows Server 2016](health-service-overview.md)
- [Espaces de stockage Direct dans Windows Server 2016](../storage/storage-spaces/storage-spaces-direct-overview.md)
