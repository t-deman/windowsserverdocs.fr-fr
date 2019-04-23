---
title: À l’aide de la commande add-ImageDriverPackages
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9dc78909-a4d1-42a2-af8f-21ebcbfe8302
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55b26acf9c4006db3d64e27be8a10e158876ddc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817360"
---
# <a name="using-the-add-imagedriverpackages-command"></a>À l’aide de la commande add-ImageDriverPackages

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ajoute des packages de pilotes à partir du magasin de pilotes à une image de démarrage. La version de l’image doit être Windows 7 ou Windows Server 2008 R2 ou version ultérieure.
Pour obtenir des exemples de la façon dont vous pouvez utiliser cette commande, consultez [exemples](#BKMK_examples).
## <a name="syntax"></a>Syntaxe
```
wdsutil /add-ImageDriverPackages [/Server:<Server name>media:<Image namemediatype:Boot /Architecture:{x86 | ia64 | x64} 
[/Filename:<File name>] /Filtertype:<Filter type> /Operator:{Equal | NotEqual | GreaterOrEqual | LessOrEqual | Contains} /Value:<Value> [/Value:<Value> ...]
```
## <a name="parameters"></a>Paramètres
|Paramètre|Description|
|-------|--------|
|/ Server :<Server name>|Spécifie le nom du serveur. Cela peut être le nom NetBIOS ou le nom de domaine complet. Si aucun nom de serveur n’est spécifié, le serveur local est utilisé.|
média :<Image name>|Spécifie le nom de l’image à ajouter au pilote.|
mediatype:Boot|Spécifie le type d’image à ajouter au pilote. Packages de pilotes peuvent être ajoutés uniquement aux images de démarrage.|
|/ Architecture : {x86 &#124; ia64 &#124; x64}|Spécifie l’architecture de l’image de démarrage. Comme il est possible d’avoir le même nom d’image pour les images de démarrage dans différentes architectures, vous devez spécifier l’architecture pour garantir que l’image correcte est utilisée.|
|/ Filename :<File name>|Spécifie le nom de fichier. Si l’image ne peut pas être identifié de manière unique par son nom, le nom de fichier doit être spécifié.|
|/ Filtertype :<Filter type>|Spécifie l’attribut du package de pilotes à rechercher. Vous pouvez spécifier plusieurs attributs dans une seule commande. Vous devez également spécifier **/Operator** et **/valeur** avec cette option.<br /><br /><Filter type> Peut prendre l’une des opérations suivantes :<br /><br />**PackageId**<br /><br />**PackageName**<br /><br />**PackageEnabled**<br /><br />**Packagedateadded**<br /><br />**PackageInfFilename**<br /><br />**PackageClass**<br /><br />**PackageProvider**<br /><br />**PackageArchitecture**<br /><br />**PackageLocale**<br /><br />**PackageSigned**<br /><br />**PackagedatePublished**<br /><br />**Packageversion**<br /><br />**Driverdescription**<br /><br />**DriverManufacturer**<br /><br />**DriverHardwareId**<br /><br />**DrivercompatibleId**<br /><br />**DriverExcludeId**<br /><br />**DriverGroupId**<br /><br />**DriverGroupName**|
|/Operator:{Equal &#124; NotEqual &#124; GreaterOrEqual &#124; LessOrEqual &#124; Contains}|La relation entre l’attribut et les valeurs. Vous pouvez uniquement spécifier **Contains** des attributs de chaîne. Vous pouvez uniquement spécifier **GreaterOrEqual** et **LessOrEqual** avec les attributs de date et de version.|
|/ Valeur :<Value>|Spécifie la valeur à rechercher par rapport à spécifié <attribute>. Vous pouvez spécifier plusieurs valeurs pour un seul **/Filtertype**. La liste ci-dessous décrit les attributs que vous pouvez spécifier pour chaque filtre. Pour plus d’informations sur ces attributs, consultez [les attributs de pilote et le Package](https://go.microsoft.com/fwlink/?LinkId=166895) (https://go.microsoft.com/fwlink/?LinkId=166895).<br /><br />-PackageId - spécifiez un GUID valide. Par exemple : {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageName spécifier toute valeur de chaîne.<br />-Spécifiez PackageEnabled - **Oui** ou **non**.<br />-Packagedateadded - spécifier la date au format suivant : AAAA/MM/JJ<br />-PackageInfFilename spécifier toute valeur de chaîne.<br />-PackageClass - spécifiez un nom de classe valide ou le GUID de la classe. Exemple : **Lecteur de disque**, **Net**, ou {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-PackageProvider spécifier toute valeur de chaîne.<br />-Spécifiez PackageArchitecture - **x86**, **x64**, ou **ia64**.<b /> -PackageLocale - spécifier un identificateur de langue valide. Par exemple : **en-US** ou **es-ES**.<br />-Spécifiez PackageSigned - **Oui** ou **non**.<br />-PackagedatePublished - spécifier la date au format suivant : AAAA/MM/JJ<br />-Packageversion - spécifier la version au format suivant : a.b.x.y. Exemple : 6.1.0.0<br />-Driverdescription spécifier toute valeur de chaîne.<br />-DriverManufacturer spécifier toute valeur de chaîne.<br />-DriverHardwareId - spécifier n’importe quelle valeur de chaîne.<br />-DrivercompatibleId - spécifier n’importe quelle valeur de chaîne.<br />-DriverExcludeId - spécifier n’importe quelle valeur de chaîne.<br />-DriverGroupId - spécifiez un GUID valide. Par exemple : {4d36e972-e325-11ce-bfc1-08002be10318}.<br />-DriverGroupName spécifier toute valeur de chaîne.|
## <a name="BKMK_examples"></a>Exemples
Pour ajouter des packages de pilotes à une image de démarrage, tapez une des opérations suivantes :
```
wdsutil /add-ImageDriverPackagemedia:"WinPE Boot Imagemediatype:Boot /Architecture:x86 /Filtertype:DriverGroupName /Operator:Equal /Value:x86Bus /Filtertype:PackageProvider /Operator:Contains /Value:Provider1 /Filtertype:Packageversion /Operator:GreaterOrEqual /Value:6.1.0.0
```
```
wdsutil /verbose /add-ImageDriverPackagemedia: "WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Filtertype:DriverManufacturer /Operator:NotEqual /Value:Name1 /Value:Name2 /Filtertype:Packagedateadded /Operator:LessOrEqual /Value:2008/01/01
```
```
wdsutil /verbose /add-ImageDriverPackagemedia:"WinPE Boot Image" /Server:MyWDSServemediatype:Boot /Architecture:x64 /Filtertype:PackageClass /Operator:Equal /Value:Net /Value:System /Value:DiskDrive /Value:HDC /Value:SCSIAdapter
```
#### <a name="additional-references"></a>Références supplémentaires
[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)
[à l’aide de la commande add-ImageDriverPackage](using-the-add-imagedriverpackage-command.md)
[à l’aide de la sous-commande add-AllDriverPackages](using-the-add-alldriverpackages-subcommand.md)