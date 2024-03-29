---
title: Présentation de la structure protégée et des machines virtuelles dotées d’une protection maximale
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 579012be66969ffc584b4b4ea021f11acbbdfb78
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855900"
---
# <a name="guarded-fabric-and-shielded-vms-overview"></a>Vue d’ensemble de la structure protégée et des machines virtuelles dotées d’une protection maximale

>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

## <a name="overview-of-the-guarded-fabric"></a>Vue d’ensemble de la structure protégée

Sécurité de la virtualisation est un secteur d’investissement important dans Hyper-V. En plus de protéger les hôtes ou d’autres machines virtuelles contre une machine virtuelle exécutant des logiciels malveillants, nous devons également protéger les machines virtuelles contre un hôte compromis. Il s’agit un danger fondamental pour chaque plate-forme de virtualisation aujourd'hui, qu’il s’agisse d’Hyper-V, VMware ou autre. En termes simples, si une machine virtuelle sort d’une organisation (à des fins malveillantes ou accidentellement), elle peut être exécutée sur n’importe quel autre système. La protection des ressources vitales de votre organisation, comme les contrôleurs de domaine, les serveurs de fichiers sensibles et les systèmes RH, est une priorité majeure.

Pour vous protéger contre la structure de virtualisation compromis, Windows Server 2016 Hyper-V introduit des machines virtuelles protégées. Une machine virtuelle protégée est une génération 2 machines virtuelles (pris en charge sur Windows Server 2012 et versions ultérieures) qui a un module TPM virtuel, est chiffré à l’aide de BitLocker et peuvent s’exécuter uniquement sur des hôtes intègres et approuvés dans l’infrastructure. Les machines virtuelles dotées d’une protection maximale et la structure protégée permettent aux fournisseurs de services cloud ou aux administrateurs du cloud privé d’entreprise d’offrir un environnement plus sécurisé pour les machines virtuelles du locataire. 

Une structure protégée se compose des éléments suivants :

- 1 Service Guardian hôte (SGH) (en général, un cluster de 3 nœuds)
- 1 ou plusieurs hôtes Service Guardian
- Un ensemble de machines virtuelles dotées d’une protection maximale Le diagramme ci-dessous montre comment le Service Guardian hôte utilise une attestation pour garantir que seuls les hôtes valides connus peuvent démarrer les machines virtuelles dotées d’une protection maximale, et la protection de clé pour libérer en toute sécurité les clés pour les machines virtuelles dotées d’une protection maximale.

Quand un locataire crée des machines virtuelles dotées d’une protection maximale qui s’exécutent sur une structure protégée, les hôtes Hyper-V et les machines virtuelles dotées d’une protection maximale elles-mêmes sont protégés par le SGH. Le SGH fournit deux services distincts : l’attestation et la protection de clé. Le service d’attestation garantit uniquement que les hôtes Hyper-V approuvés peuvent exécuter des machines virtuelles dotées d’une protection maximale tandis que le service de protection de clé fournit les clés nécessaires pour les mettre sous tension et les migrer dynamiquement vers d’autres hôtes Service Guardian.

![Structure d’hôte Service Guardian](../media/Guarded-Fabric-Shielded-VM/Guarded-Host-Overview-Diagram.png)

## <a name="video-introduction-to-shielded-virtual-machines"></a>Vidéo : Présentation des machines virtuelles protégées 

<iframe src="https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016/player" width="650" height="440" allowFullScreen frameBorder="0"></iframe>

## <a name="attestation-modes-in-the-guarded-fabric-solution"></a>Modes d’attestation de la solution de structure protégée

Le SGH prend en charge les modes d’attestation différents pour une structure protégée :

- Attestation approuvée par le module de plateforme sécurisée (basé sur matériel)
- Attestation de clé d’hôte (en fonction des paires de clés asymétriques)

