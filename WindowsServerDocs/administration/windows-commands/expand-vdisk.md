---
title: Développez vdisk
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8019ce62d6cf38c7430a789f68749444ac91ad48
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439442"
---
# <a name="expand-vdisk"></a>Développez vdisk

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

développe un disque dur virtuel (VHD) à la taille que vous spécifiez.
> [!NOTE]
> Cette commande est uniquement applicable à Windows 7 et Windows Server 2008 R2.
> ## <a name="syntax"></a>Syntaxe
> ```
> expand vdisk maximum=<n>
> ```
> ## <a name="parameters"></a>Paramètres
> 
> |  Paramètre  |                      Description                      |
> |-------------|-------------------------------------------------------|
> | maximum=<n> | Spécifie la nouvelle taille du disque dur virtuel en mégaoctets (Mo). |
> 
> ## <a name="remarks"></a>Notes
> - Un disque dur virtuel doit être sélectionné et détaché pour cette opération réussisse. Utilisez le **sélectionnez vdisk** commande pour sélectionner un volume et déplacer le focus vers elle.
>   ## <a name="BKMK_Examples"></a>Exemples
>   Pour développer le disque dur virtuel sélectionné de 20 Go, tapez :
>   ```
>   expand vdisk maximum=20000
>   ```
>   ## <a name="additional-references"></a>Références supplémentaires
> - [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
> - [attach vdisk](attach-vdisk.md)
> - [compact vdisk](compact-vdisk.md)

-   [Détacher vdisk](detach-vdisk.md)
-   [detail vdisk](detail-vdisk.md)
-   [Fusion vdisk](merge-vdisk.md)
-   [select vdisk](select-vdisk.md)
-   [list_1](list_1.md)
