---
title: Configurer et gérer le quorum dans un cluster de basculement
description: Obtenir des informations détaillées sur la façon de gérer le quorum de cluster dans un cluster de basculement Windows Server.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.openlocfilehash: bf854418e9efb7dbb5bd07ba86f29d84ba54d68a
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280377"
---
# <a name="configure-and-manage-quorum"></a>Configurer et gérer le quorum

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit des informations et des étapes pour configurer et gérer le quorum dans un cluster de basculement Windows Server.

## <a name="understanding-quorum"></a>Quorum de présentation

Le quorum d'un cluster est déterminé par le nombre d'éléments votants qui doivent faire partie des membres actifs du cluster pour permettre à ce dernier de démarrer correctement ou de continuer à s'exécuter. Pour obtenir une explication plus détaillée, consultez le [doc de quorum de cluster et pool compréhension](../storage/storage-spaces/understand-quorum.md).

## <a name="quorum-configuration-options"></a>Options de configuration de quorum

Le modèle de quorum dans Windows Server est flexible. Si vous avez besoin modifier la configuration de quorum pour votre cluster, vous pouvez utiliser l’Assistant Configuration de Quorum de Cluster ou les applets de commande de basculement Windows PowerShell. Vous trouverez les procédures et les éléments en prendre en compte pour la configuration du quorum dans la section [Configurer le quorum d'un cluster](#configure-the-cluster-quorum) plus loin dans cette rubrique.

Le tableau suivant présente les trois options de configuration de quorum disponibles dans l'Assistant Configuration de quorum du cluster.

| Option  |Description  |
| --------- | ---------|
| Utiliser les paramètres standard     |  Le cluster attribue automatiquement un vote à chaque nœud et gère de façon dynamique les votes des nœuds. Si cette option est adaptée à votre cluster et si un stockage partagé de cluster est disponible, le cluster sélectionne un témoin de disque. Cette option est recommandée dans la plupart des cas, car le logiciel de cluster choisit automatiquement une configuration de quorum et de témoin qui assure à votre cluster le plus haut niveau de disponibilité.       |
| Ajouter ou changer le témoin de quorum     |   Vous pouvez ajouter, modifier ou supprimer une ressource de témoin. Vous pouvez configurer un partage de fichiers ou un témoin de disque. Le cluster attribue automatiquement un vote à chaque nœud et gère de façon dynamique les votes des nœuds.      |
| Configuration de quorum et sélection de témoin avancées     | Vous ne devez sélectionner cette option que si la configuration du quorum est assortie de contraintes liées à l'application ou au site. Vous pouvez modifier le témoin du quorum, ajouter ou retirer des votes aux nœuds et choisir si le cluster doit gérer les votes des nœuds de façon dynamique. Par défaut, les votes sont attribués à tous les nœuds, et les votes des nœuds sont gérés de façon dynamique.        |

Selon l'option de configuration de quorum que vous choisissez et vos paramètres spécifiques, le cluster est configuré dans l'un des modes de quorum suivants :

| Mode  | Description  |
| --------- | ---------|
| Nœud majoritaire (sans témoin)     |   Seuls les nœuds disposent d'un vote. Aucun témoin de quorum n'est configuré. Le quorum de cluster correspond à la majorité des nœuds votants parmi les membres actifs du cluster.      |
| Nœud majoritaire avec témoin (disque ou partage de fichiers)     |   Les nœuds disposent d'un vote. Par ailleurs, un témoin de quorum dispose d'un vote. Le quorum de cluster correspond à la majorité des nœuds votants parmi les membres actifs du cluster, plus le vote d'un témoin. Un témoin de quorum peut être un témoin de disque désigné ou un témoin de partage de fichiers désigné. 
| Pas de majorité (témoin de disque uniquement)     | Aucun nœud ne dispose d'un vote. Seul un témoin de disque dispose d'un vote. <br>Le quorum du cluster est déterminé par l'état du témoin de disque. En général, ce mode n'est pas recommandé et il est préférable de ne pas le sélectionner, car il créé un point de défaillance unique pour le cluster.       |

Les sous-sections suivantes vous donnera plus d’informations sur les paramètres de configuration de quorum avancée.

### <a name="witness-configuration"></a>Configuration d'un témoin

En règle générale, quand vous configurez un témoin, les éléments votants du cluster doivent être en nombre impair. Par conséquent, si le cluster compte un nombre pair de nœuds votants, vous devez configurer un témoin de disque ou un témoin de partage de fichiers. Le cluster pourra ainsi supporter la défaillance d'un nœud supplémentaire. De plus, en ajoutant le vote d'un témoin, le cluster pourra continuer à s'exécuter si la moitié des nœuds du cluster subissent simultanément une défaillance ou sont déconnectés.

Si tous les nœuds peuvent voir le disque, il est généralement conseillé d'utiliser un témoin de disque. Vous avez tout intérêt à utiliser un témoin de partage de fichiers dans l'optique d'une récupération d'urgence multisite avec un stockage répliqué. La configuration d'un témoin de disque avec un stockage répliqué n'est possible que si le fournisseur de stockage prend en charge l'accès en lecture-écriture au stockage répliqué à partir de tous les sites. <strong>*Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct*</strong>.

Le tableau suivant fournit des informations et des considérations supplémentaires sur les types de témoin de quorum.

| Type de témoin  | Description  | Conditions requises et recommandations  |
| ---------    |---------        |---------                        |
| Témoin de disque     |  <ul><li> Numéro d'unité logique dédié stockant une copie de la base de données de cluster</li><li> Particulièrement utile aux clusters dotés d'un stockage partagé (non répliqué)</li>       |  <ul><li>Le numéro d'unité logique doit avoir une taille minimale de 512 Mo</li><li> Son utilisation doit être dédiée au cluster et il ne doit pas être affecté à un rôle en cluster</li><li> Doit être inclus dans un stockage en cluster et réussir les tests de validation de stockage</li><li> Disque différent d'un volume partagé de cluster</li><li> Disque de base doté d'un seul volume</li><li> Ne nécessite pas de lettre de lecteur</li><li> Peut être formaté en NTFS ou ReFS</li><li> Peut éventuellement être configuré avec un RAID matériel à des fins de tolérance de panne</li><li> Doit être exclu des sauvegardes et des analyses antivirus</li><li> Un témoin de disque n’est pas pris en charge avec les espaces de stockage Direct</li>|
| Témoin de partage de fichiers     | <ul><li>Partage de fichiers SMB configuré sur un serveur de fichiers exécutant Windows Server</li><li> Ne stocke pas de copie de la base de données de cluster</li><li> Consigne les informations de cluster dans un fichier witness.log uniquement</li><li> Essentiellement utile pour les clusters multisites dotés d'un stockage répliqué </li>       |  <ul><li>Doit disposer au minimum de 5 Mo d'espace libre</li><li> Doit être dédié au seul cluster et ne pas être utilisé pour stocker des données d'utilisateurs ou d'applications</li><li> Doit avoir les autorisations en écriture activées pour l'objet ordinateur correspondant au nom du cluster</li></ul><br>Les autres considérations qui suivent s'appliquent à un serveur de fichiers hébergeant le témoin de partage de fichiers :<ul><li>Il est possible de configurer un seul serveur de fichiers avec des témoins de partage de fichiers pour plusieurs clusters.</li><li> Le serveur de fichiers doit se trouver sur un site distinct de la charge de travail de cluster. Chaque site de cluster dispose ainsi des mêmes chances de survie en cas de perte de communication réseau entre les sites. Si le serveur de fichiers sur trouve sur le même site, ce dernier devient le site principal et il est le seul à pouvoir accéder au partage de fichiers.</li><li> Le serveur de fichiers peut s'exécuter sur un ordinateur virtuel si celui-ci n'est pas hébergé sur le cluster qui utilise le témoin de partage de fichiers.</li><li> Pour bénéficier d'un haut niveau de disponibilité, le serveur de fichiers peut être configuré sur un cluster de basculement distinct. </li>      |
| Témoin de cloud     |  <ul><li>Un fichier de témoin stocké dans le stockage blob Azure</li><li> Recommandé lorsque tous les serveurs du cluster possèdent une connexion Internet fiable.</li>      |  Consultez [déployer un témoin cloud](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).       |

### <a name="node-vote-assignment"></a>Attribution de votes aux nœuds

Comme une option de configuration de quorum avancée, vous pouvez choisir attribuer ou retirer des votes de quorum de nœud par nœud. Par défaut, un vote est attribué à tous les nœuds. Indépendamment de l'attribution de votes, tous les nœuds continuent de fonctionner dans le cluster, reçoivent des mises à jour de base de données de cluster et peuvent héberger des applications.

Vous pouvez retirer les votes aux nœuds dans certaines configurations de récupération d'urgence. Par exemple, dans un cluster multisite, vous pouvez retirer les votes aux nœuds sur un site de sauvegarde pour éviter que ces nœuds affectent les calculs de quorum. Cette configuration est recommandée uniquement dans le cas d'un basculement manuel entre sites. Pour plus d'informations, voir [Éléments de quorum à prendre en considération pour les configurations de récupération d'urgence](#quorum-considerations-for-disaster-recovery-configurations) plus loin dans cette rubrique.

Le vote configuré d’un nœud peut être vérifié en examinant le **NodeWeight** propriété commune du nœud de cluster à l’aide de la [Get-ClusterNode](https://technet.microsoft.com/library/hh847268.aspx)applet de commande Windows PowerShell. La valeur 0 indique qu'aucun vote de quorum n'est configuré pour le nœud. La valeur 1 indique que le vote de quorum du nœud est attribué et qu'il est géré par le cluster. Pour plus d'informations sur la gestion des votes des nœuds, voir [Gestion de quorum dynamique](#dynamic-quorum-management) plus loin dans cette rubrique.

L'attribution de votes peut être vérifiée pour tous les nœuds de cluster via le test de validation **Valider le quorum du cluster** .

#### <a name="additional-considerations-for-node-vote-assignment"></a>Considérations supplémentaires pour l’attribution de votes de nœud

  - Il n'est pas recommandé d'attribuer le vote aux nœuds pour obtenir un nombre de nœuds votants impair. Il est préférable de configurer un témoin de disque ou un témoin de partage de fichiers. Pour plus d’informations, consultez [configuration d’un témoin](#witness-configuration) plus loin dans cette rubrique.
  - Si la gestion de quorum dynamique est activée, seuls les nœuds configurés pour disposer d'un vote peuvent faire l'objet d'une attribution ou d'un retrait dynamique de leur vote. Pour plus d'informations, voir [Gestion de quorum dynamique](#dynamic-quorum-management) plus loin dans cette rubrique.

### <a name="dynamic-quorum-management"></a>Gestion de quorum dynamique

Dans Windows Server 2012, comme une option de configuration de quorum avancée, vous pouvez choisir activer la gestion de quorum dynamique par cluster. Pour plus d’informations sur le fonctionnement du quorum dynamique, consultez [cette explication](../storage/storage-spaces/understand-quorum.md#dynamic-quorum-behavior).

Avec la gestion de quorum dynamique, un cluster peut aussi s'exécuter sur le dernier nœud de cluster survivant. En ajustant de façon dynamique les critères de majorité de quorum, le cluster peut supporter des arrêts de nœuds successifs jusqu'à ce qu'il n'en reste qu'un seul.

Le vote dynamique attribué en cluster un nœud peut être vérifié avec le **DynamicWeight** propriété commune du nœud de cluster à l’aide de la [Get-ClusterNode](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusternode?view=win10-ps) applet de commande Windows PowerShell. La valeur 0 indique que le nœud ne dispose pas d'un vote de quorum. La valeur 1 indique que le nœud dispose d'un vote de quorum.

L'attribution de votes peut être vérifiée pour tous les nœuds de cluster via le test de validation **Valider le quorum du cluster** .

#### <a name="additional-considerations-for-dynamic-quorum-management"></a>Considérations supplémentaires pour la gestion de quorum dynamique

- La gestion de quorum dynamique ne permet pas au cluster de supporter la défaillance simultanée d'une majorité de membres votants. Pour continuer à s'exécuter, le cluster doit toujours avoir une majorité de quorum au moment où l'arrêt ou la défaillance de nœud intervient.

- Si vous avez retiré explicitement le vote à un nœud, le cluster ne peut pas l'ajouter ou la retirer de façon dynamique.
- Espaces de stockage Direct est activé, le cluster peut uniquement prendre en charge deux défaillances de nœud. Cela est expliqué plus dans la [section de quorum de pool](../storage/storage-spaces/understand-quorum.md)

## <a name="general-recommendations-for-quorum-configuration"></a>Recommandations générales concernant la configuration d'un quorum

Le logiciel de cluster configure automatiquement le quorum pour un nouveau cluster en fonction du nombre de nœuds configuré et de la disponibilité du stockage partagé. Il s'agit habituellement de la configuration de quorum qui convient le mieux à ce cluster. Cependant, il est judicieux d'examiner la configuration du quorum après avoir créé le cluster et avant de mettre ce dernier en production. Pour afficher la configuration de quorum de cluster détaillées, vous pouvez utiliser l’Assistant Validation d’une Configuration ou le [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps) applet de commande Windows PowerShell pour exécuter le **valider la Configuration de Quorum** tester. Dans le Gestionnaire de Cluster de basculement, la configuration de quorum de base s’affiche dans les informations récapitulatives du cluster sélectionné, ou vous pouvez consulter les informations sur les ressources de quorum renvoyées lorsque vous exécutez le [Get-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/get-clusterquorum?view=win10-ps) Applet de commande Windows PowerShell.

À tout moment, vous pouvez exécuter le test **Valider la configuration de quorum** pour vérifier que la configuration du quorum est optimale pour votre cluster. Les résultats du test indiquent s'il est recommandé de modifier la configuration du quorum et présente les paramètres optimaux. Si une modification est recommandée, vous pouvez utiliser l'Assistant Configuration de quorum du cluster pour appliquer les paramètres recommandés.

Après avoir mis le cluster en production, évitez de modifier la configuration du quorum tant que vous n'avez pas établi que la modification est judicieuse pour votre cluster. Vous pouvez envisager de modifier la configuration du quorum dans les cas suivants :

- ajout ou retrait de nœuds ;
- ajout ou suppression de stockage ;
- défaillance de longue durée d'un nœud ou d'un témoin ;
- récupération d'un cluster dans un scénario de récupération d'urgence multisite.

Pour plus d'informations sur la validation d'un cluster de basculement, voir [Valider le matériel pour un cluster de basculement](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="configure-the-cluster-quorum"></a>Configurer le quorum d'un cluster

Vous pouvez configurer les paramètres de quorum de cluster à l’aide de gestionnaire du Cluster de basculement ou les applets de commande de basculement Windows PowerShell.

> [!IMPORTANT]
> Il est généralement préférable d'utiliser la configuration de quorum recommandée par l'Assistant Configuration de quorum du cluster. Nous vous conseillons de personnaliser la configuration du quorum seulement si vous avez établi que cette modification est judicieuse pour votre cluster. Pour plus d'informations, consultez [Recommandations générales concernant la configuration d'un quorum](#general-recommendations-for-quorum-configuration) dans cette rubrique.

### <a name="configure-the-cluster-quorum-settings"></a>Configurer les paramètres de quorum d'un cluster

L'appartenance au groupe **Administrateurs** local sur chaque serveur en cluster, ou l'équivalent, constitue l'autorisation minimale requise pour pouvoir effectuer cette procédure. De même, le compte que vous utilisez doit être un compte d'utilisateur de domaine.

> [!NOTE]
> Vous pouvez modifier la configuration de quorum du cluster sans arrêter le cluster ni mettre les ressources du cluster hors connexion.

### <a name="change-the-quorum-configuration-in-a-failover-cluster-by-using-failover-cluster-manager"></a>Modifier la configuration de quorum dans un cluster de basculement à l’aide du Gestionnaire du Cluster de basculement

1. Dans le Gestionnaire du cluster de basculement, sélectionnez ou spécifiez le cluster que vous voulez modifier.
2. Avec le cluster sélectionné, sous **Actions**, sélectionnez **autres Actions**, puis sélectionnez **configurer les paramètres de Cluster Quorum**. L'Assistant Configuration de quorum du cluster s'affiche à l'écran. Sélectionnez **Suivant**.
3. Dans la page **Sélectionner l'option de configuration du quorum**, sélectionnez l'une des trois options de configuration et effectuez les étapes correspondant à cette option. Avant de configurer les paramètres de quorum, vous pouvez passer en revue vos choix. Pour plus d’informations sur les options, consultez [quorum de présentation](#understanding-quorum), plus haut dans cette rubrique.

    - Pour permettre au cluster de réinitialiser automatiquement les paramètres de quorum optimaux pour votre configuration actuelle du cluster, sélectionnez **utiliser les paramètres standard** , puis terminez l’Assistant.
    - Pour ajouter ou modifier le témoin de quorum, sélectionnez **ajouter ou modifier le témoin de quorum**, puis terminez les étapes suivantes. Pour obtenir des informations et des considérations sur la configuration d'un témoin de quorum, voir [Configuration d'un témoin](#witness-configuration) plus haut dans cette rubrique.

      1. Dans la page **Sélectionner le témoin de quorum** , sélectionnez une option pour configurer un témoin de disque ou un témoin de partage de fichiers. L'Assistant indique les options de sélection de témoin recommandées compte tenu de votre cluster.

          > [!NOTE]
          > Vous pouvez aussi sélectionner **Ne pas configurer de témoin de quorum** , puis complétez l'Assistant. Si vous avez un nom pair de nœuds votants dans votre cluster, il ne s'agit peut-être pas de la configuration recommandée.

      2. Si vous sélectionnez l'option pour configurer un témoin de disque, dans la page **Configurer le témoin de stockage** , sélectionnez le volume de stockage que vous voulez affecter en tant que témoin de disque, puis complétez l'Assistant.
      3. Si vous sélectionnez l'option pour configurer un témoin de partage de fichiers, dans la page **Configurer le témoin de partage de fichiers** , entrez ou localisez l'emplacement du partage de fichiers qui fera office de ressource de témoin, plus complétez l'Assistant.

    - Pour configurer les paramètres de gestion de quorum et pour ajouter ou modifier le témoin de quorum, sélectionnez **avancés de sélection de configuration et le témoin de quorum**, puis terminez les étapes suivantes. Pour obtenir des informations et des recommandations sur les paramètres de configuration de quorum avancés, consultez [Attribution de votes aux nœuds](#node-vote-assignment) et [Gestion de quorum dynamique](#dynamic-quorum-management) plus haut dans cette rubrique.

      1. Dans la page **Sélectionner la configuration de vote**, sélectionner une option pour attribuer des votes aux nœuds. Par défaut, tous les nœuds bénéficient d'un vote. Cependant, pour certains scénarios, vous pouvez attribuer des votes seulement à un sous-ensemble de nœuds.

          > [!NOTE]
          > Vous pouvez aussi sélectionner **Aucun nœud**. Cette option n'est pas recommandée, car les nœuds ne peuvent pas participer au vote du quorum et cela impose de configurer un témoin de disque. Ce témoin de disque devient le seul point de défaillance du cluster.

      2. Dans la page **Configurer la gestion du quorum** , vous pouvez activer ou désactiver l'option **Autoriser le cluster à gérer dynamiquement l'attribution de votes de nœud** . La sélection de cette option a généralement pour effet d'accroître la disponibilité du cluster. Cette option est activée par défaut et il est fortement déconseillé de la désactiver. Cette option permet au cluster de continuer à s'exécuter en cas de défaillance, ce qui n'est pas possible quand cette option est désactivée.
      3. Dans la page **Sélectionner le témoin de quorum** , sélectionnez une option pour configurer un témoin de disque ou un témoin de partage de fichiers. L'Assistant indique les options de sélection de témoin recommandées compte tenu de votre cluster.

          > [!NOTE]
          > Vous pouvez aussi sélectionner **Ne pas configurer de témoin de quorum**, puis complétez l'Assistant. Si vous avez un nom pair de nœuds votants dans votre cluster, il ne s'agit peut-être pas de la configuration recommandée.

      4. Si vous sélectionnez l'option pour configurer un témoin de disque, dans la page **Configurer le témoin de stockage** , sélectionnez le volume de stockage que vous voulez affecter en tant que témoin de disque, puis complétez l'Assistant.
      5. Si vous sélectionnez l'option pour configurer un témoin de partage de fichiers, dans la page **Configurer le témoin de partage de fichiers** , entrez ou localisez l'emplacement du partage de fichiers qui fera office de ressource de témoin, plus complétez l'Assistant.

4. Sélectionnez **Suivant**. Confirmez vos sélections dans la page de confirmation qui s’affiche, puis sélectionnez **suivant**.

Une fois que l’Assistant s’exécute et le **Résumé** page s’affiche, si vous souhaitez afficher un rapport des tâches effectuées par l’Assistant, sélectionnez **afficher le rapport**. Le rapport plus récent est conservé dans le <em>systemroot</em> **\\Cluster\\rapports** dossier portant le nom **QuorumConfiguration.mht**.

> [!NOTE]
> Après avoir configuré le quorum de cluster, nous vous recommandons d'exécuter le test **Valider la configuration de quorum** pour vérifier les paramètres de quorum mis à jour.

### <a name="windows-powershell-equivalent-commands"></a>Commandes Windows PowerShell équivalentes

Les exemples suivants montrent comment utiliser le [Set-ClusterQuorum](https://docs.microsoft.com/powershell/module/failoverclusters/set-clusterquorum?view=win10-ps) applet de commande et d’autres applets de commande Windows PowerShell pour configurer le quorum de cluster.

L'exemple suivant modifie la configuration de quorum sur le cluster *CONTOSO-FC1* pour adopter une configuration de nœud majoritaire simple sans témoin de quorum.

```PowerShell
Set-ClusterQuorum –Cluster CONTOSO-FC1 -NodeMajority
```

L'exemple suivant modifie la configuration de quorum sur le cluster local pour adopter une configuration de nœud majoritaire avec témoin. La ressource de disque nommée *Cluster Disk 2* est configurée en tant que témoin de disque.

```PowerShell
Set-ClusterQuorum -NodeAndDiskMajority "Cluster Disk 2"
```

L'exemple suivant modifie la configuration de quorum sur le cluster local pour adopter une configuration de nœud majoritaire avec témoin. La ressource de partage de fichiers nommée  *\\ \\CONTOSO-FS\\fsw* est configuré comme un témoin de partage de fichiers.

```PowerShell
Set-ClusterQuorum -NodeAndFileShareMajority "\\fileserver\fsw"
```

L'exemple suivant retire le vote de quorum au nœud *ContosoFCNode1* du cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=0
```

L'exemple suivant ajoute le vote de quorum au nœud *ContosoFCNode1* du cluster local.

```PowerShell
(Get-ClusterNode ContosoFCNode1).NodeWeight=1
```

L’exemple suivant active la propriété **DynamicQuorum** du cluster *CONTOSO-FC1* (si elle a été auparavant désactivée) :

```PowerShell
(Get-Cluster CONTOSO-FC1).DynamicQuorum=1
```

## <a name="recover-a-cluster-by-starting-without-quorum"></a>Récupérer un cluster en démarrant sans quorum

Un cluster qui n'a pas suffisamment de votes de quorum ne peut pas démarrer. La première mesure à prendre dans ce cas consiste invariablement à vérifier la configuration de quorum du cluster et de chercher à comprendre pourquoi le cluster n'a plus le quorum. Cela peut être imputable à des nœuds qui ont cessé de répondre ou au site principal qui n'est plus accessible dans un cluster multisite. Après avoir identifié la cause originelle de la défaillance du cluster, vous pouvez suivre la procédure de récupération décrite dans cette section.

> [!NOTE]
> * Si le service de cluster s'arrête pour cause de perte de quorum, l'ID d'événement 1177 s'inscrit dans le journal système.
> * Il est toujours nécessaire de rechercher la cause de la perte de quorum du cluster.
> * Il est toujours préférable de rétablir l'intégrité d'un nœud ou d'un témoin de quorum (c'est-à-dire, le réintégrer dans le cluster) plutôt que de démarrer le cluster sans quorum.

### <a name="force-start-cluster-nodes"></a>Forcer le démarrage de nœuds de cluster

Dès lors que vous avez déterminé ne pas être en mesure de récupérer le cluster en rétablissant l'intégrité des nœuds ou du témoin de quorum, vous n'avez pas d'autre alternative que de forcer le démarrage de votre cluster. En se faisant, vous passez outre les paramètres de configuration de quorum du cluster et démarrez le cluster en mode **ForceQuorum** .

Forcer le démarrage d'un cluster qui n'a pas le quorum peut s'avérer particulièrement utile dans un cluster multisite. Prenons l'exemple d'un scénario de récupération d'urgence d'un cluster constitué d'un site principal et d'un site de sauvegarde tous deux séparés géographiquement, *SiteA* et *SiteB*. Si *SiteA*subit une véritable défaillance, remettre le site en ligne risque de prendre beaucoup de temps. Il faudrait probablement forcer la mise en ligne de *SiteB*, même si le site n'a pas le quorum.

Quand un cluster est démarré en mode **ForceQuorum**, il quitte automatiquement cet état forcé pour reprendre un comportement normal dès lors qu'il a recueilli suffisamment de votes de quorum. Il n'est donc pas nécessaire de redémarrer le cluster selon la procédure normale. Si le cluster perd un nœud et par la même occasion le quorum, il se remet hors connexion, car il n'est plus à l'état forcé. Pour mettre en ligne lorsqu’il n’a pas de quorum nécessaire de le forcer le cluster à démarrer sans quorum.

> [!IMPORTANT]
> * L'administrateur dispose d'un contrôle total sur le cluster après que celui-ci a été démarré de force.
> * Le cluster utilise la configuration de cluster du nœud à partir duquel le cluster a été démarré de force et la réplique sur tous les autres nœuds disponibles.
> * Si vous forcez le cluster à démarrer sans quorum, tous les paramètres de configuration de quorum sont ignorés tant que le cluster reste en mode **ForceQuorum**. Cela inclut certaines attributions de votes aux nœuds et les paramètres de gestion de quorum dynamique.

### <a name="prevent-quorum-on-remaining-cluster-nodes"></a>Empêcher le quorum sur les nœuds de cluster restants

Après avoir forcé le démarrage du cluster sur un nœud, il est nécessaire de démarrer les nœuds restants du cluster avec un paramètre qui empêche le quorum. Un nœud démarré avec un paramètre qui empêche le quorum indique au service de cluster de se joindre à un cluster opérationnel existant au lieu de créer une nouvelle instance de cluster. Cela empêche les nœuds restants de créer un cluster fractionné contenant deux instances concurrentes.

Cela devient nécessaire dans certains scénarios de récupération d'urgence multisite qui imposent de récupérer le cluster après que celui-ci a été démarré de force sur le site de sauvegarde, en l'occurrence, *SiteB*. Pour se joindre au cluster démarré de force dans *SiteB*, les nœuds du site principal *SiteA*doivent être démarrés en empêchant le quorum.

> [!IMPORTANT]
> Du moment où vous forcez le démarrage d'un cluster, nous vous recommandons de toujours démarrer les nœuds restants en empêchant le quorum.

Voici comment récupérer le Gestionnaire du Cluster avec basculement de cluster :

1. Dans le Gestionnaire du cluster de basculement, sélectionnez ou spécifiez le cluster que vous voulez récupérer.
2. Avec le cluster sélectionné, sous **Actions**, sélectionnez **forcer le démarrage du Cluster**.

    Le Gestionnaire du cluster de basculement force le démarrage du cluster sur tous les nœuds accessibles. Lors du démarrage, le cluster utilise la configuration de cluster active.

> [!NOTE]
> * Pour forcer le cluster à démarrer sur un nœud spécifique contenant une configuration de cluster que vous souhaitez utiliser, vous devez utiliser les applets de commande Windows PowerShell ou des outils de ligne de commande équivalents comme présentés à la suite de cette procédure. 
> * Si vous utilisez le Gestionnaire du cluster de basculement pour vous connecter à un cluster démarré de force et que vous utilisez l'action **Démarrer le service de cluster** pour démarrer un nœud, celui-ci démarre automatiquement avec le paramètre qui empêche le quorum.

#### <a name="windows-powershell-equivalent-commands-start-clusternode"></a>Commandes équivalentes de Windows PowerShell (Start-Clusternode)

L'exemple suivant montre comment utiliser l'applet de commande **Start-ClusterNode** pour forcer le démarrage du cluster sur le nœud *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –FQ
```

Vous pouvez aussi taper la commande suivante en local sur le nœud :

```PowerShell
Net Start ClusSvc /FQ
```

L'exemple suivant montre comment utiliser l'applet de commande **Start-ClusterNode** pour démarrer le service de cluster en empêchant le quorum sur le nœud *ContosoFCNode1*.

```PowerShell
Start-ClusterNode –Node ContosoFCNode1 –PQ
```

Vous pouvez aussi taper la commande suivante en local sur le nœud :

```PowerShell
Net Start ClusSvc /PQ
```

## <a name="quorum-considerations-for-disaster-recovery-configurations"></a>Éléments de quorum à prendre en considération pour les configurations de récupération d'urgence

Cette section synthétise les caractéristiques et les configurations de quorum pour deux configurations de cluster multisite dans des déploiements de récupération d'urgence. Les consignes de configuration de quorum varient selon que vous avez besoin d'un basculement automatique ou manuel pour les charges de travail entre sites. Votre configuration dépend généralement des contrats de niveau de service (SLA) en vigueur dans votre organisation pour fournir et prendre en charge les charges de travail en cluster en cas de défaillance ou de sinistre sur un site.

### <a name="automatic-failover"></a>Basculement automatique

Dans cette configuration, le cluster se compose de plusieurs sites capables d'héberger des rôles en cluster. Si une défaillance se produit sur un site, les rôles en cluster doivent basculer automatiquement sur les sites restants. Par conséquent, le quorum de cluster doit être configuré de sorte que n'importe quel site puisse supporter une défaillance de site complète.

Le tableau suivant résume les éléments à prendre en considération et les recommandations pour cette configuration.

| Élément  | Description  |
| ---------| ---------|
| Nombre de votes de nœud par site     | Doit être égal       |
| Attribution de votes aux nœuds     |  Le vote ne doit pas être retiré à des nœuds, car tous les nœuds ont la même importance       |
| Gestion de quorum dynamique     |   Activation souhaitable      |
| Configuration d'un témoin     |  Configuration d'un témoin de partage de fichiers recommandée sur un site distinct des sites de cluster       |
| Charges de travail     |  Possibilité de configurer des charges de travail sur n'importe quel site       |

#### <a name="additional-considerations-for-automatic-failover"></a>Considérations supplémentaires pour le basculement automatique

- Le témoin de partage de fichiers doit être configuré sur un site distinct pour donner à chaque site une même chance de survie. Pour plus d’informations, consultez [Configuration d'un témoin](#witness-configuration) plus haut dans cette rubrique.

### <a name="manual-failover"></a>Basculement manuel

Dans cette configuration, le cluster se compose d'un site principal, *SiteA*, et d'un site de sauvegarde (récupération), *SiteB*. Les rôles en cluster sont hébergés sur *SiteA*. Compte tenu de la configuration de quorum du cluster, si une défaillance se produit sur tous les nœuds de *SiteA*, le cluster cesse de fonctionner. Dans ce scénario, l'administrateur doit faire basculer manuellement les services de cluster sur *SiteB* et prendre des mesures supplémentaires pour récupérer le cluster.

Le tableau suivant résume les éléments à prendre en considération et les recommandations pour cette configuration.

| Élément   |Description  |
| ---------| ---------|
| Nombre de votes de nœud par site     |  <ul><li> Les votes ne doivent pas être retirés aux nœuds du site principal, **SiteA**</li><li>Les votes doivent être retirés aux nœuds du site de sauvegarde, **SiteB**</li><li>Si une panne de longue durée se produit sur **SiteA**, les votes doivent être attribués aux nœuds de **SiteB** pour permettre au site d'atteindre le quorum dans le cadre de la récupération</li>       |
| Gestion de quorum dynamique     |  Activation souhaitable       |
| Configuration d'un témoin     |  <ul><li>Configuration d'un témoin nécessaire si **SiteA** a un nombre de nœuds pair</li><li>Si un témoin est nécessaire, configurez un témoin de partage de fichiers ou un témoin de disque accessible uniquement aux nœuds de **SiteA** (parfois appelé « témoin de disque asymétrique »)</li>       |
| Charges de travail     |  Utilisez des propriétaires favoris pour maintenir les charges de travail en exécution sur les nœuds de **SiteA**       |

#### <a name="additional-considerations-for-manual-failover"></a>Considérations supplémentaires pour le basculement manuel

- Seuls les nœuds de *SiteA* sont initialement configurés avec les votes de quorum. Cela est nécessaire pour éviter que l'état des nœuds de *SiteB* affecte le quorum du cluster.
- La procédure de récupération peut varier selon que *SiteA* supporte une défaillance temporaire ou une défaillance de longue durée.

## <a name="more-information"></a>Informations supplémentaires

* [Clustering de basculement](failover-clustering.md)
* [Applets de commande de basculement Windows PowerShell](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)
* [Cluster de présentation et de Quorum de Pool](../storage/storage-spaces/understand-quorum.md)