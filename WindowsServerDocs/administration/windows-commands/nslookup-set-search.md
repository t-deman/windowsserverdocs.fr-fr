---
title: nslookup set search
description: 'Rubrique de commandes de Windows pour ***- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d95ebe30ce45430787bebbfe63766a571a436bbf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66436590"
---
# <a name="nslookup-set-search"></a>nslookup set search



Ajoute les noms de domaine système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande jusqu'à ce qu’une réponse est reçue. Cela s’applique lorsque le jeu et la recherche demandent contenir au moins un point, mais ne se terminent pas par un point final.

## <a name="syntax"></a>Syntaxe

```
set [no]search
```

## <a name="parameters"></a>Paramètres

|  Paramètre   |                                                                          Description                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Arrête l’ajout des noms de domaine le système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande.                            |
|  **search**  | Ajoute les noms de domaine système DNS (Domain Name) dans la liste de recherche du domaine DNS à la demande jusqu'à ce qu’une réponse est reçue. La syntaxe par défaut est **recherche**. |
|    {aide     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Références supplémentaires

[Clé de syntaxe de ligne de commande](command-line-syntax-key.md)