---
title: Set-ImageGroup sous-commande
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4d86946a-e261-4d41-8b0c-1ab0ba2e3430
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0c7ba47148ba6f8295ab720dd0118759ac9346c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59822980"
---
# <a name="subcommand-set-imagegroup"></a>Sous-commande : set-ImageGroup

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifie les attributs d’un groupe d’images.
## <a name="syntax"></a>Syntaxe
```
wdsutil [Options] /Set-ImageGroumediaGroup:<Image group name> [/Server:<Server name>] [/Name:<New image group name>] [/Security:<SDDL>]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
mediaGroup :<Image group name>|Spécifie le nom du groupe d'images.|
|[/Server:<Server name>]|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom FQDN. Si non spécifié, le serveur local doit être utilisé.|
|[/ Nom :<New image group name>]|Spécifie le nouveau nom du groupe d’images.|
|[/ Sécurité :<SDDL>]|Spécifie le nouveau descripteur de sécurité de groupe d’images, dans le format de langage (SDDL) de définition de descripteur de sécurité.|
## <a name="BKMK_examples"></a>Exemples
Pour définir le nom d’un groupe d’images, tapez :
```
wdsutil /Set-ImageGroumediaGroup:ImageGroup1 /Name:"New Image Group Name"
```
Pour spécifier différents paramètres pour un groupe d’images, tapez :
```
wdsutil /verbose /Set-ImageGroumediaGroup:ImageGroup1 /Server:MyWDSServer /Name:"New Image Group Name" 
/Security:"O:BAG:S-1-5-21-2176941838-3499754553-4071289181-513 D:AI(A;ID;FA;;;SY)(A;OICIIOID;GA;;;SY)(A;ID;FA;;;BA)(A;OICIIOID;GA;;;BA) (A;ID;0x1200a9;;;AU)(A;OICIIOID;GXGR;;;AU)"
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageGroup](using-the-add-imagegroup-command.md)
[à l’aide de la commande get-AllImageGroups](using-the-get-allimagegroups-command.md) 
 [ À l’aide de la commande get-ImageGroup](using-the-get-imagegroup-command.md)
[à l’aide de la commande remove-ImageGroup](using-the-remove-imagegroup-command.md)