L’attestation approuvée par le module de plateforme sécurisée (TPM) est recommandée, car elle offre des garanties renforcées, comme expliqué dans le tableau suivant, mais elle nécessite que TPM 2.0 soit installé sur vos hôtes Hyper-V. Si vous en n’avez pas TPM 2.0 ou n’importe quel module de plateforme sécurisée, vous pouvez utiliser l’attestation de clé hôte. Si vous décidez de passer à l’attestation approuvée par le module de plateforme sécurisée (TPM) quand vous achetez du nouveau matériel, vous pouvez changer de mode d’attestation sur le Service Guardian hôte avec peu ou pas d’interruption sur votre structure.

| **Mode d’attestation choisi pour les hôtes**                                            | **Garanties de l’hôte** |
|-------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**Attestation approuvée par le module de plateforme sécurisée :** Offre les protections possibles plus puissantes, mais nécessite également des étapes de configuration supplémentaires. Matériel hôte et microprogramme doivent inclure TPM 2.0 et UEFI 2.3.1 avec démarrage sécurisé. | Hôtes service Guardian sont approuvées selon leur identité de module de plateforme sécurisée, la séquence de démarrage mesuré et stratégies d’intégrité du code pour vous assurer qu’ils uniquement exécutent du code approuvé.| 
| **Attestation de clé hôte :** Conçu prendre en charge de matériel hôte existant où TPM 2.0 n’est pas disponible. Nécessite moins d’étapes de configuration et est compatible avec le matériel courant. | Hôtes sont approuvés en fonction de la possession de la clé. | 

Un autre mode nommé **attestation approuvée par l’administrateur** est déconseillée à compter de Windows Server 2019. Ce mode était basé sur l’appartenance du service Guardian hôte dans un groupe de sécurité des Services de domaine Active Directory (AD DS) désigné. L’attestation de clé hôte fournissent l’identification de l’hôte similaire et est plus facile à configurer. 

## <a name="assurances-provided-by-the-host-guardian-service"></a>Garanties fournies par le Service Guardian hôte

SGH, ainsi que les méthodes de création de machines virtuelles dotées d’une protection maximale, permettent de fournir les garanties suivantes.

