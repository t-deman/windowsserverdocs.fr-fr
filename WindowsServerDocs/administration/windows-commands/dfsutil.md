---
title: dfsutil
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ef5093a4-0d24-4b21-9d04-59933ad98e2c
robots: noindex,nofollow
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 45545b4e12d31c293ead5b18b83efd50d7bc37bb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439651"
---
# <a name="dfsutil"></a>dfsutil

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La commande dfsutil gère les espaces de noms DFS, les serveurs et clients. commandes de Dfsutil utilisent la terminologie de système de fichiers distribués d’origine, avec mise à jour les espaces de noms DFS la terminologie fournie comme explication pour la plupart des commandes.

Pour obtenir des exemples d’utilisation de cette commande, consultez 

## <a name="syntax"></a>Syntaxe

```
command </parameter> </param2>
```

### <a name="parameters"></a>Paramètres

|Paramètre|Description|
|-------|--------|
|[Dfsutil racine](dfsutil-root.md)|Affiche, crée, supprime, importe, exporte les racines de l’espace de noms.|
|[Dfsutil lien](dfsutil-link.md)|Affiche, crée, supprime ou déplace des dossiers \(liens\).|
|[Dfsutil cible](dfsutil-target.md)|Affichages, créer, supprimer le serveur cible ou l’espace de noms de dossier.|
|[Dfsutil, propriété](dfsutil-property.md)|Affiche ou modifie un serveur cible ou l’espace de noms de dossier.|
|[Dfsutil Client](dfsutil-client.md)|Affiche ou modifie les clés d’informations ou de Registre du client.|
|[Dfsutil Server](dfsutil-server.md)|Affiche ou modifie la configuration de l’espace de noms.|
|[Dfsutil Diag](dfsutil-diag.md)|Effectuer des diagnostics ou afficher dfsdirs\/dfspath.|
|[Dfsutil domaine](dfsutil-domain.md)|Affiche tous les domaine\-en fonction des espaces de noms dans un domaine.|
|[dfsutil Cache](dfsutil-cache.md)|Affiche ou vide le cache du client.|
|[dfsutil oldcli](dfsutil-oldcli.md)|Utilisez la commande dfsutil \/commande oldcli à l’utilisation de la syntaxe de commande dfsutil d’origine.|

## <a name="remarks-optional-section"></a>Remarques <optional section>
Si vous spécifiez un objet \(tel qu’un serveur d’espace de noms\) à la fin d’une commande, la plupart des commandes affiche des informations sur l’objet sans nécessiter davantage de paramètres ou des commandes. Par exemple, lorsque vous utilisez la commande Root dfsutil, vous pouvez ajouter un espace de noms racine à la commande pour afficher des informations sur la racine.

## <a name="BKMK_Examples"></a>Exemples
&lt;Voici où vous placez une description détaillée de votre exemple.&gt;

```
This /is /the /example /of /calling /command /with /parameters
```

&lt;Voici où vous placez une description détaillée d’un autre exemple.&gt;

```
This /is /a:different /example
```

## <a name="additional-references"></a>Références supplémentaires

-   [Clé de syntaxe de ligne de commande](command-line-syntax-key.md)


