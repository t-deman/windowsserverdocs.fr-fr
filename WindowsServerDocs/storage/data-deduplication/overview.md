---
ms.assetid: 4b844404-36ba-4154-aa5d-237a3dd644be
title: Vue d’ensemble de la déduplication des données
ms.technology: storage-deduplication
ms.prod: windows-server-threshold
ms.topic: article
author: wmgries
manager: klaasl
ms.author: wgries
ms.date: 05/09/2017
ms.openlocfilehash: bf346844337740f7585070ff78de4e7f61f25624
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447261"
---
# <a name="data-deduplication-overview"></a>Vue d’ensemble de la déduplication des données

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel), 

## <a name="what-is-dedup"></a>Quelle est la déduplication des données ?

Déduplication des données, souvent appelée tout simplement déduplication, sont une fonctionnalité qui permet de réduire l’impact des données redondantes sur les coûts de stockage. Quand elle est activée, la déduplication des données optimise l’espace libre sur un volume en examinant les données qu’il contient et en recherchant les parties dupliquées sur le volume. Les parties dupliquées du jeu de données du volume sont stockées une seule fois et sont (éventuellement) compressées pour réaliser encore plus d’économies. La déduplication des données permet d’optimiser les redondances sans compromettre la fidélité ni l’intégrité des données. Vous trouverez un complément d’informations sur le fonctionnement de la déduplication des données dans la section « [Fonctionnement de la déduplication des données](understand.md#how-does-dedup-work) » de la page [Présentation de la déduplication des données](understand.md).

> [!Important]  
> [KB4025334](https://support.microsoft.com/kb/4025334) contient une restauration jusqu'à correctifs de la déduplication des données, y compris la fiabilité des correctifs, et nous vous recommandons vivement de l’installer lors de l’utilisation de la déduplication des données avec Windows Server 2016 et Windows Server 2019.

## <a name="why-is-dedup-useful"></a>Pourquoi la déduplication des données est-elle utile ?

La déduplication des données permet aux administrateurs du stockage de réduire les coûts associés aux données dupliquées. Les jeux de données volumineux impliquent souvent **<u>beaucoup</u>** de duplication, laquelle augmente les coûts du stockage des données. Exemple :

- Les partages de fichiers utilisateur peuvent comporter de nombreuses copies de fichiers identiques ou très similaires.
- Les invités de virtualisation peuvent être presque identiques d’une machine virtuelle à l’autre.
- Les instantanés de sauvegarde peuvent afficher des différences mineures d’un jour à l’autre.

L’espace que vous pouvez gagner avec la déduplication des données dépend du jeu de données ou de la charge de travail au niveau du volume. Les jeux de données qui présentent une duplication élevée peuvent obtenir des taux d’optimisation allant jusqu’à 95 % ou une utilisation du stockage divisée par 20. Le tableau ci-dessous met en relief les économies réalisées par une déduplication standard sur différents types de contenu :

| Scénario       | Contenu                                        | Gains d’espace types |
|----------------|------------------------------------------------|-----------------------|
| Documents utilisateur | Documents Office, photos, musique, vidéos, etc.  | 30-50 %                |
| Partages de déploiement | Fichiers binaires de logiciels, fichiers cab, symboles, etc. | 70-80 %                |
| Bibliothèques de virtualisation | Fichiers ISO, fichiers de disque dur virtuel, etc.  | 80-95 %                |
| Partage de fichiers général | Tout le contenu ci-dessus                           | 50-60 %                |

## <a id="when-can-dedup-be-used"></a>Lorsque la déduplication des données peut être utilisée ?  
<table>
    <tbody>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-clustered-gpfs.png" alt="Illustration of file servers" /></td>
            <td style="vertical-align:top">
                <b>Serveurs de fichiers à usage général</b><br />
Les serveurs de fichiers à usage général sont des serveurs de fichiers qui peuvent contenir l’un des types de partage suivants : <ul>
                    <li>Partages d’équipe</li>
                    <li>Dossiers de base d’utilisateur</li>
                    <li><a href="https://technet.microsoft.com/library/dn265974.aspx">Dossiers de travail</a></li>
                    <li>Partages de développement de logiciels</li>
                </ul>
Les serveurs de fichiers à usage général conviennent parfaitement à la déduplication des données, car les divers utilisateurs ont tendance à avoir plusieurs copies ou versions d’un même fichier. Les partages de développement logiciel tirent profit de la déduplication des données, car de nombreux fichiers binaires restent pratiquement inchangés d’une build à une autre. 
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-vdi.png" alt="Illustration of VDI servers" /></td>
            <td style="vertical-align:top">
                <b>Déploiements virtualisés Desktop Infrastructure (VDI)</b><br />
Les serveurs VDI, comme les <a href="https://technet.microsoft.com/library/cc725560.aspx">Services Bureau à distance</a>, offrent une option allégée aux organisations qui veulent approvisionner des postes de travail pour leurs utilisateurs. Il existe de nombreuses raisons pour qu’une organisation s’appuie sur cette technologie : <ul>
                    <li><b>Déploiement d’applications</b>: Vous pouvez rapidement déployer des applications au sein de votre entreprise. Cette possibilité s’avère particulièrement utile quand vous avez des applications qui sont fréquemment mises à jour, peu utilisées ou difficiles à gérer.</li>
                    <li><b>Consolidation des applications</b>: Lorsque vous installez et exécutez des applications à partir d’un ensemble de machines virtuelles gérées de manière centralisée, vous éliminez le besoin pour mettre à jour des applications sur les ordinateurs clients. Cette option a aussi pour effet de réduire la quantité de bande passante réseau nécessaire pour accéder aux applications.</li>
                    <li><b>Accès à distance</b>: Accessibles aux utilisateurs des applications d’entreprise à partir d’appareils tels que les ordinateurs domestiques, bornes, le matériel de faible puissance alimenté et systèmes d’exploitation autres que Windows.</li>
                    <li><b>Accès aux filiales</b>: Déploiements VDI peuvent fournir de meilleures performances d’application pour la branche des employés de bureau qui doivent accéder aux magasins de données centralisés. Les applications gourmandes en données ne disposent parfois pas de protocoles client/serveur optimisés pour les connexions lentes.</li>
                </ul>
Les déploiements VDI conviennent parfaitement à la déduplication des données, car les disques durs virtuels qui pilotent les postes de travail à distance pour les utilisateurs sont globalement identiques. De plus, la déduplication des données peut être utile face à aux <em>tempêtes VDI de démarrage</em>, à savoir la chute des performances de stockage au moment où un grand nombre d’utilisateurs se connectent simultanément à leur ordinateur en début de journée.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-backup.png" alt="Illustration of backup applications" /></td>
            <td style="vertical-align:top">
                <b>Cibles de sauvegarde, telles que les applications de sauvegarde virtualisées</b><br />
Les applications de sauvegarde, telles que <a href="https://technet.microsoft.com/library/hh758173.aspx">Microsoft Data Protection Manager (DPM)</a>, constituent d’excellents candidats à la déduplication des données en raison de la duplication significative entre les instantanés de sauvegarde.
            </td>
        </tr>
        <tr>
            <td style="text-align:center;min-width:150px;vertical-align:center;"><img src="media/overview-other.png" alt="Illustration of other workloads" /></td>
            <td style="vertical-align:top">
                <b>Autres charges de travail</b><br />
                <a href="install-enable.md#enable-dedup-candidate-workloads" data-raw-source="[Other workloads may also be excellent candidates for Data Deduplication](install-enable.md#enable-dedup-candidate-workloads)">D’autres charges de travail peuvent également convenir parfaitement à la déduplication des données</a>.
            </td>
        </tr>
    </tbody>
</table>