| **Type de garantie pour les machines virtuelles**                         | **Protégées des assurances de machine virtuelle, de Service de Protection de clé et de méthodes de création de machines virtuelles protégées** |
|----------------------------|--------------------------------------------------|
| **Disques (disques de système d’exploitation et disques de données) chiffré avec BitLocker**   | Les machines virtuelles dotées d’une protection maximale utilisent BitLocker pour protéger leurs disques. Les clés BitLocker nécessaires pour démarrer la machine virtuelle et de déchiffrer les disques sont protégées par le module TPM virtuel de machine virtuelle protégée à l’aide de technologies éprouvées telles que le démarrage mesuré sécurisé. Même si les machines virtuelles dotées d’une protection maximale chiffrent et protègent automatiquement le disque du système d’exploitation uniquement, vous pouvez [chiffrer les lecteurs de données](https://technet.microsoft.com/itpro/windows/keep-secure/bitlocker-overview) attachés aux machines virtuelles dotées d’une protection maximale. |
| **Déploiement de nouvelles machines virtuelles protégées à partir de disques/images de modèle « approuvés »** | Lors du déploiement de nouvelles machines virtuelles dotées d’une protection maximale, les locataires peuvent spécifier les disques de modèle qu’ils approuvent. Les disques de modèle protégés ont des signatures qui sont calculées à un instant donné quand leur contenu est jugé digne de confiance. Les signatures de disque sont ensuite stockées dans un catalogue de signatures, que les locataires fournissent de façon sécurisée à la structure lors de la création de machines virtuelles dotées d’une protection maximale. Pendant la mise en service des machines virtuelles dotées d’une protection maximale, la signature du disque est recalculée et comparée aux signatures approuvées du catalogue. Si les signatures correspondent, la machine virtuelle dotée d’une protection maximale est déployée. Si les signatures ne correspondent pas, le disque de modèle protégé est jugé non fiable et le déploiement échoue. |
| **Protection des mots de passe et autres secrets lors de la création d’une machine virtuelle protégée** | Lorsque vous créez des machines virtuelles, il est nécessaire pour vous assurer que les secrets de machine virtuelle, telles que les signatures de disque approuvées, les certificats RDP et le mot de passe du compte d’administrateur local de la machine virtuelle, ne sont pas révélés à l’infrastructure. Ces secrets sont stockés dans un fichier chiffré appelé « fichier de données de protection (fichier .PDK), qui est protégé par des clés de locataire et chargé sur la structure par le locataire. Quand une machine virtuelle dotée d’une protection maximale est créée, le locataire sélectionne les données de protection à utiliser qui fournissent en toute sécurité ces secrets uniquement aux composants approuvés dans la structure protégée. |
| **Contrôle du locataire d’où la machine virtuelle peut être démarrée** | Les données de protection contiennent également une liste de structures protégées sur lesquelles une machine virtuelle dotée d’une protection maximale particulière est autorisée à s’exécuter. Cela est utile, par exemple, dans les cas où une machine virtuelle dotée d’une protection maximale réside dans un cloud privé local, mais doit éventuellement être migrée vers un autre cloud (public ou privé) pour une reprise d’activité. Le cloud ou la structure cible doit prendre en charge les machines virtuelles dotées d’une protection maximale, et celles-ci doivent permettre à cette structure de les exécuter. |

## <a name="what-is-shielding-data-and-why-is-it-necessary"></a>Que sont les données de protection et pourquoi sont-elles nécessaires ?

Un fichier de données de protection (également appelé « fichier de données d’approvisionnement » ou « fichier PDK ») est un fichier chiffré créé par un locataire ou un propriétaire de machine virtuelle pour protéger les informations clés de configuration de la machine virtuelle, telles que le mot de passe de l’administrateur, les certificats RDP et autres certificats liés aux identités, les informations d’identification de jonction de domaine, etc. Un administrateur de structure utilise le fichier de données de protection lors de la création d’une machine virtuelle dotée d’une protection maximale, mais ne peut pas afficher ou utiliser les informations qu’il contient.

Un fichier de données de protection contient des secrets, notamment :

- Les informations d’identification de l’administrateur
- Un fichier de réponses (unattend.xml)
- Une stratégie de sécurité qui détermine si les machines virtuelles créées à l’aide de cette protection données sont configurées comme protégé ou chiffrement pris en charge
    - N’oubliez pas que les machines virtuelles configurées avec une protection maximale ne sont pas accessibles par les administrateurs de structure tandis que les machines virtuelles prises en charge par le chiffrement le sont
- Un certificat RDP pour sécuriser les communications de bureau à distance avec la machine virtuelle
- Un catalogue des signatures de volume qui contient une liste de signatures de disques de modèles signés et approuvés à partir desquelles une nouvelle machine virtuelle est autorisée à être créée
- Un protecteur de clé (ou KP) qui définit les structures protégées sur lesquelles une machine virtuelle dotée d’une protection maximale est autorisée à s’exécuter

Le fichier de données de protection (fichier PDK) garantit que la machine virtuelle est créée de la manière voulue par le locataire. Par exemple, quand le locataire place un fichier de réponses (unattend.xml) dans le fichier de données de protection, puis les remet au fournisseur d’hébergement, le fournisseur d’hébergement ne peut pas afficher ou modifier ce fichier de réponses. De même, le fournisseur d’hébergement ne peut pas remplacer un VHDX différent lors de la création d’une machine virtuelle dotée d’une protection maximale, car le fichier de données de protection contient les signatures des disques approuvés à partir desquelles les machines virtuelles dotées d’une protection maximale peuvent être créées.

L’illustration suivante montre le fichier de données de protection et les éléments de configuration associés.

![Fichier de données de protection](../media/Guarded-Fabric-Shielded-VM/shielded-vms-shielding-data-file.png)

## <a name="what-are-the-types-of-virtual-machines-that-a-guarded-fabric-can-run"></a>Quels sont les types de machines virtuelles qu’une structure protégée peut exécuter ?

Les structures protégées sont capables d’exécuter des machines virtuelles de trois façons différentes :

1.  Une machine virtuelle normale n’offrant aucune protection au-delà des versions précédentes d’Hyper-V
2.  Une machine virtuelle prise en charge par le chiffrement dont les protections peuvent être configurées par un administrateur de structure
3.  Une machine virtuelle dotée d’une protection maximale dont les protections sont toutes activées et qui ne peuvent pas être désactivées par un administrateur de structure

Les machines virtuelles prises en charge par le chiffrement sont conçues pour être utilisées quand les administrateurs de structure sont entièrement fiables.  Par exemple, une entreprise peut déployer une structure protégée afin de s’assurer que les disques de machines virtuelles sont chiffrés au repos par souci de conformité. Les administrateurs de structure peuvent continuer à utiliser des fonctionnalités de gestion pratiques, telles que les connexions à la console de la machine virtuelle, PowerShell Direct et d’autres outils de gestion et de dépannage courants.

Les machines virtuelles dotées d’une protection maximale sont destinées à être utilisées dans des structures où les données et l’état de la machine virtuelle doivent être protégés à la fois contre les administrateurs de structure et contre les logiciels non approuvés susceptibles de s’exécuter sur les hôtes Hyper-V. Par exemple, les machines virtuelles dotées d’une protection maximale n’autorisent jamais une connexion à la console de la machine virtuelle alors qu’un administrateur de structure peut activer ou désactiver cette protection pour les machines virtuelles prises en charge par le chiffrement.

Le tableau suivant résume les différences entre les machines virtuelles protégées et de chiffrement pris en charge.

| Fonctionnalité        | Génération 2 prise en charge par le chiffrement     | Génération 2 dotée d’une protection maximale         |
|----------|--------------------|----------------|
|Démarrage sécurisé        | Oui, obligatoire mais configurable        | Oui, obligatoire et appliqué    |
|Module de plateforme sécurisée virtuelle (vTPM)               | Oui, obligatoire mais configurable        | Oui, obligatoire et appliqué    |
|Chiffrer l’état et le trafic de migration dynamique d’une machine virtuelle | Oui, obligatoire mais configurable |  Oui, obligatoire et appliqué  |
|Composants d’intégration | Configurable par l’administrateur de structure      | Certains composants d’intégration bloqués (par exemple, l’échange de données, PowerShell Direct) |
|Connexion à une machine virtuelle (console), périphériques HDI (par exemple, clavier, souris) | Activé. Ne peut pas être désactivé | Activé sur les ordinateurs hôtes à partir de Windows Server version 1803 ; Désactivé sur les ordinateurs hôtes antérieures |
|Ports COM/série   | Prise en charge                             | Désactivé (ne peut pas être activé) |
|Attacher un débogueur (pour le processus de la machine virtuelle)<sup>1</sup>| Prise en charge          | Désactivé (ne peut pas être activé) |

<sup>1</sup> les débogueurs traditionnels attachement directement à un processus, tels que WinDbg.exe, sont bloqués pour des machines virtuelles protégées, car le processus de travail de la machine virtuelle (VMWP.exe) est une lumière de processus protégé (PPL). Autres techniques de débogage, tels que ceux utilisés par LiveKd.exe, ne sont pas bloqués. Contrairement aux machines virtuelles protégées, le processus de travail pour les machines virtuelles de chiffrement pris en charge ne s’exécute pas comme une bibliothèque de modèles parallèles pour les débogueurs traditionnels comme WinDbg.exe continueront à fonctionner normalement. 

Les machines virtuelles dotées d’une protection maximale et les machines virtuelles prises en charge par le chiffrement continuent à prendre en charge les fonctionnalités de gestion de structure courantes, telles que la migration dynamique, le réplica Hyper-V, les points de contrôle de la machine virtuelle, etc.

## <a name="the-host-guardian-service-in-action-how-a-shielded-vm-is-powered-on"></a>Le Service Guardian hôte en action : Comment une machine virtuelle protégée est sous tension

![Fichier de données de protection](../media/Guarded-Fabric-Shielded-VM/shielded-vms-how-a-shielded-vm-is-powered-on.png)

1. VM01 est sous tension.

    Avant qu’un hôte Service Guardian puisse mettre sous tension une machine virtuelle dotée d’une protection maximale, il faut d’abord certifier qu’elle est intègre. Pour prouver qu’elle est intègre, elle doit présenter un certificat d’intégrité au service de protection de clé (KPS). Le certificat d’intégrité est obtenu via le processus d’attestation.

2. L’hôte demande une attestation.

    L’hôte Service Guardian demande une attestation. Le mode d’attestation est dicté par le Service Guardian hôte :

    **Attestation approuvée par le module de plateforme sécurisée**: Hôte Hyper-V envoie des informations qui inclut :

       - Informations d’identification du module de plateforme sécurisée (TPM) (sa paire de clés de type EK (Endorsement Key))
       - Informations sur les processus qui ont été démarrés pendant la séquence de démarrage la plus récente (le journal TCG)
       - Informations sur la stratégie d’intégrité du Code (CI) qui a été appliquée sur l’ordinateur hôte. 

       Attestation happens when the host starts and every 8 hours thereafter. If for some reason a host doesn't have an attestation certificate when a VM tries to start, this also triggers attestation.

    **Héberger l’attestation de clé**: Hôte Hyper-V envoie le grand public de la moitié de la paire de clés. SGH valide l’hôte clé est inscrite. 
    
    **Attestation approuvée par l’administrateur**: Hôte Hyper-V envoie un ticket Kerberos, qui identifie les groupes de sécurité figurant dans l’hôte. SGH valide le fait que l’hôte appartient à un groupe de sécurité qui a été précédemment configuré par l’administrateur SGH approuvé.

3. L’attestation réussit (ou échoue).

    Le mode d’attestation détermine quels contrôles sont nécessaires pour correctement attester de que l’hôte est sain. Avec l’attestation approuvée par le module de plateforme sécurisée, de l’hôte module de plateforme sécurisée identité, des mesures de démarrage et stratégie d’intégrité du code sont validés. Avec l’attestation de clé hôte, uniquement l’inscription de la clé d’hôte est validée. 

4. Le certificat d’attestation est envoyé à l’hôte.

    En supposant que l’attestation a réussi, un certificat d’intégrité est envoyé à l’hôte et l’hôte est considéré comme « hôte service Guardian » (autorisé à exécuter des machines virtuelles protégées). L’hôte utilise le certificat d’intégrité pour autoriser le service de protection de clé à libérer en toute sécurité les clés nécessaires pour travailler avec des machines virtuelles dotées d’une protection maximale.

5. L’hôte demande une clé de machine virtuelle.

    L’hôte Service Guardian n’a pas les clés nécessaires pour mettre sous tension une machine virtuelle dotée d’une protection maximale (VM01 dans le cas présent). Pour obtenir les clés nécessaires, l’hôte Service Guardian doit fournir les éléments suivants à KPS :

    - Le certificat d’intégrité actuel
    - Un secret chiffré (un protecteur de clé ou KP) qui contient les clés nécessaires pour mettre sous tension VM01. Le secret est chiffré à l’aide d’autres clés que seul KPS connaît.

6. Libération de la clé.

    KPS examine le certificat d’intégrité pour déterminer sa validité. Le certificat ne doit pas avoir expiré et KPS doit approuver le service d’attestation qui l’a émis.

7. La clé est retournée à l’hôte.

    Si le certificat d’intégrité est valide, KPS tente de déchiffrer la clé secrète et de retourner en toute sécurité les clés nécessaires pour mettre sous tension la machine virtuelle. Notez que les clés sont chiffrées à VBS l’hôte service Guardian.

8. L’hôte met VM01 sous tension.

## <a name="guarded-fabric-and-shielded-vm-glossary"></a>Glossaire de la structure protégée et des machines virtuelles dotées d’une protection maximale

| Terme              | Définition           |
|----------|------------|
| Service Guardian hôte (SGH) | Rôle Windows Server installé sur un cluster sécurisé de serveurs nus qui est capable de mesurer l’intégrité d’un hôte Hyper-V et de libérer des clés pour les hôtes Hyper-V intègres lors de la mise sous tension ou de la migration dynamique de machines virtuelles dotées d’une protection maximale. Ces deux fonctions sont indispensables pour une solution de machine virtuelle dotée d’une protection maximale et sont appelées **Service d’attestation** et **Service de protection de clé**, respectivement. |
| hôte Service Guardian | Hôte Hyper-V sur lequel les machines virtuelles dotées d’une protection maximale peuvent s’exécuter. Un hôte peut uniquement être considéré comme _service Guardian_ quand il a été jugé intègre par le service d’Attestation SHG. Les machines virtuelles dotées d’une protection maximale ne peuvent pas être mises sous tension ou migrées dynamiquement vers un hôte Hyper-V qui n’est pas encore attesté ou dont l’attestation a échoué. |
| structure protégée    | Terme générique utilisé pour décrire une structure d’hôtes Hyper-V et leur Service Guardian hôte qui a la possibilité de gérer et d’exécuter des machines virtuelles dotées d’une protection maximale. |
| machine virtuelle dotée d’une protection maximale | Machine virtuelle qui peut s’exécuter uniquement sur des hôtes Service Guardian et qui est protégée contre l’inspection, la falsification et le vol par des administrateurs de structure malveillants et des programmes malveillants hôtes. |
| administrateur de structure | Administrateur de cloud public ou privé qui peut gérer des machines virtuelles. Dans le contexte d’une structure protégée, un administrateur de structure n’a pas accès aux machines virtuelles dotées d’une protection maximale ni aux stratégies qui déterminent quelles machines virtuelles dotées d’une protection maximale peuvent s’exécuter dessus. |
| administrateur SGH | Administrateur approuvé dans le cloud public ou privé et qui a l’autorité nécessaire pour gérer les stratégies et le matériel de chiffrement pour les hôtes Service Guardian, c’est-à-dire les hôtes sur lesquels une machine virtuelle dotée d’une protection maximale peut s’exécuter.|
| fichier de données d’approvisionnement ou fichier de données de protection (fichier PDK) | Fichier chiffré qu’un locataire ou un utilisateur crée pour contenir des informations de configuration de machine virtuelle importantes et pour protéger ces informations contre tout accès par d’autres utilisateurs. Par exemple, un fichier de données de protection peut contenir le mot de passe qui sera affecté au compte Administrateur local lors de la création de la machine virtuelle. |
| sécurité basée sur la virtualisation (VBS) | Hyper-V en fonction de traitement et l’environnement de stockage qui est protégé contre les administrateurs. Le mode sécurisé virtuel fournit au système la possibilité de stocker les clés de système d’exploitation qui ne sont pas visibles par un administrateur de système d’exploitation.|
| module de plateforme sécurisée (TPM) virtuel | Version virtualisée d’un module de plateforme sécurisée (TPM). À compter de Hyper-V dans Windows Server 2016, vous pouvez fournir un périphérique TPM 2.0 virtuel afin que les machines virtuelles peuvent être chiffrées, tout comme un TPM physique permet à un ordinateur physique à chiffrer.|

## <a name="see-also"></a>Voir aussi

- [Structure protégée et machines virtuelles protégées](guarded-fabric-and-shielded-vms-top-node.md)
- Blog : [Centre de données et le Blog de sécurité de Cloud privé](https://blogs.technet.microsoft.com/datacentersecurity/)
- Vidéo : [Présentation des Machines virtuelles protégées](https://channel9.msdn.com/Shows/Mechanics/Introduction-to-Shielded-Virtual-Machines-in-Windows-Server-2016)
- Vidéo : [Plongez dans les machines virtuelles protégées avec Windows Server 2016 Hyper-V](https://channel9.msdn.com/events/Ignite/2016/BRK3124)
