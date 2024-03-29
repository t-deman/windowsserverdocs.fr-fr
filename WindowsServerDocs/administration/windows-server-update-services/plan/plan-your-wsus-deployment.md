---
title: Planifier votre déploiement de WSUS
description: Rubrique de Windows Server Update Service (WSUS) - une vue d’ensemble du processus avec des liens vers les rubriques connexes de planification du déploiement
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 35865398-b011-447a-b781-1c52bc0c9e3a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 05/24/2018
ms.openlocfilehash: a568324ba69b13c7016f4715d3c37f991ae4c1ad
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66439724"
---
# <a name="plan-your-wsus-deployment"></a>Planifier votre déploiement WSUS

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La première étape du déploiement de WSUS (Windows Server Update Services) consiste à prendre des décisions importantes, par exemple le choix du scénario de déploiement de WSUS, le choix d’une topologie de réseau et la prise de connaissance de la configuration requise. La liste de vérification suivante résume les étapes impliquées dans la préparation de votre déploiement.

|Tâche|Description|
|----|--------|
|[1.1. Configuration système requise et considérations relatives à la révision](#11-review-considerations-and-system-requirements)|Passez en revue la liste des considérations et la configuration requise pour vous assurer de disposer de tous les matériels et logiciels nécessaires pour déployer WSUS.|
|[1.2. Choisissez un scénario de déploiement de WSUS](#12-choose-a-wsus-deployment-scenario)|Choisissez le scénario de déploiement de WSUS à utiliser.|
|[1.3. Choisir une stratégie de stockage WSUS](#13-choose-a-wsus-storage-strategy)|Choisissez la stratégie de stockage WSUS la mieux adaptée à votre déploiement.|
|[1.4. Choisissez les que langues de mise à jour WSUS](#14-choose-wsus-update-languages)|Choisissez les langues de mise à jour WSUS à installer.|
|[1.5. Planifier des groupes d’ordinateurs WSUS](#15-plan-wsus-computer-groups)|Planifiez l’approche des groupes d’ordinateurs WSUS à utiliser pour votre déploiement.|
|[1.6. Planifier les considérations sur les performances WSUS : Service de transfert Intelligent en arrière-plan](#16-plan-wsus-performance-considerations)|Planifiez une conception WSUS pour des performances optimisées.|
|[1.7. Planifier les paramètres de mises à jour automatiques](#17-plan-automatic-updates-settings)|Planifiez la configuration des paramètres des Mises à jour automatiques pour votre scénario.|

## <a name="11-review-considerations-and-system-requirements"></a>1.1. Passer en revue les considérations initiales et la configuration requise

### <a name="system-requirements"></a>Configuration système

Avant d’activer le rôle serveur WSUS, vérifiez que le serveur répond à la configuration requise et que vous disposez des autorisations nécessaires pour effectuer l’installation en vous conformant aux recommandations suivantes :

-   La configuration matérielle requise du serveur pour activer le rôle WSUS dépend de la configuration matérielle requise. Les configurations matérielles minimales requises pour WSUS sont les suivantes :

    -   **Processeur :** 1,4 gigahertz (GHz) x64 processeur (2 Ghz ou supérieur recommandé)

    -   **Mémoire :** WSUS exige 2 Go de RAM plus que ce qui est requis par le serveur et tous les autres logiciels ou services.

    -   **Espace disque disponible :** 10 Go (40 Go ou supérieur recommandé)

    -   **Carte réseau :** 100 mégabits par seconde (Mbits/s) ou supérieur

-   Configuration logicielle requise :

    -   Pour consulter des rapports, WSUS nécessite [Microsoft Report Viewer Redistributable 2008](https://www.microsoft.com/download/details.aspx?id=6576). Sur Windows Server 2016, WSUS nécessite [Microsoft Report Viewer Runtime 2012](https://www.microsoft.com/download/details.aspx?id=35747)

-   Si vous installez des rôles ou des mises à jour logicielles qui vous demandent de redémarrer le serveur une fois l’installation terminée, redémarrez le serveur avant d’activer le rôle serveur WSUS.

-   Microsoft .NET Framework 4.0 doit être installé sur le serveur sur lequel le rôle serveur WSUS sera installé.

-   Le compte Autorité NT\Service réseau doit disposer des autorisations de contrôle total pour les dossiers suivants afin que le composant logiciel enfichable Administration WSUS s’affiche correctement :

    -   %windir%\Microsoft.NET\Framework\v4.0.30319\Temporary ASP.NET Files

        > [!NOTE]
        > Il est possible que ce chemin d’accès n’existe pas avant l’installation du rôle Serveur Web qui contient Internet Information Services (IIS).

    -   %windir%\Temp

-   Vérifiez que le compte que vous envisagez d’utiliser pour installer WSUS est un membre du groupe Administrateurs local.

### <a name="installation-considerations"></a>Considérations relatives à l'installation

Au cours du processus d’installation, WSUS installera les éléments suivants par défaut :

-   API .NET et applets de commande Windows PowerShell

-   Base de données interne Windows, qui est utilisée par WSUS

-   Services utilisés par WSUS, qui sont les suivants :

    -   Service de mise à jour

    -   Service Web de création de rapports

    -   Service Web client

    -   Service Web d’authentification Web simple

    -   Service de synchronisation de serveur

    -   Service Web d’authentification DSS

### <a name="features-on-demand-considerations"></a>Considérations sur les fonctionnalités à la demande

Configurer des ordinateurs clients (y compris les serveurs) pour mettre à jour à l’aide de WSUS entraîne les limitations suivantes :

1. Les rôles de serveur qui ont eu leur charge utile supprimée à l’aide des fonctionnalités à la demande ne peuvent pas être installés à la demande à partir de Microsoft Update. Vous devez fournir une source d’installation au moment où que vous essayez d’installer ces rôles de serveur, ou configurer une source pour les fonctionnalités à la demande dans la stratégie de groupe.

2. Les éditions Client Windows ne seront pas en mesure d’installer .NET 3.5 à la demande à partir du web. Les mêmes considérations que pour les rôles de serveur s’appliquent à .NET 3.5.

   > [!NOTE]
   > Configuration de des fonctionnalités sur la source d’installation à la demande n’implique pas WSUS. Pour plus d’informations sur comment configurer les fonctionnalités, voir [Configurer des fonctionnalités à la demande dans Windows Server](https://technet.microsoft.com/library/jj127275.aspx).

3. Appareils d’entreprise exécutant Windows 10, version 1709 ou version 1803, Impossible d’installer toutes les fonctionnalités à la demande directement à partir de WSUS. Pour installer des fonctionnalités à la demande, [créer un fichier de fonctionnalité (magasin côte à côte)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127275%28v=ws.11%29#create-a-feature-file-or-side-by-side-store) ou obtenir la fonctionnalité sur le package à la demande à partir d’une des sources suivantes :
   - [Volume Licensing Service Center](https://www.microsoft.com/licensing/servicecenter) (VLSC) - l’accès avec licence en volume est requis
   - Portail de l’OEM - accès aux OEM est requis
   - Téléchargement MSDN - abonnement MSDN est requis

     Individuellement obtenu la fonctionnalité sur les packages à la demande peut être installée à l’aide de [les options de ligne de commande DISM](https://docs.microsoft.com/windows-hardware/manufacture/desktop/dism-operating-system-package-servicing-command-line-options).

### <a name="wsus-database-requirements"></a>Base de données WSUS requise
WSUS requiert l’une des bases de données suivantes :

-   Base de données interne Windows

-   Microsoft SQL Server 2017

-   Microsoft SQL Server 2016

-   Microsoft SQL Server 2014

-   Microsoft SQL Server 2012

-   Microsoft SQL Server 2008 R2

Les éditions suivantes de SQL Server sont prises en charge par WSUS :

-   Standard

-   Entreprise

-   Express

> [!NOTE]
> SQL Server Express 2008 R2 a une limite de taille de base de données de 10 Go. Cette taille de base de données semble suffisante pour WSUS, même si l’utilisation de cette base de données ne présente pas d’avantage notable par rapport à WID. Base de données WID a une mémoire RAM requise de 2 Go au-delà de la configuration requise Windows Server standard.

Vous pouvez installer le rôle WSUS sur un ordinateur distinct du serveur de base de données. Dans ce cas, les critères supplémentaires suivants s’appliquent :

1.  Le serveur de base de données ne peut pas être configuré comme un contrôleur de domaine.

2.  Le serveur WSUS ne peut pas exécuter les services Bureau à distance.

3.  Le serveur de base de données doit être dans le même domaine active directory que le serveur WSUS, ou il doit avoir une relation d’approbation avec le domaine active directory du serveur WSUS.

4.  Le serveur WSUS et le serveur de base de données doivent se trouver dans le même fuseau horaire ou être synchronisés avec la même source de temps d’universel coordonné (heure de Greenwich).

## <a name="12-choose-a-wsus-deployment-scenario"></a>1.2. Choisir le scénario de déploiement de WSUS
Cette section décrit les fonctionnalités de base de tous les déploiements de WSUS. Utilisez cette section pour vous familiariser avec un déploiement simple impliquant un seul serveur WSUS ainsi qu’avec des scénarios plus complexes, tels qu’une hiérarchie de serveurs WSUS ou un serveur WSUS sur un segment de réseau isolé.

### <a name="simple-wsus-deployment"></a>Déploiement de WSUS simple
Le déploiement de WSUS plus simple se compose d’un serveur à l’intérieur du pare-feu d’entreprise qui sert les ordinateurs clients sur un intranet privé. Le serveur WSUS se connecte à Microsoft Update pour télécharger les mises à jour. Cette opération est connue sous le nom de *synchronisation*. Lors de la synchronisation, WSUS détermine si de nouvelles mises à jour ont été mises à disposition depuis la dernière synchronisation. S’il s’agit de votre première synchronisation de WSUS, toutes les mises à jour sont disponibles en téléchargement.

> [!NOTE]
> La synchronisation initiale peut durer plus d’une heure. Toutes les synchronisations suivantes devraient être considérablement plus rapides.

Par défaut, le serveur WSUS utilise le port 80 pour le protocole HTTP et le port 443 pour le protocole HTTPS pour obtenir les mises à jour de Microsoft. S’il existe un pare-feu d’entreprise entre votre réseau et Internet, vous devez ouvrir ces ports sur le serveur qui communique directement avec Microsoft Update. Si vous envisagez d’utiliser des ports personnalisés pour cette communication, vous devez ouvrir ces ports à la place. Vous pouvez configurer plusieurs serveurs WSUS à synchroniser avec un serveur WSUS parent. Par défaut, le serveur WSUS utilise le port 8530 pour le protocole HTTP et le port 8531 pour le protocole HTTPS pour fournir les mises à jour aux postes de travail clients.

### <a name="multiple-wsus-servers"></a>Plusieurs serveurs WSUS
Les administrateurs peuvent déployer plusieurs serveurs exécutant WSUS qui synchronisent tout le contenu au sein de l’intranet de leur organisation. Vous pouvez exposer qu’un seul serveur à Internet, ce qui est le seul serveur qui télécharge les mises à jour à partir de Microsoft Update. Ce serveur est configuré en tant que le serveur en amont la source à laquelle synchronisent les serveurs en aval. Le cas échéant, les serveurs peuvent être situés sur un réseau multisite afin d’offrir la meilleure connectivité à tous les ordinateurs clients.

### <a name="disconnected-wsus-server"></a>Serveur WSUS déconnecté
Si la stratégie d’entreprise ou d’autres circonstances limitent l’accès des ordinateurs à Internet, les administrateurs peuvent configurer un serveur interne pour exécuter WSUS. Il peut s’agir, par exemple, d’un serveur connecté à l’intranet, mais isolé d’Internet. Après avoir téléchargé, testé et approuvé les mises à jour sur ce serveur, un administrateur exporte les métadonnées et le contenu de mise à jour vers un DVD. Les métadonnées et le contenu de mise à jour sont importés du DVD vers les serveurs exécutant WSUS à l’intérieur de l’intranet.

### <a name="wsus-server-hierarchies"></a>Hiérarchies de serveurs WSUS
Vous pouvez créer des hiérarchies complexes de serveurs WSUS. Dans la mesure où vous pouvez synchroniser un serveur WSUS avec un autre serveur WSUS plutôt qu’avec Microsoft Update, vous n’avez besoin que d’un seul serveur WSUS connecté à Microsoft Update. Lorsque vous reliez des serveurs WSUS, il existe un serveur WSUS en amont et un serveur WSUS en aval. Le déploiement d’une hiérarchie de serveurs WSUS présente les avantages suivants :

-   Vous pouvez télécharger les mises à jour une fois à partir d’Internet, puis les distribuer aux ordinateurs clients en utilisant les serveurs en aval. Cette méthode permet d’économiser la bande passante sur la connexion Internet d’entreprise.

-   Vous pouvez télécharger les mises à jour sur un serveur WSUS qui est physiquement plus proche des ordinateurs clients, par exemple dans les filiales.

-   Vous pouvez configurer des serveurs WSUS séparés pour servir des ordinateurs clients qui utilisent différentes langues de produits Microsoft.

-   Vous pouvez adapter WSUS à une grande organisation qui dispose de plus d’ordinateurs clients qu’un serveur WSUS ne peut efficacement gérer.

> [!NOTE] 
> Nous vous conseillons de ne pas créer une hiérarchie de serveurs WSUS avec une profondeur de plus de trois niveaux. Chaque niveau ajoute du temps pour propager les mises à jour sur tous les serveurs connectés. Bien qu’il n’existe aucune limite théorique à une hiérarchie, seuls les déploiements qui ont une hiérarchie de cinq niveaux ont été testés par Microsoft.
>
> En outre, les serveurs en aval doivent être à la même version ou une version antérieure de WSUS comme source de synchronisation de serveur en amont.

Vous pouvez connecter des serveurs WSUS en mode autonome (pour obtenir une gestion distribuée) ou en mode Réplica (pour obtenir une gestion centralisée). Vous n’avez pas à déployer une hiérarchie de serveurs qui n’utilise qu’un mode : vous pouvez déployer une solution WSUS qui utilise des serveurs WSUS en mode autonome et en mode Réplica.

#### <a name="autonomous-mode"></a>Mode autonome
Le mode autonome, également appelé gestion distribuée, est l’option d’installation par défaut pour WSUS. En mode autonome, un serveur WSUS en amont partage des mises à jour avec des serveurs en aval pendant la synchronisation. Les serveurs WSUS en aval sont gérés séparément et ne reçoivent pas de statut d’approbation de mise à jour ni d’informations sur les groupes d’ordinateurs du serveur en amont. En utilisant le modèle de gestion distribuée, chaque administrateur de serveur WSUS sélectionne des langues de mise à jour, crée des groupes d’ordinateurs, affecte des ordinateurs à des groupes, teste et approuve les mises à jour, et vérifie que les mises à jour appropriées sont installées dans les groupes d’ordinateurs adéquats. L’image suivante illustre la façon dont vous pouvez déployer des serveurs WSUS autonomes dans un environnement de filiales :

#### <a name="replica-mode"></a>Mode Réplica :
Le mode Réplica, également appelé gestion centralisée, fonctionne en ayant un serveur WSUS en amont qui partage des mises à jour, un statut d’approbation et des groupes d’ordinateurs avec des serveurs en aval. Les serveurs réplica héritent des approbations de mise à jour et ne sont pas administrés séparément du serveur WSUS en amont. L’image suivante illustre la façon dont vous pouvez déployer des serveurs WSUS de réplication dans un environnement de filiales :

> [!NOTE]
> Si vous configurez plusieurs serveurs réplica pour une connexion à un serveur WSUS en amont unique, ne planifiez pas l’exécution de la synchronisation en même temps sur chaque serveur réplica. Vous éviterez ainsi de soudaines surtensions dans l’utilisation de la bande passante.

### <a name="branch-offices"></a>Filiales
Vous pouvez exploiter la fonctionnalité de filiale de Windows pour optimiser le déploiement de WSUS. Ce type de déploiement présente les avantages suivants :

1.  permet de réduire l’utilisation de liaisons réseau étendu et améliore la réactivité de l’application. Pour activer l’accélération fournie par BranchCache pour l’accès au contenu délivré le serveur WSUS, vous devez installer la fonctionnalité BranchCache sur le serveur et les clients, puis vérifier que le service BranchCache a démarré. Aucune autre étape n’est requise.

2.  Dans les filiales avec des connexions à bande passante faible au bureau central, mais avec des connexions à bande passante élevée à Internet, la fonctionnalité de filiale peut également être utilisée. Dans ce cas, vous pouvez configurer des serveurs WSUS en aval pour qu’ils obtiennent des informations sur les mises à jour à installer du serveur WSUS central, mais qu’ils téléchargent les mises à jour à partir de Microsoft Update.

### <a name="network-load-balancing"></a>Équilibrage de la charge réseau
L’équilibrage de la charge réseau augmente la fiabilité et les performances de votre réseau WSUS. Vous pouvez configurer plusieurs serveurs WSUS qui partagent un cluster de basculement exécutant SQL Server tels que SQL Server 2008 R2 SP1. Dans cette configuration, vous devez utiliser une installation de SQL Server complète, et non l’installation de la base de données interne Windows qui est fournie par WSUS, et le rôle de base de données doit être installé sur tous les serveurs frontaux WSUS. Vous pouvez également faire en sorte que tous les serveurs WSUS utilisent un système de fichiers distribués (DFS) pour stocker leur contenu.

**Le programme d’installation WSUS pour NLB :** contrairement au programme d’installation WSUS 3.2 pour NLB, aucun appel d’installation spéciales et les paramètres ne sont ne sont plus requis pour configurer WSUS pour NLB. Vous devez uniquement installer chaque serveur WSUS, en tenant compte des considérations suivantes.

-   WSUS doit être configuré à l’aide de l’option de base de données SQL au lieu de WID.

-   Si vous stockez les mises à jour localement, le même dossier de contenu doit être partagé entre les serveurs WSUS utilisant la même base de données SQL.

-   Le programme d’installation WSUS doit être effectué en série. Les tâches de postinstallation ne peuvent pas être exécutées sur plusieurs serveurs en même temps lors du partage de la même base de données SQL.

### <a name="wsus-deployment-with-roaming-client-computers"></a>Déploiement de WSUS avec des ordinateurs clients itinérants
Si le réseau comprend des utilisateurs mobiles qui se connectent au réseau à partir de différents emplacements, vous pouvez configurer WSUS pour permettre aux utilisateurs itinérants de mettre à jour leurs ordinateurs clients à partir du serveur WSUS qui leur est le plus proche géographiquement. Par exemple, vous pourrez déployer un serveur WSUS chaque région et utiliser un sous-réseau DNS différent pour chaque région. Tous les ordinateurs clients pourraient être redirigés vers le même serveur WSUS, ce qui résout chaque sous-réseau au serveur WSUS physique le plus proche.

## <a name="13-choose-a-wsus-storage-strategy"></a>1.3. Choisir la stratégie de stockage WSUS
WSUS (Windows Server Update Services) utilise deux types de systèmes de stockage : une base de données pour stocker les métadonnées de configuration WSUS et de mise à jour, et un système de fichiers local facultatif pour stocker les fichiers de mise à jour. Avant d’installer WSUS, vous devez décider du mode d’implémentation du stockage.

Les mises à jour sont constituées de deux parties : les métadonnées qui décrivent la mise à jour et les fichiers requis pour installer la mise à jour. Les métadonnées de mise à jour sont généralement beaucoup plus petites que la mise à jour réelle et elles sont stockées dans la base de données WSUS. Les fichiers de mise à jour sont stockés sur un serveur WSUS local ou sur un serveur Web Microsoft Update.

### <a name="wsus-database"></a>Base de données WSUS
WSUS requiert une base de données pour chaque serveur WSUS. WSUS prend en charge l’utilisation d’une base de données qui se trouve sur un ordinateur différent du serveur WSUS, avec quelques restrictions. Pour une liste des bases de données pris en charge et des limitations de la base de données distante, consultez la section « 1.1 Considérations initiales de révision et la configuration système requise, » dans ce guide.

La base de données WSUS stocke les informations suivantes :

-   Informations de configuration du serveur WSUS

-   Métadonnées qui décrivent chaque mise à jour

-   Informations sur les ordinateurs clients, les mises à jour et les interactions

Si vous installez plusieurs serveurs WSUS, vous devez gérer une base de données distincte pour chaque serveur WSUS, qu’il s’agisse d’un serveur autonome ou de réplication. Vous ne pouvez pas stocker plusieurs bases de données WSUS sur une seule instance de SQL Server, à l’exception des clusters d’équilibrage de la charge réseau qui utilisent le basculement SQL Server.

SQL Server, SQL Server Express et la base de données interne Windows fournissent les mêmes caractéristiques de performances pour une configuration sur un serveur unique, où la base de données et le service WSUS sont situés sur le même ordinateur. Une configuration sur un serveur unique peut prendre en charge plusieurs milliers d’ordinateurs clients WSUS.

> [!NOTE]
> Ne tentez pas de gérer WSUS en accédant directement à la base de données. manipuler directement la base de données peut entraîner la corruption de base de données. Il est possible que l’endommagement ne soit pas immédiatement évident, mais il peut empêcher des mises à niveau vers la version suivante du produit. Vous pouvez gérer WSUS en utilisant la console WSUS ou les interfaces de programmation d’applications (API) WSUS.

#### <a name="wsus-with-windows-internal-database"></a>WSUS avec la base de données interne Windows
Par défaut, l’Assistant Installation crée et utilise une base de données interne Windows nommée SUSDB.mdf. Cette base de données est située dans le dossier %windir%\wid\data\, où %windir% est le lecteur local sur lequel le logiciel du serveur WSUS est installé.

> [!NOTE]
> Base de données interne de Windows (WID) a été introduite dans Windows Server 2008.

WSUS prend en charge l’authentification Windows uniquement pour la base de données. Vous ne pouvez pas utiliser l’authentification SQL Server avec WSUS. Si vous utilisez la base de données interne Windows pour la base de données WSUS, le programme d’installation de WSUS crée une instance de SQL Server nommée server\Microsoft##WID, où server est le nom de l’ordinateur. Avec l’une ou l’autre des options de la base de données, le programme d’installation de WSUS crée une base de données nommée SUSDB. Le nom de cette base de données ne peut pas être configuré.

Nous vous conseillons d’utiliser la base de données interne Windows dans les cas suivants :

-   L’organisation n’a pas déjà acheté et ne requiert pas de produit SQL Server pour toute autre application.

-   L’organisation ne requiert pas de solution WSUS d’équilibrage de la charge réseau.

-   Vous avez l’intention de déployer plusieurs serveurs WSUS (par exemple, dans les filiales). Dans ce cas, vous devez envisager d’utiliser la base de données interne Windows sur les serveurs secondaires, même si vous allez utiliser SQL Server pour le serveur WSUS racine. Dans la mesure où chaque serveur WSUS requiert une instance distincte de SQL Server, vous allez rapidement être confronté à des problèmes de performances de base de données si une seule instance de SQL Server gère plusieurs serveurs WSUS.

La base de données interne Windows ne fournit pas d’interface utilisateur ni d’outils de gestion de base de données. Si vous sélectionnez cette base de données pour WSUS, vous devez utiliser des outils externes pour gérer la base de données. Pour plus d'informations, consultez :

-   [Sauvegarde et restauration des données WSUS et sauvegarde de votre serveur](https://technet.microsoft.com/library/dd939904(WS.10).aspx)

-   [Réindexer la base de données WSUS](https://technet.microsoft.com/library/dd939795(WS.10).aspx)

#### <a name="wsus-with-sql-server"></a>WSUS avec SQL Server
Nous vous conseillons d’utiliser SQL Server avec WSUS dans les cas suivants :

1.  Vous avez besoin d’une solution WSUS d’équilibrage de la charge réseau.

2.  Au moins une instance de SQL Server est déjà installée.

3.  Vous ne pouvez pas exécuter le service SQL Server sous un compte non-système local ou en utilisant l’authentification SQL Server. WSUS prend en charge l’authentification Windows uniquement.

### <a name="wsus-update-storage"></a>Stockage des mises à jour WSUS
Lorsque les mises à jour sont synchronisées sur votre serveur WSUS, les fichiers de mise à jour et les métadonnées sont stockés à deux endroits différents. Les métadonnées sont stockées dans la base de données WSUS. Les fichiers de mise à jour peuvent être stockés sur votre serveur WSUS ou sur des serveurs Microsoft Update, en fonction de la manière dont vous avez configuré vos options de synchronisation. Si vous choisissez de stocker les fichiers de mise à jour sur votre serveur WSUS, les ordinateurs clients téléchargent les mises à jour approuvées à partir du serveur WSUS local. Sinon, les ordinateurs clients téléchargent les mises à jour approuvées directement à partir de Microsoft Update. L’option la plus appropriée pour votre organisation dépend de la bande passante du réseau pour Internet, de la bande passante du réseau sur l’intranet et de la disponibilité de stockage en local.

Vous pouvez sélectionner une autre solution de stockage des mises à jour pour chaque serveur WSUS que vous déployez.

#### <a name="local-wsus-server-storage"></a>Stockage de serveur WSUS en local
Le stockage en local des fichiers de mise à jour est l’option par défaut lorsque vous installez et configurez WSUS. Cette option permet d’économiser la bande passante sur la connexion d’entreprise à Internet, car les ordinateurs clients téléchargent les mises à jour directement à partir du serveur WSUS local.

Cette option nécessite que le serveur dispose de suffisamment d’espace disque pour stocker toutes les mises à jour requises. au minimum, WSUS requiert 20 Go stocker les mises à jour localement ; Toutefois, nous recommandons 30 Go en fonction des variables testées.

#### <a name="remote-storage-on-microsoft-update-servers"></a>Stockage étendu sur les serveurs Microsoft Update
Vous pouvez stocker les mises à jour à distance sur les serveurs Microsoft Update. Cette option est utile si la plupart des ordinateurs clients se connectent au serveur WSUS via une connexion de réseau étendu lente, mais qu’ils se connectent à Internet via une connexion à bande passante élevée.

Dans ce cas, le serveur WSUS racine se synchronise avec Microsoft Update et reçoit les métadonnées de mise à jour. Après que vous avez approuvé les mises à jour, les ordinateurs clients téléchargent les mises à jour approuvées à partir des serveurs Microsoft Update.

## <a name="14-choose-wsus-update-languages"></a>1.4. Choisir les langues de mise à jour WSUS
Lorsque vous déployez une hiérarchie de serveurs WSUS, vous devez identifier les mises à jour des langues requises dans toute l’organisation. Vous devez configurer le serveur WSUS racine pour télécharger les mises à jour dans toutes les langues utilisées dans toute l’organisation.

Par exemple, le siège social peut nécessiter les mises à jour des langues anglaise et française, mais une filiale a besoin des mises à jour des langues anglaise, française et allemande, et une autre a besoin des mises à jour des langues anglaise et espagnole. Dans ce cas, vous configurez le serveur WSUS racine pour télécharger les mises à jour en anglais, français, allemand et espagnol. Vous configurez ensuite le serveur WSUS de la première filiale pour télécharger les mises à jour en anglais, français et allemand uniquement, puis celui de la deuxième filiale pour télécharger les mises à jour en anglais et espagnol uniquement.

La page **Choisir les langues** de l’Assistant Configuration de WSUS vous permet d’obtenir des mises à jour dans toutes les langues ou un sous-ensemble de langues. sélectionner un sous-ensemble de langues économise de l’espace de disque, mais il est IMPORTANT de choisir toutes les langues qui sont requises par tous les serveurs en aval et ordinateurs clients d’un serveur WSUS.

Voici quelques remarques importantes sur le langage de mise à jour que vous devez prendre en compte avant de configurer cette option :

-   Incluez toujours l’anglais en plus de toutes les autres langues requises dans toute l’organisation. Toutes les mises à jour sont basées sur des modules linguistiques en anglais.

-   Les serveurs en aval et ordinateurs clients ne recevront pas toutes les mises à jour dont ils ont besoin si vous n’avez pas sélectionné toutes les langues nécessaires pour le serveur en amont. Veillez à sélectionner toutes les langues nécessaires à tous les ordinateurs clients associés à tous les serveurs en aval.

-   Vous devez généralement télécharger les mises à jour dans toutes les langues sur le serveur WSUS racine qui est synchronisé avec Microsoft Update. Cette sélection garantit que tous les serveurs en aval et ordinateurs clients recevront les mises à jour dans les langues requises.

Si vous stockez des mises à jour en local et que vous avez configuré un serveur WSUS pour télécharger les mises à jour dans un nombre limité de langues, vous pouvez remarquer qu’il existe des mises à jour dans des langues autres que celles que vous avez indiquées. De nombreux fichiers de mise à jour sont des lots de plusieurs langues différentes, qui comprennent au moins l’une des langues spécifiées sur le serveur.

**Serveurs en amont**

> [!NOTE]
> Configurez les serveurs en amont pour synchroniser les mises à jour dans toutes les langues requises par les serveurs de réplication en aval. Vous ne serez pas informé des mises à jour nécessaires pour les langues non synchronisées.

Les mises à jour apparaîtront en tant que **Non applicable** sur les ordinateurs clients qui requièrent la langue. Pour éviter ce problème, assurez-vous que toutes les langues du système d’exploitation sont incluses dans les options de synchronisation du serveur WSUS. Vous pouvez voir toutes les langues de système d’exploitation en accédant à la **ordinateurs** vue de la Console d’Administration WSUS et trier les ordinateurs en langue du système d’exploitation. Toutefois, vous souhaitez inclure davantage de langues s’il existe des applications de Microsoft dans plusieurs langues (par exemple, si la version Français de Microsoft Word est installée sur certains ordinateurs qui utilisent la version anglaise de Windows 8.

Le choix de langues pour un serveur en amont n’est pas le même que le choix de langues pour un serveur en aval. Les procédures ci-dessous expliquent comment identifier les différences.

#### <a name="to-choose-update-languages-for-a-server-synchronizing-from-microsoft-update"></a>Pour choisir les langues de mise à jour d’un serveur de synchronisation à partir de Microsoft Update

1.  Dans l’Assistant Configuration WSUS :

    -   Pour obtenir les mises à jour dans toutes les langues, cliquez sur **Télécharger les mises à jour dans toutes les langues, y compris les nouvelles langues**.

    -   Si vous choisissez d’obtenir les mises à jour pour quelques langues uniquement, cliquez sur **Télécharger les mises à jour dans ces langues uniquement**, puis sélectionnez les langues correspondantes.

#### <a name="to-choose-update-languages-for-a-downstream-server"></a>Pour choisir les langues de mise à jour d’un serveur en aval

1.  Si le serveur en amont a été configuré pour télécharger les fichiers de mise à jour dans un sous-ensemble de langues : Dans l’Assistant Configuration WSUS, cliquez sur **Télécharger les mises à jour dans les langues suivantes (seules les langues marquées d’un astérisque sont prises en charge par le serveur en amont)** , puis sélectionnez les langues pour lesquelles vous souhaitez effectuer les mises à jour.

> [!NOTE]
> Vous devez effectuer cette opération même si vous voulez que le serveur en aval puisse télécharger les mêmes langues que le serveur en amont.

2. Si le serveur en amont a été configuré pour télécharger les fichiers de mise à jour dans toutes les langues : Dans l’Assistant Configuration WSUS, cliquez sur **Télécharger les mises à jour dans toutes les langues prises en charge par le serveur en amont**.

> [!NOTE]
> Vous devez effectuer cette opération même si vous voulez que le serveur en aval puisse télécharger les mêmes langues que le serveur en amont. Ce paramètre permet au serveur en amont de télécharger les mises à jour dans toutes les langues, y compris celles qui n’étaient pas initialement configurées pour le serveur en amont. Si vous ajoutez des langues sur le serveur en amont, vous devez copier les nouvelles mises à jour sur ses serveurs de réplication.
>
> La modification des options de langue uniquement sur le serveur en amont peut entraîner une incohérence entre le nombre de mises à jour approuvées sur le serveur central et le nombre de mises à jour approuvées sur les serveurs de réplication.

## <a name="15-plan-wsus-computer-groups"></a>1.5. Planifier les groupes d’ordinateurs WSUS
WSUS vous permet de cibler les mises à jour vers des groupes d’ordinateurs clients afin que vous puissiez vous assurer que des ordinateurs spécifiques obtiennent toujours les mises à jour adéquates au moment le plus approprié. Par exemple, si tous les ordinateurs d’un service (tel que l’équipe Comptabilité) ont une configuration spécifique, vous pouvez configurer un groupe pour cette équipe, décider des mises à jour dont leurs ordinateurs ont besoin et l’heure à laquelle elles doivent être installées, puis utiliser les rapports WSUS pour évaluer les mises à jour pour l’équipe.

> [!NOTE]
> Si un serveur WSUS est exécuté en mode Réplica, il n’est pas possible de créer des groupes d’ordinateurs sur ce serveur. Tous les groupes d’ordinateurs nécessaires pour les ordinateurs clients du serveur de réplication doivent être créés sur le serveur WSUS qui est la racine de la hiérarchie de serveurs WSUS. Pour plus d’informations sur le mode réplica, consultez gérer les serveurs WSUS réplica [gérer les serveurs WSUS réplica](https://technet.microsoft.com/library/dd939893(WS.10).aspx) dans le Guide des opérations WSUS 3.0 SP2.

Les ordinateurs sont toujours affectés à la **tous les ordinateurs** groupe et ils restent affectés au **ordinateurs non assignés** groupe tant que vous les affectez à un autre groupe. Les ordinateurs peuvent appartenir à plusieurs groupes.

Les groupes d’ordinateurs peuvent être configurés en hiérarchies (par exemple, le groupe Traitement des paies et le groupe Compte fournisseurs sous le groupe Comptabilité). Les mises à jour qui sont approuvées pour un groupe supérieur sont automatiquement déployées vers les groupes inférieurs, en plus du groupe supérieur. Dans cet exemple, si vous approuvez Update1 pour le groupe Comptabilité, la mise à jour sera déployée vers tous les ordinateurs du groupe Comptabilité, tous les ordinateurs du groupe Traitement des paies et tous les ordinateurs du groupe Compte fournisseurs.

Dans la mesure où les ordinateurs peuvent être affectés à plusieurs groupes, il est possible pour une seule mise à jour d’être approuvée plusieurs fois pour le même ordinateur. Toutefois, la mise à jour ne sera déployée qu’une seule fois et tous les conflits seront résolus par le serveur WSUS. Pour continuer avec l’exemple précédent, si computerA est affecté au groupe traitement des paies et le groupe compte fournisseurs, et si Update1 est approuvée pour les deux groupes, il sera déployé qu’une seule fois.

Vous pouvez affecter les ordinateurs à des groupes d’ordinateurs en adoptant l’une des deux méthodes, à savoir le ciblage côté serveur ou le ciblage côté client. Voici les définitions de chaque méthode :

-   **Ciblage côté serveur** : Vous affectez manuellement un ou plusieurs ordinateurs clients à plusieurs groupes simultanément.

-   **Ciblage côté client** : Vous utilisez la stratégie de groupe ou modifiez les paramètres du Registre sur les ordinateurs clients pour permettre à ces ordinateurs d’être automatiquement ajoutés aux groupes d’ordinateurs précédemment créés.

### <a name="conflict-resolution"></a>Résolution de conflits
Le serveur applique les règles suivantes pour résoudre les conflits et déterminer l’action résultante sur les clients :

1.  Priority

2.  Installer/Désinstaller

3.  Échéance

#### <a name="priority"></a>Priority
Les actions associées au groupe présentant la priorité la plus élevée remplacent les actions des autres groupes. Plus un groupe apparaît profondément dans la hiérarchie des groupes, plus sa priorité est élevée. La priorité est attribuée uniquement en fonction de la profondeur. Toutes les branches ont la même priorité. Par exemple, le niveau deux du groupe sous la branche Bureaux a une priorité supérieure à celle du niveau un du groupe sous la branche Serveurs.

Dans l’exemple de texte suivant du volet hiérarchie de console de Services de mise à jour, pour un serveur WSUS nommé WSUS-01, groupes d’ordinateurs nommés ordinateurs de bureau et serveur ont été ajoutées à la valeur par défaut **tous les ordinateurs** groupe. Les ordinateurs de bureau et les groupes de serveurs sont au même niveau hiérarchique.

-   **Services de mise à jour**

    -   **WSUS-01**

        -   **mises à jour**

        -   **computers**

            -   **Tous les ordinateurs**

                -   **Ordinateurs non attribués**

                -   **Ordinateurs de bureau**

                    -   **Postes de travail-L1**

                        -   **Desktops-L2**

                -   **Serveurs**

                    -   **Servers-L1**

        -   **Serveurs en aval**

        -   **Synchronisations**

        -   **Rapports**

        -   **Options**

Dans cet exemple, les deux niveaux de groupe sous la branche ordinateurs de bureau (bureaux-L2) a une priorité plus élevée que le niveau d’un groupe sous la branche serveurs (serveurs-L1). En conséquence, pour un ordinateur qui appartient aux deux groupes Bureaux-L2 et Serveurs-L1, toutes les actions pour le groupe Bureaux-L2 ont priorité sur les actions spécifiées pour le groupe Serveurs-L1.

#### <a name="priority-of-install-and-uninstall"></a>Priorité d’installation et de désinstallation
Les actions d’installation remplacent les actions de désinstallation. Les installations obligatoires remplacent les installations facultatives (ces dernières sont uniquement disponibles via l’API et la modification d’une approbation pour une mise à jour à l’aide la console d’administration WSUS supprime toutes les approbations facultatives).

#### <a name="priority-of-deadlines"></a>Priorité des échéances
Les actions qui ont une échéance remplacent celles qui n’en ont pas.  Les actions dont les échéances sont plus proches remplacent celles qui sont plus lointaines.

## <a name="16-plan-wsus-performance-considerations"></a>1.6. Planifier les considérations sur les performances WSUS
Il existe certaines zones que vous devez soigneusement planifier avant de déployer WSUS afin que vous pouvez obtenir des performances optimisées. Les principaux domaines sont les suivants :

-   Configuration du réseau

-   Téléchargement différé

-   Filtres

-   Installation

-   Déploiements de mises à jour importantes

-   Service de transfert intelligent en arrière-plan (BITS)

### <a name="network-setup"></a>Configuration du réseau
Pour optimiser les performances sur les réseaux WSUS, tenez compte des suggestions suivantes :

1.  Configurez les réseaux WSUS dans une topologie Hub and Spoke plutôt que dans une topologie hiérarchique.

2.  Utilisez le tri de masques réseau DNS pour les ordinateurs clients itinérants et configurez les ordinateurs clients itinérants pour obtenir des mises à jour à partir du serveur WSUS local.

### <a name="deferred-download"></a>Téléchargement différé
Vous pouvez approuver des mises à jour et télécharger les métadonnées de mise à jour avant de télécharger les fichiers de mise à jour : cette méthode est appelée *téléchargement différé*. Lorsque vous différez des téléchargements, une mise à jour n’est téléchargée qu’après avoir été approuvée. Nous vous conseillons de différer les téléchargements, car cela permet d’optimiser l’espace disque et la bande passante du réseau.

Dans une hiérarchie de serveurs WSUS, WSUS configure automatiquement tous les serveurs en aval pour qu’ils utilisent le paramètre de téléchargement différé du serveur WSUS racine. Vous pouvez modifier ce paramètre par défaut. Par exemple, vous pouvez configurer un serveur en amont pour effectuer des synchronisations immédiates et complètes, puis configurer un serveur en aval pour différer les téléchargements.

Si vous déployez une hiérarchie de serveurs WSUS connectés, nous vous conseillons de ne pas imbriquer profondément les serveurs. Si vous activez les téléchargements différés et un serveur en aval demande une mise à jour qui n’est pas approuvé sur le serveur en amont, la demande du serveur en aval force un téléchargement sur le serveur en amont. Le serveur en aval télécharge ensuite la mise à jour lors d’une synchronisation ultérieure. Dans une hiérarchie profonde de serveurs WSUS, des retards peuvent se produire lorsque les mises à jour dont demandées, téléchargées, puis transmises par la hiérarchie de serveurs. Par défaut, les téléchargements différés sont activés lorsque vous stockez des mises à jour en local. Vous pouvez modifier cette option manuellement.

### <a name="filters"></a>Filtres
WSUS vous permet de filtrer les synchronisations des mises à jour par langue, produit et classification. Dans une hiérarchie de serveurs WSUS, WSUS configure automatiquement tous les serveurs en aval pour qu’ils utilisent les options de filtrage des mises à jour qui sont sélectionnées sur le serveur WSUS racine. Vous pouvez reconfigurer les serveurs de téléchargement pour recevoir uniquement un sous-ensemble des langues.

Par défaut, les produits à mettre à jour sont Windows et Office, et les classifications par défaut sont Mises à jour critiques, Mises à jour de sécurité et Mises à jour de définitions. Pour économiser l’espace disque et la bande passante, nous vous conseillons de vous limiter aux langues que vous utilisez vraiment.

### <a name="installation"></a>Installation
Les mises à jour sont généralement constituées de nouvelles versions de fichiers qui existent déjà sur l’ordinateur qui est mis à jour. Sur un niveau binaire, il est possible que ces fichiers existants ne soient pas très différents des versions mises à jour. La fonctionnalité des fichiers d’installation rapide identifie les octets exacts entre les versions, crée et distribue des mises à jour de ces différences uniquement, puis fusionne le fichier existant avec les octets mis à jour.

Cette fonctionnalité est parfois appelée « remise delta », car elle télécharge uniquement le delta (différence) entre deux versions d’un fichier. Les fichiers d’installation rapide sont plus volumineux que les mises à jour distribuées aux ordinateurs clients, car ils contiennent toutes les versions possibles de chaque fichier à mettre à jour.

Vous pouvez utiliser les fichiers d’installation rapide pour limiter la bande passante consommée sur le réseau local, étant donné que WSUS transmet uniquement le delta applicable à une version particulière d’un composant mis à jour. Toutefois, cela se fait au prix de la bande passante supplémentaire entre votre serveur WSUS, les serveurs WSUS en amont et Microsoft Update, et nécessite de l’espace disque supplémentaire. Par défaut, WSUS n’utilise pas les fichiers d’installation rapide.

Les mises à jour ne sont pas toutes appropriées pour une distribution à l’aide de fichiers d’installation rapide. Si vous sélectionnez cette option, vous obtenez des fichiers d’installation rapide pour toutes les mises à jour. Si vous ne stockez pas les mises à jour localement, l’agent Windows Update décidera de télécharger les fichiers d’installation rapide ou les distributions de mise à jour du fichier complet.

### <a name="large-update-deployment"></a>Déploiements de mises à jour importantes
Lorsque vous déployez des mises à jour importantes (par exemple, des Service Packs), vous pouvez éviter de saturer le réseau en utilisant les pratiques suivantes :

1.  Utilisez la limitation de bande passante du service de transfert intelligent en arrière-plan (BITS). Les limitations de bande passante du service BITS peuvent être contrôlées selon le créneau horaire, mais elles s’appliquent à toutes les applications qui utilisent le service BITS. Pour savoir comment contrôler la limitation BITS, consultez [stratégies de groupe](https://msdn.microsoft.com/library/windows/desktop/aa362844(v=vs.85).aspx).

2.  Utilisez la limitation de bande passante des services Internet Information Services pour limiter la bande passante à un ou plusieurs services Web.

3.  Utilisez des groupes d’ordinateurs pour contrôler le lancement. Un ordinateur client s’identifie en tant que membre d’un groupe d’ordinateurs particulier lorsqu’il envoie des informations au serveur WSUS. Le serveur WSUS utilise ces informations pour déterminer les mises à jour à déployer sur cet ordinateur. Vous pouvez configurer plusieurs groupes d’ordinateurs et approuver de manière séquentielle les téléchargements de Service Packs volumineux pour un sous-ensemble de ces groupes.

### <a name="background-intelligent-transfer-service"></a>Service de transfert intelligent en arrière-plan (BITS)
WSUS utilise le protocole du service de transfert intelligent en arrière-plan (BITS) pour toutes ses tâches de transfert de fichiers. Cela comprend les téléchargements vers les ordinateurs clients et les synchronisations du serveur. Le service BITS permet aux programmes de télécharger des fichiers en utilisant la bande passante de rechange. Le service BITS gère les transferts de fichiers par le biais de déconnexions du réseau et de redémarrages de l’ordinateur. Pour plus d'informations, consultez : [Service de transfert intelligent en arrière-plan (BITS)](https://msdn.microsoft.com/library/bb968799.aspx).

## <a name="17-plan-automatic-updates-settings"></a>1.7. Planifier les paramètres des Mises à jour automatiques
Vous pouvez spécifier une date limite pour approuver les mises à jour sur le serveur WSUS. La date limite oblige les ordinateurs clients à installer la mise à jour à une heure spécifique, mais il existe différentes situations, selon si la date limite a expiré, si la file d’attente comprend d’autres mises à jour que l’ordinateur doit installer et si la mise à jour (ou une autre mise à jour de la file d’attente) requiert un redémarrage.

Par défaut, les Mises à jour automatiques interrogent le serveur WSUS au sujet des mises à jour approuvées toutes les 22 heures moins un décalage aléatoire. Si de nouvelles mises à jour doivent être installées, elles sont téléchargées. Vous pouvez définir une valeur comprise entre 1 et 22 heures pour le délai entre chaque cycle de détection.

Vous pouvez manipuler les options de notification comme suit :

1.  Si les Mises à jour automatiques sont configurées pour avertir l’utilisateur que des mises à jour sont prêtes à être installées, la notification est envoyée au journal système et à la zone de notification de l’ordinateur client.

2.  Lorsqu’un utilisateur avec les informations d’identification appropriées clique sur l’icône de la zone de notification, les Mises à jour automatiques affichent les mises à jour disponibles à installer. L’utilisateur doit cliquer sur **Installer** pour démarrer l’installation. Un message s’affiche si l’installation de la mise à jour nécessite un redémarrage de l’ordinateur pour être complète. Si un redémarrage est demandé, les Mises à jour automatiques ne peuvent pas détecter d’autres mises à jour tant que l’ordinateur n’est pas redémarré.

Si les Mises à jour automatiques sont configurées pour installer les mises à jour selon une planification, les mises à jour applicables sont téléchargées et marquées comme prêtes à être installées. Les Mises à jour automatiques avertissent les utilisateurs qui disposent des informations d’identification appropriées en utilisant une icône de la zone de notification, et un événement est consigné dans le journal système.

Au jour et à l’heure planifiés, les Mises à jour automatiques installent la mise à jour et redémarrent l’ordinateur (si nécessaire), même si aucun administrateur local n’a ouvert de session. Si un administrateur local a ouvert une session et que l’ordinateur requiert un redémarrage, les Mises à jour automatiques affichent un avertissement et un compte à rebours pour le redémarrage. Sinon, l’installation se déroule en arrière-plan.

Si l’ordinateur doit être redémarré et qu’un utilisateur a ouvert une session, une boîte de dialogue de compte à rebours similaire s’affiche, qui avertit l’utilisateur au sujet du redémarrage imminent. Vous pouvez manipuler les redémarrages de l’ordinateur avec une stratégie de groupe.

Une fois les nouvelles mises à jour téléchargées, les Mises à jour automatiques interrogent le serveur WSUS au sujet de la liste des packages approuvés pour confirmer que les packages téléchargés sont toujours valides et approuvés. Cela signifie que, si un administrateur WSUS supprime des mises à jour de la liste des mises à jour approuvées alors que les Mises à jour automatiques téléchargent les mises à jour, seules celles qui sont toujours approuvées sont en réalité installées.

