---
ms.assetid: e4c31187-f15f-410b-bb79-8d63e2f2b421
title: Mettre à niveau des contrôleurs de domaine vers Windows Server 2012 R2 et Windows Server 2012
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6dda30bd15bedab8ea5ca8ca2e9597e1cc196e43
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443052"
---
# <a name="upgrade-domain-controllers-to-windows-server-2012-r2-and-windows-server-2012"></a>Mettre à niveau des contrôleurs de domaine vers Windows Server 2012 R2 et Windows Server 2012

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique fournit des informations générales sur les Services de domaine Active Directory dans Windows Server 2012 R2 et Windows Server 2012 et explique le processus de mise à niveau des contrôleurs de domaine à partir de Windows Server 2008 ou Windows Server 2008 R2.  
  
## <a name="BKMK_UpgradeWorkflow"></a>Procédure de mise à niveau de contrôleurs de domaine  
La méthode recommandée pour mettre à niveau un domaine consiste à promouvoir les contrôleurs de domaine qui exécutent des versions plus récentes de Windows Server et rétrograder les contrôleurs de domaine plus anciens en fonction des besoins. Cette méthode est préférable à la mise à niveau du système d’exploitation d’un contrôleur de domaine existant. Cette liste couvre les étapes générales à suivre avant de promouvoir un contrôleur de domaine qui exécute une version plus récente de Windows Server :  
  
1. Vérifiez que le serveur cible répond à la [configuration requise](https://technet.microsoft.com/library/dn303418.aspx).  
2. Vérifiez [Application compatibility](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_AppCompat).  
3. Vérifiez les paramètres de sécurité Pour plus d’informations, consultez [Fonctionnalités déconseillées et modifications de comportement associées aux services de domaine Active Directory dans Windows Server 2012](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2012-R2-and-Windows-Server-2012.md#BKMK_DeprecatedFeatures) et [Secure default settings in Windows Server 2008 et Windows Server 2008 R2](https://technet.microsoft.com/library/upgrade-domain-controllers-to-windows-server-2008-r2(WS.10).aspx#BKMK_SecureDefault).  
4. Vérifiez la connectivité au serveur cible à partir de l’ordinateur sur lequel vous envisagez d’exécuter l’installation.  
5. Vérifiez la disponibilité des rôles de maître d’opérations nécessaires :  

   - Pour installer le premier contrôleur de domaine qui exécute Windows Server 2012 dans une forêt et un domaine existant, l’ordinateur où vous exécutez l’installation nécessite une connectivité au contrôleur de schéma afin d’exécuter adprep /forestprep et au maître d’infrastructure afin d’exécuter adprep / DomainPrep.  
   - Pour installer le premier contrôleur de domaine dans un domaine où le schéma de la forêt est déjà étendu, seule une connectivité au maître d’infrastructure est requise.  
   - Pour installer ou supprimer un domaine dans une forêt existante, vous avez besoin d’une connectivité au maître d’opérations des noms de domaine.  
   - Toute installation d’un contrôleur de domaine requiert également une connectivité au maître RID.  
   - Si vous installez le premier contrôleur de domaine en lecture seule dans une forêt existante, vous avez besoin d’une connectivité au maître d’infrastructure pour chaque partition d’annuaire d’applications, également désignée sous le nom de contexte de nommage (autre que celui d’un domaine).  

6. Veillez à fournir les informations d’identification requises pour exécuter l’installation des services de domaine Active Directory.  

   |Action d’installation|Informations d’identification requises|  
   |-----------------------|---------------------------|  
   |Installer une nouvelle forêt|Administrateur local sur le serveur cible|  
   |Installer un nouveau domaine dans une forêt existante|Administrateurs de l’entreprise|  
   |Installer un autre contrôleur de domaine dans un domaine existant|Administrateurs du domaine|  
   |Exécuter adprep /forestprep|Administrateurs du schéma, Administrateurs de l’entreprise et Administrateurs du domaine|  
   |Exécuter adprep /domainprep|Administrateurs du domaine|  
   |Exécuter adprep /domainprep /gpprep|Administrateurs du domaine|  
   |Exécuter adprep /rodcprep|Administrateurs de l’entreprise|  

   Vous pouvez déléguer des autorisations pour installer les services de domaine Active Directory. Pour plus d’informations, voir [Tâches de gestion de l’installation](https://technet.microsoft.com/library/cc773327(WS.10).aspx).  

Vous pouvez trouver des instructions pas à pas pour promouvoir des contrôleurs de domaine Windows Server 2012 nouveaux et répliqués à l’aide des applets de commande Windows PowerShell et du Gestionnaire de serveur en suivant les liens ci-dessous :  

- [Installer les Services de domaine Active Directory (niveau 100)](https://technet.microsoft.com/library/hh472162.aspx)  
- [Installer une nouvelle forêt d’Active Directory de Windows Server 2012 (niveau 200)](https://technet.microsoft.com/library/jj574166.aspx)  
- [Installer un contrôleur de domaine Windows Server 2012 dans un domaine existant (niveau 200)](https://technet.microsoft.com/library/jj574134.aspx)  
- [Installer un nouvel enfant de Active Directory Windows Server 2012 ou d’un domaine d’arborescence (niveau 200)](https://technet.microsoft.com/library/jj574105.aspx)  
- [Installer un contrôleur de domaine Server 2012 Active Directory en lecture seule (RODC) (niveau 200) de Windows](https://technet.microsoft.com/library/jj574152.aspx)  
- [Guide pas à pas pour le paramètre de Windows Server 2012 contrôleur de domaine (en-US)](https://social.technet.microsoft.com/wiki/contents/articles/12370.step-by-step-guide-for-setting-up-windows-server-2012-domain-controller-en-us.aspx)  

## <a name="windows-update-considerations"></a>Considérations relatives à la mise à jour de Windows

Avant la version Windows 8, Windows Update gérait sa propre planification interne pour rechercher les mises à jour, puis pour les télécharger et les installer. Pour cela, l'Agent de mise à jour automatique Windows Update devait toujours être en cours d'exécution en arrière-plan et utilisait de la mémoire ainsi que d'autres ressources système.  
  
Windows 8 et Windows Server 2012 introduisent la nouvelle fonctionnalité de [maintenance automatique](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). La maintenance automatique consolide de nombreuses fonctionnalités différentes qui géraient chacune leur propre logique de planification et d'exécution. Cette consolidation permet à tous ces composants d’utiliser beaucoup moins de ressources système, de fonctionner de façon cohérente, de respecter le nouvel état [Veille connectée](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) pour les nouveaux types d’appareils et de consommer moins de batterie sur les appareils mobiles.  
  
Comme Windows Update fait partie de la maintenance automatique dans Windows 8 et Windows Server 2012, sa propre planification interne pour définir un jour et une heure d'installation des mises à jour n'est plus en vigueur. Pour garantir un comportement de redémarrage cohérent et prévisible pour tous les appareils et ordinateurs de votre entreprise, notamment ceux qui exécutent Windows 8 et Windows Server 2012, consultez l’article de la Base de connaissances Microsoft [2885694](https://support.microsoft.com/kb/2885694) (ou le correctif cumulatif d’octobre 2013 [2883201](https://support.microsoft.com/kb/2883201)), puis configurez les paramètres de stratégie décrits dans le billet de blog WSUS [Enabling a more predictable Windows Update experience for Windows 8 and Windows Server 2012 (KB 2885694)](http://blogs.technet.com/b/wsus/archive/2013/10/08/enabling-a-more-predictable-windows-update-experience-for-windows-8-and-windows-server-2012-kb-2885694.aspx)(en anglais).  

## <a name="BKMK_NewWS2012R2"></a>Nouveautés dans AD DS dans Windows Server 2012 R2 ?

Le tableau suivant récapitule les nouvelles fonctionnalités pour les services de domaine Active Directory dans Windows Server 2012 R2, avec un lien vers des informations plus détaillées le cas échéant. Pour obtenir une explication plus détaillée de certaines fonctionnalités, notamment les conditions requises, voir [Nouveautés d’Active Directory sous Windows Server 2012 R2](https://technet.microsoft.com/library/dn268294.aspx).  

|Fonctionnalité|Description|  
|-----------|---------------|  
|[Workplace Join](https://technet.microsoft.com/library/dn280945.aspx)|Permet aux travailleurs de l'information d'établir une connexion entre leurs appareils personnels et leur entreprise pour accéder aux ressources et services de celle-ci.|  
|[Proxy d’Application Web](https://technet.microsoft.com/library/dn280942.aspx)|Fournit l'accès à une application web à l'aide d'un nouveau service de rôle Accès à distance.|  
|[Services de fédération Active Directory (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Les services AD FS ont simplifié le déploiement et apporté des améliorations pour permettre aux utilisateurs d'accéder aux ressources à partir des appareils personnels et aider les services informatiques à gérer le contrôle d'accès.|  
|[Unicité des noms SPN et UPN](https://technet.microsoft.com/library/dn535779.aspx)|Les contrôleurs de domaine exécutant Windows Server 2012 R2 bloquent la création de noms de principal du service (SPN) et de noms d'utilisateurs principaux (UPN) en double.|  
|[Connexion de redémarrage automatique Winlogon](https://technet.microsoft.com/library/dn535772.aspx)|Permet aux applications d'écran de verrouillage d'être redémarrées et disponibles sur les appareils Windows 8.1.|  
|[Attestation de clé TPM](https://technet.microsoft.com/library/dn581921.aspx)|Permet aux autorités de certification d'attester par chiffrement dans un certificat émis que la clé privée du demandeur de certificat est réellement protégée par un module de plateforme sécurisée (TPM).|  
|[Gestion et protection des informations d'identification](https://technet.microsoft.com/library/dn408190.aspx)|Nouveaux contrôles d'authentification de domaine et de protection des informations d'identification pour réduire le vol d'informations d'identification.|  
|[Désapprobation du Service de réplication de fichiers (FRS)](https://technet.microsoft.com/library/dn535775.aspx)|Le niveau fonctionnel de domaine Windows Server 2003 est également déconseillé, car le service de réplication de fichiers permet de répliquer SYSVOL au niveau fonctionnel. Cela signifie que, quand vous créez un domaine sur un serveur qui exécute Windows Server 2012 R2, le niveau fonctionnel de domaine doit être Windows Server 2008 ou version plus récente. Vous pouvez toujours ajouter un contrôleur de domaine qui exécute Windows Server 2012 R2 à un domaine existant qui a un niveau fonctionnel du domaine Windows Server 2003 ; Vous ne pouvez pas simplement créer un nouveau domaine à ce niveau.|  
|[Nouveaux niveaux fonctionnels de domaine et de forêt](../active-directory-functional-levels.md)|Il existe de nouveaux niveaux fonctionnels pour Windows Server 2012 R2. De nouvelles fonctionnalités sont disponibles au niveau fonctionnel de domaine Windows Server 2012 R2.|  
|[Modifications d’optimiseur de requête LDAP](https://technet.microsoft.com/library/dn535775.aspx)|Améliorations des performances au niveau de l'efficacité des recherches LDAP et du temps de recherche LDAP pour les requêtes complexes.|  
|[Améliorations de l'événement 1644](https://technet.microsoft.com/library/dn535775.aspx)|Les statistiques des résultats de recherche LDAP ont été ajoutées à l'ID d'événement 1644 pour faciliter le dépannage.|  
|[Amélioration du débit de réplication Active Directory](https://technet.microsoft.com/library/dn535775.aspx)|Remplace le débit de réplication Active Directory maximal de 40 Mbits/s par 600 Mbits/s environ.|  

## <a name="BKMK_WhatsNewAD"></a>Nouveautés dans AD DS dans Windows Server 2012

Le tableau suivant récapitule les nouvelles fonctionnalités pour les services de domaine Active Directory dans Windows Server 2012, avec un lien vers des informations plus détaillées le cas échéant. Pour une explication plus détaillée de certaines fonctionnalités, notamment les conditions requises, consultez [quelles sont les nouveautés dans les Services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/hh831477.aspx).  
  
|Fonctionnalité|Description|  
|-----------|---------------|  
|Activation basée sur Active Directory ; voir [Vue d’ensemble de l’activation en volume](https://technet.microsoft.com/library/hh831612.aspx)|Simplifie la tâche de configuration de la distribution et de la gestion des licences de logiciels en volume.|  
|[Active Directory Federation Services (AD FS)](https://technet.microsoft.com/library/hh831502.aspx)|Ajoute l’installation du rôle via le Gestionnaire de serveur, une configuration d’approbation simplifiée, une gestion de la relation d’approbation automatique, une prise en charge du protocole SAML, et bien plus encore.|  
|Événements de vidage des pages perdues Active Directory|L’événement NTDS ISAM 530 avec erreur Jet -1119 est consigné pour détecter les événements de vidage des pages perdues dans les bases de données Active Directory.|  
|[Interface utilisateur de la Corbeille Active Directory](https://technet.microsoft.com/library/hh831702.aspx#ad_recycle_bin_mgmt)|Le Centre d’administration Active Directory ajoute une gestion d’interface graphique utilisateur de la fonctionnalité Corbeille introduite à l’origine dans Windows Server 2008 R2.|  
|[Applets de commande Active Directory Replication topologie et de Windows PowerShell](https://technet.microsoft.com/library/hh831757.aspx)|Prend en charge la création et la gestion des sites, liens de sites, objets de connexion Active Directory (entre autres) à l’aide de Windows PowerShell.|  
|[Contrôle d’accès dynamique](https://technet.microsoft.com/library/hh831717.aspx)|Nouvelle plateforme d’autorisation basée sur des revendications qui améliore le modèle de contrôle d’accès hérité.|  
|[Interface utilisateur de stratégie de mot de passe affinées](https://technet.microsoft.com/library/hh831702.aspx#fine_grained_pswd_policy_mgmt)|Le Centre d’administration Active Directory ajoute la prise en charge d’interface graphique utilisateur pour la création, la modification et l’affectation des objets PSO ajoutés à l’origine dans Windows Server 2008.|  
|[Groupe des comptes de Service administrés (gMSA)](https://technet.microsoft.com/library/hh831782.aspx)|Nouveau type de principal de sécurité connu sous le nom de compte de service administré de groupe. Les services exécutés sur plusieurs hôtes peuvent être exécutés sous le même compte de service administré de groupe.|  
|[Jonction de domaine hors connexion DirectAccess](https://technet.microsoft.com/library/jj574150.aspx)|Étend la jonction de domaine hors connexion en incluant les conditions préalables DirectAccess.|  
|[Déploiement rapide via le clonage de contrôleur de domaine virtuel](https://technet.microsoft.com/library/hh831734.aspx#virtualized_dc_cloning)|Les contrôleurs de domaine virtualisés peuvent être rapidement déployés en clonant des contrôleurs de domaine virtuels existants à l’aide des applets de commande Windows PowerShell.|  
|[Modifications du pool RID](https://technet.microsoft.com/library/jj574229.aspx)|Ajoute de nouveaux quotas et événements de contrôle pour empêcher une consommation excessive du pool RID global. Double éventuellement la taille du pool RID global si le pool d’origine est épuisé.|  
|Service de temps sécurisé|Améliore la sécurité pour W32tm en supprimant des secrets du réseau, en supprimant les fonctions de hachage MD5 et en demandant au serveur de s’authentifier auprès des clients du service de temps Windows 8|  
|[Protection des restaurations USN pour les contrôleurs de domaine virtualisés](https://technet.microsoft.com/library/hh831734.aspx#safe_virt_dc)|La restauration accidentelle des sauvegardes de captures instantanées des contrôleurs de domaine virtualisés ne provoque plus de restauration USN.|  
|[Afficheur d’historique PowerShell Windows](https://technet.microsoft.com/library/hh831702.aspx#windows_powershell_history_viewer)|Permet aux administrateurs d’afficher les commandes Windows PowerShell exécutées lors de l’utilisation du Centre d’administration Active Directory.|  
  
### <a name="BKMK_"></a>Maintenance automatique et modifications de comportement de redémarrage après que les mises à jour sont appliquées par Windows Update

Avant la version Windows 8, Windows Update gérait sa propre planification interne pour rechercher les mises à jour, puis pour les télécharger et les installer. Pour cela, l'Agent de mise à jour automatique Windows Update devait toujours être en cours d'exécution en arrière-plan et utilisait de la mémoire ainsi que d'autres ressources système.  
  
Windows 8 et Windows Server 2012 introduisent la nouvelle fonctionnalité de [maintenance automatique](https://msdn.microsoft.com/library/windows/desktop/hh848037(v=vs.85).aspx). La maintenance automatique consolide de nombreuses fonctionnalités différentes qui géraient chacune leur propre logique de planification et d'exécution. Cette consolidation permet à tous ces composants d’utiliser beaucoup moins de ressources système, de fonctionner de façon cohérente, de respecter le nouvel état [Veille connectée](https://msdn.microsoft.com/library/windows/hardware/jj248729.aspx) pour les nouveaux types d’appareils et de consommer moins de batterie sur les appareils mobiles.  
  
Comme Windows Update fait partie de la maintenance automatique dans Windows 8 et Windows Server 2012, sa propre planification interne pour définir un jour et une heure d'installation des mises à jour n'est plus en vigueur. Pour garantir un comportement de redémarrage cohérent et prévisible pour tous les appareils et ordinateurs de votre entreprise, notamment ceux qui exécutent Windows 8 et Windows Server 2012, vous pouvez configurer les paramètres de stratégie de groupe suivants :  

- **Configuration ordinateur | Stratégies | Modèles d’administration | Les composants Windows | Mise à jour de Windows | Configurer les mises à jour automatiques**  
- **Configuration ordinateur | Stratégies | Modèles d’administration | Les composants Windows | Mise à jour de Windows | Aucun redémarrage automatique avec des utilisateurs connectés**  
- **Configuration ordinateur | Stratégies | Modèles d’administration | Les composants Windows | Planificateur de maintenance | Délai aléatoire de maintenance**  

Le tableau suivant répertorie certains exemples de configuration de ces paramètres pour fournir le comportement de redémarrage souhaité.  

|||  
|-|-|  
|**Scénario**|**Configurations recommandées**|  
|**Géré par WSUS**<br /><br />-Installer les mises à jour une fois par semaine<br />-Redémarrer les vendredis à 23 h 00|Définition d'une installation automatique sur les ordinateurs, aucun redémarrage automatique jusqu'au moment voulu<br /><br />**Stratégie** : Configuration du service Mises à jour automatiques (activée)<br /><br />Configuration de la mise à jour automatique : 4 - téléchargement automatique et planification des installations<br /><br />**Stratégie** : Pas de redémarrage automatique avec les utilisateurs connectés (désactivé)<br /><br />**Échéance WSUS** : les vendredis à 23:00|  
|**Géré par WSUS**<br /><br />-Échelonner les installations sur différentes heures/journées|Définition de groupes cibles pour différents groupes d'ordinateurs qui doivent être mis à jour ensemble<br /><br />Utilisation des étapes ci-dessus pour le scénario précédent<br /><br />Définition de différentes échéances pour différents groupes cibles|  
|**Pas ne gérés par WSUS - échéances d’aucune prise en charge pour**<br /><br />-Échelonner les installations à des moments différents|**Stratégie** : Configuration du service Mises à jour automatiques (activée)<br /><br />Configuration de la mise à jour automatique : 4 - téléchargement automatique et planification des installations<br /><br />**Clé de Registre :** Activez la clé de Registre présentée dans l’article KB Microsoft [2835627](https://support.microsoft.com/kb/2835627)<br /><br />**stratégie :** Délai aléatoire de maintenance automatique (activée)<br /><br />Affectation de la valeur PT6H au **Délai aléatoire de la maintenance classique** pour un délai aléatoire de 6 heures afin d'obtenir le comportement suivant :<br /><br />-Mises à jour seront installées à l’heure de maintenance configurée plus un délai aléatoire<br /><br />-Redémarrer pour chaque ordinateur aura lieu exactement 3 jours plus tard<br /><br />Vous pouvez également définir une heure de maintenance différente pour chaque groupe d'ordinateurs|  

Pour plus d’informations sur la raison pour laquelle l’équipe d’ingénieurs Windows a implémenté ces modifications, voir [Minimizing restarts after automatic updating in Windows Update](http://blogs.msdn.com/b/b8/archive/2011/11/14/minimizing-restarts-after-automatic-updating-in-windows-update.aspx)(en anglais).  

## <a name="BKMK_InstallationChanges"></a>Modifications d’installation de rôle de serveur AD DS

De Windows Server 2003 à Windows Server 2008 R2, vous exécutiez la version x86 ou X64 de l’outil en ligne de commande Adprep.exe avant d’exécuter l’Assistant Installation de Active Directory, Dcpromo.exe, et Dcpromo.exe proposait des variantes facultatives pour une installation à partir du support ou une installation sans assistance.  
  
À compter de Windows Server 2012, les installations de ligne de commande sont effectuées à l'aide du module ADDSDeployment dans Windows PowerShell. Des promotions basées sur l’interface graphique utilisateur sont effectuées dans le Gestionnaire de serveur à l’aide d’un Assistant Configuration des services de domaine Active Directory entièrement nouveau. Pour simplifier le processus d’installation, la commande ADPREP a été intégrée à l’installation des services de domaine Active Directory et est exécutée automatiquement si nécessaire. L’Assistant de Configuration basé sur Windows PowerShell Active Directory cible automatiquement les rôles de maître schéma et d’infrastructure dans les domaines où les contrôleurs de domaine sont ajoutés, puis exécutant à distance les commandes ADPREP requises sur les contrôleurs de domaine appropriés.  
  
Les vérifications de configuration requise dans l’Assistant Installation des services de domaine Active Directory identifient les erreurs potentielles avant le début de l’installation. Les conditions d’erreur peuvent être corrigées pour éliminer les problèmes résultant d’une mise à niveau partiellement terminée. L’Assistant exporte également un script Windows PowerShell qui contient toutes les options qui ont été spécifiées pendant l’installation graphique.  
  
Dans leur ensemble, les modifications apportées à l'installation des services de domaine Active Directory simplifient le processus d'installation du rôle de contrôleur de domaine et réduisent la probabilité d'erreurs d'administration, en particulier quand vous déployez plusieurs contrôleurs de domaine sur des régions et domaines globaux.  
Pour obtenir des informations plus détaillées sur l’interface graphique utilisateur et les installations Windows PowerShell, notamment la syntaxe de ligne de commande et les instructions pas à pas de l’Assistant, voir [Installer les services de domaine Active Directory](https://technet.microsoft.com/library/hh472162.aspx). Pour les administrateurs qui veulent contrôler l’introduction des modifications de schéma dans une forêt Active Directory indépendamment de l’installation des contrôleurs de domaine Windows Server 2012 dans une forêt existante, les commandes Adprep.exe peuvent toujours être exécutées à une invite de commandes avec élévation de privilèges.  

## <a name="BKMK_DeprecatedFeatures"></a>Fonctionnalités déconseillées et modifications de comportement associées aux services AD DS dans Windows Server 2012

Il existe quelques modifications associées aux services de domaine Active Directory :  

- **Désapprobation d’Adprep32.exe**
   - Il n'existe qu'une seule version d'Adprep.exe et elle peut être exécutée en fonction des besoins sur des serveurs 64 bits qui exécutent Windows Server 2008 ou version ultérieure. Elle peut être exécutée à distance, et doit l’être si ce rôle de maître d’opérations ciblé est hébergé sur un système d’exploitation 32 bits ou Windows Server 2003.  
- **Désapprobation de Dcpromo.exe**
   - Dcpromo est déconseillé, même dans Windows Server 2012 uniquement, il peut toujours être exécuté avec un fichier de réponses ou des paramètres de ligne de commande pour donner aux organisations le temps de convertir l’automatisation existante vers les nouvelles options d’installation de Windows PowerShell.  
- **LMHash est désactivé sur les comptes d’utilisateur**
  - Les paramètres par défaut sécurisés dans les modèles de sécurité de Windows Server 2008, Windows Server 2008 R2 et Windows Server 2012 activent la stratégie NoLMHash qui est désactivée dans les modèles de sécurité des contrôleurs de domaine Windows 2000 et Windows Server 2003. Désactivez la stratégie NoLMHash pour les clients dépendant de LMHash en fonction des besoins, en suivant les étapes de l’article [946405](https://support.microsoft.com/kb/946405) de la Base de connaissances.  

À compter de Windows Server 2008, les contrôleurs de domaine ont également les paramètres par défaut sécurisés suivants, comparés aux contrôleurs de domaine qui exécutent Windows Server 2003 ou Windows 2000.

|||||  
|-|-|-|-|  
|Type de chiffrement ou stratégie|Paramètre par défaut Windows Server 2008|Paramètre par défaut Windows Server 2012 et Windows Server 2008 R2|Commentaire|  
|AllowNT4Crypto|Désactivée|Désactivée|Les clients du protocole SMB (Server Message Block) tiers peuvent être incompatibles avec les paramètres par défaut sécurisés sur les contrôleurs de domaine. Dans tous les cas, la valeur de ces paramètres peut être moins stricte pour permettre l’interopérabilité, mais uniquement au détriment de la sécurité. Pour plus d’informations, consultez [article 942564](https://go.microsoft.com/fwlink/?LinkId=164558) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=164558).|  
|DES|Enabled|Désactivée|[Article 977321](https://go.microsoft.com/fwlink/?LinkId=177717) dans la Base de connaissances Microsoft ()https://go.microsoft.com/fwlink/?LinkId=177717)|  
|Jeton de liaison de canal/Protection étendue pour l’authentification intégrée|N/A|Enabled|Consultez [l’avis de sécurité Microsoft (937811)](https://go.microsoft.com/fwlink/?LinkId=164559) (https://go.microsoft.com/fwlink/?LinkId=164559) et [article 976918](https://go.microsoft.com/fwlink/?LinkId=178251) dans la Base de connaissances Microsoft (https://go.microsoft.com/fwlink/?LinkId=178251).<br /><br />Examinez et installez le correctif logiciel de [article 977073](https://go.microsoft.com/fwlink/?LinkId=186394) (https://go.microsoft.com/fwlink/?LinkId=186394) dans la Base de connaissances Microsoft, en fonction des besoins.|  
|LMv2|Enabled|Désactivée|[Article 976918](https://go.microsoft.com/fwlink/?LinkId=178251) dans la Base de connaissances Microsoft ()https://go.microsoft.com/fwlink/?LinkId=178251)|  

## <a name="BKMK_SysReqs"></a>Configuration requise du système d’exploitation

La configuration minimale requise pour Windows Server 2012 est répertoriée dans le tableau suivant. Pour plus d’informations sur la configuration requise et la préinstallation, voir [Installation de Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx). Aucune configuration supplémentaire n’est requise pour installer une nouvelle forêt Active Directory, mais vous devez ajouter suffisamment de mémoire pour mettre en cache le contenu de la base de données Active Directory afin d’améliorer les performances pour les contrôleurs de domaine, les demandes de clients LDAP et les applications prenant en charge Active Directory. Si vous mettez à niveau un contrôleur de domaine existant ou que vous ajoutez un nouveau contrôleur de domaine à une forêt existante, passez en revue la section suivante pour vous assurer que l'espace disque du serveur est suffisant.  

|||  
|-|-|  
|Processeur|Processeur 1,4 GHz 64 bits|  
|RAM|512 Mo|  
|Espace disque spécifique requis|32 Go|  
|Résolution de l’écran|800 x 600, ou supérieure|  
|Divers|Lecteur de DVD, clavier, accès Internet|  

### <a name="BKMK_DiskSpaceDCWin8"></a>Espace disque requis pour la mise à niveau des contrôleurs de domaine

Cette section couvre l’espace disque requis uniquement pour la mise à niveau des contrôleurs de domaine à partir de Windows Server 2008 ou Windows Server 2008 R2. Pour plus d’informations sur l’espace disque requis pour mettre à niveau des contrôleurs de domaine vers des versions antérieures de Windows Server, voir [Espace disque requis pour la mise à niveau vers Windows Server 2008](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008) ou [Espace disque requis pour la mise à niveau vers Windows Server 2008 R2](https://technet.microsoft.com/library/cc754463(WS.10).aspx#BKMK_2008R2).  
  
Dimensionnez le disque qui héberge la base de données Active Directory et les fichiers journaux afin de prendre en compte les extensions de schéma personnalisées et pilotées par l’application, les index initiés par l’administrateur et l’application, plus l’espace pour les objets et attributs que vous ajouterez au répertoire pendant toute la durée du déploiement du contrôleur de domaine (en général, 5 à 8 ans). Bien évaluer la taille du disque au moment du déploiement est en général plus intéressant financièrement, car les coûts supplémentaires qu’implique l’ajout de stockage sur disque après le déploiement se révèlent plus élevés. Pour plus d’informations, voir [Planification de la capacité pour les services de domaine Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/14355.capacity-planning-for-active-directory-domain-services.aspx).  
  
Sur les contrôleurs de domaine que vous prévoyez de mettre à niveau, assurez-vous que le lecteur qui héberge la base de données Active Directory (NTDS.DIT) dispose d'une quantité d'espace disque disponible qui représente au moins 20 % du fichier NTDS.DIT avant de lancer la mise à niveau du système d'exploitation. Si l’espace disque disponible est insuffisant sur le volume, la mise à niveau peut échouer et le rapport de compatibilité de la mise à niveau retourne une erreur indiquant que la quantité d’espace disque disponible est insuffisante :  
  
Dans ce cas, vous pouvez tenter de lancer une défragmentation hors connexion de la base de données Active Directory pour libérer plus d’espace, puis réessayer la mise à niveau. Pour plus d’informations, voir [Compacter le fichier de base de données d’annuaire (défragmentation hors connexion)](https://technet.microsoft.com/library/cc794920(v=WS.10).aspx).  
  
### <a name="available-skus"></a>Références disponibles

Il existe 4 éditions de Windows Server : Foundation, Essentials, Standard et Datacenter.
Les deux éditions qui prennent en charge le rôle AD DS sont Standard et Datacenter.  
  
Dans les versions précédentes, les éditions de Windows Server étaient différentes dans la prise en charge des rôles de serveur, le nombre de processeurs et la prise en charge de grande mémoire. Les éditions Standard et Datacenter de Windows Server prennent en charge de toutes les fonctionnalités et le matériel sous-jacent, mais varient dans leurs droits de virtualisation - deux instances virtuelles sont autorisés pour l’Édition Standard et un nombre illimité d’instances virtuelles sont autorisées pour le centre de données Édition.  
  
### <a name="windows-client-and-windows-server-operating-systems-that-are-supported-to-join-windows-server-domains"></a>Systèmes d'exploitation Windows Server et clients Windows pris en charge pour rejoindre des domaines Windows Server

Les systèmes d'exploitation Windows Server et clients Windows suivants sont pris en charge pour des ordinateurs membres du domaine avec des contrôleurs de domaine qui exécutent Windows Server 2012 ou version ultérieure :  
  
- Systèmes d'exploitation clients : Windows 8.1, Windows 8, Windows 7, Windows Vista
   - Les ordinateurs qui exécutent Windows 8.1 ou Windows 8 sont également en mesure de rejoindre des domaines avec des contrôleurs de domaine qui exécutent une version antérieure de Windows Server, notamment Windows Server 2003 ou version ultérieure. Dans ce cas, cependant, certaines fonctionnalités Windows 8 peuvent nécessiter une configuration supplémentaire ou ne pas être disponibles. Pour obtenir plus d’informations sur ces fonctionnalités et d’autres recommandations pour la gestion des clients Windows 8 dans les domaines de niveau inférieur, voir [Exécution d’ordinateurs membres Windows 8 dans des domaines Windows Server 2003](https://social.technet.microsoft.com/wiki/contents/articles/17361.running-windows-8-member-computers-in-windows-server-2003-domains.aspx).  
- Systèmes d'exploitation Windows Server : Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 R2, Windows Server 2003  

## <a name="BKMK_UpgradePaths"></a>Chemins de mise à niveau sur place pris en charge

Les contrôleurs de domaine qui exécutent des versions 64 bits de Windows Server 2008 ou Windows Server 2008 R2 peuvent être mis à niveau vers Windows Server 2012. Vous ne pouvez pas mettre à niveau des contrôleurs de domaine qui exécutent Windows Server 2003 ou des versions 32 bits de Windows Server 2008. Pour les remplacer, installez des contrôleurs de domaine qui exécutent une version ultérieure de Windows Server dans le domaine, puis supprimez les contrôleurs de domaine Windows Server 2003.  

|Si vous exécutez ces éditions|Vous pouvez effectuer une mise à niveau vers ces éditions|  
|-------------------------------------|-------------------------------------|  
|Windows Server 2008 Standard avec SP2<br /><br />OU<br /><br />Windows Server 2008 Entreprise avec SP2|Windows Server 2012 Standard<br /><br />OU<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 Datacenter avec SP2|Windows Server 2012 Datacenter|  
|Windows Web Server 2008|Windows Server 2012 Standard|  
|Windows Server 2008 R2 Standard avec SP1<br /><br />OU<br /><br />Windows Server 2008 R2 Entreprise avec SP1|Windows Server 2012 Standard<br /><br />OU<br /><br />Windows Server 2012 Datacenter|  
|Windows Server 2008 R2 Datacenter avec SP1|Windows Server 2012 Datacenter|  
|Windows Web Server 2008 R2|Windows Server 2012 Standard|  
  
Pour plus d’informations sur les chemins de mise à niveau pris en charge, voir [Versions d’évaluation et options de mise à niveau pour Windows Server 2012](https://go.microsoft.com/fwlink/?LinkId=260917). Notez que vous ne pouvez pas convertir un contrôleur de domaine qui exécute une version d’évaluation de Windows Server 2012 directement en version commerciale. À la place, installez un contrôleur de domaine supplémentaire sur un serveur qui exécute une version commerciale, puis supprimez les services AD DS du contrôleur de domaine qui exécute la version d’évaluation.  
  
En raison d’un problème connu, vous ne pouvez pas mettre à niveau un contrôleur de domaine qui exécute une installation Server Core de Windows Server 2008 R2 vers une installation Server Core de Windows Server 2012. La mise à niveau se bloque sur un écran noir vers la fin du processus de mise à niveau. Le redémarrage de ces contrôleurs de domaine expose une option dans le fichier boot.ini pour restaurer la version de système d’exploitation précédente. Un autre redémarrage déclenche la restauration automatique de la version de système d’exploitation précédente. Jusqu'à ce qu’une solution est disponible, il est recommandé d’installer un nouveau contrôleur de domaine exécutant une installation Server Core de Windows Server 2012 au lieu d’un contrôleur de domaine existant qui exécute une installation Server Core de Windows Server de la mise à niveau sur place 2008 R2. Pour plus d’informations, voir l’article [2734222](https://support.microsoft.com/kb/2734222)de la Base de connaissances.  

## <a name="BKMK_FunctionalLevels"></a>Configuration requise et les fonctionnalités des niveaux fonctionnels

Windows Server 2012 nécessite un niveau fonctionnel de forêt Windows Server 2003. Autrement dit, avant de pouvoir ajouter un contrôleur de domaine qui exécute Windows Server 2012 à une forêt Active Directory existante, le niveau fonctionnel de forêt doit être Windows Server 2003 ou version ultérieure. Cela signifie que les contrôleurs de domaine qui exécutent Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 peuvent fonctionner dans la même forêt, mais que les contrôleurs de domaine qui exécutent Windows 2000 Server ne sont pas pris en charge et bloqueront l’installation d’un contrôleur de domaine qui exécute Windows Server 2012. Si la forêt contient des contrôleurs de domaine exécutant Windows Server 2003 ou version supérieure, mais que le niveau fonctionnel de la forêt est toujours Windows 2000, l’installation est également bloquée.  
  
Les contrôleurs de domaine Windows 2000 doivent être supprimés avant d’ajouter des contrôleurs de domaine Windows Server 2012 à votre forêt. Dans ce cas, examinez le flux de travail suivant :  

1. Installez des contrôleurs de domaine qui exécutent Windows Server 2003 ou version supérieure. Ces contrôleurs de domaine peuvent être déployés sur une version d’évaluation de Windows Server. Cette étape nécessite également l’[exécution d’adprep.exe](https://technet.microsoft.com/library/dd464018(WS.10).aspx) pour cette version du système d’exploitation comme condition préalable.  
2. Supprimez les contrôleurs de domaine Windows 2000. Plus précisément, rétrogradez correctement ou supprimez de force les contrôleurs de domaine Windows Server 2000 dans le domaine et utilisez Utilisateurs et ordinateurs Active Directory pour supprimer les comptes de tous les contrôleurs de domaine supprimés.  
3. Augmentez le niveau fonctionnel de la forêt à Windows Server 2003 ou version ultérieure.  
4. Installez des contrôleurs de domaine qui exécutent Windows Server 2012.  
5. Supprimez les contrôleurs de domaine qui exécutent des versions précédentes de Windows Server.  

Le nouveau niveau fonctionnel Windows Server 2012 offre une nouvelle fonctionnalité : le **prise en charge des revendications, l’authentification composée et le blindage Kerberos** KDC d’administration de stratégie de modèle a deux paramètres (**toujours fournir des revendications** et **les demandes d’authentification non blindées**) qui nécessitent le niveau fonctionnel du domaine Windows Server 2012.  
  
Le niveau fonctionnel de forêt Windows Server 2012 ne fournit pas toutes les nouvelles fonctionnalités, mais il garantit que tout nouveau domaine créé dans la forêt fonctionne automatiquement au niveau fonctionnel du domaine Windows Server 2012. Le niveau fonctionnel du domaine Windows Server 2012 ne fournit pas d’autres nouvelles fonctionnalités au-delà de prise en charge du contrôleur de domaine Kerberos des revendications, l’authentification composée et le blindage Kerberos. Mais il garantit que tout contrôleur de domaine dans le domaine exécute Windows Server 2012. Pour plus d’informations sur les autres fonctionnalités disponibles à différents niveaux fonctionnels, voir [Présentation des niveaux fonctionnels des services de domaine Active Directory (AD DS)](../active-directory-functional-levels.md).  
  
Après avoir défini le niveau fonctionnel de forêt à une certaine valeur, vous ne pouvez pas restaurer ou diminuer le niveau fonctionnel de forêt, avec les exceptions suivantes : lorsque vous augmentez le niveau fonctionnel de la forêt à Windows Server 2012, vous pouvez la réduire pour Windows Server 2008 R2. Si la Corbeille Active Directory n’a pas été activée, vous pouvez aussi diminuer la niveau fonctionnel de Windows Server 2012 vers Windows Server 2008 R2 ou Windows Server 2008 ou Windows Server 2008 R2 forêt à Windows Server 2008. Si le niveau fonctionnel de forêt est défini à Windows Server 2008 R2, il ne peut pas être rétabli, par exemple, vers Windows Server 2003.  
  
Après avoir défini le niveau fonctionnel du domaine à une certaine valeur, vous ne pouvez pas restaurer ou diminuer le niveau fonctionnel de domaine, avec les exceptions suivantes : lorsque vous augmentez le niveau fonctionnel du domaine à Windows Server 2008 R2 ou Windows Server 2012 et si la fonction de forêt niveau de la NAL est Windows Server 2008 ou, vous avez la possibilité de déployer le niveau fonctionnel du domaine reviennent à Windows Server 2008 ou Windows Server 2008 R2. Vous pouvez réduire le niveau fonctionnel du domaine uniquement à partir de Windows Server 2012 vers Windows Server 2008 R2 ou Windows Server 2008 ou Windows Server 2008 R2 vers Windows Server 2008. Si le niveau fonctionnel du domaine est défini à Windows Server 2008 R2, il ne peut pas être rétabli, par exemple, vers Windows Server 2003.  
  
Pour plus d’informations sur les fonctionnalités qui sont disponibles à des niveaux fonctionnels inférieurs, voir [Présentation des niveaux fonctionnels des services de domaine Active Directory (AD DS)](../active-directory-functional-levels.md).  
  
Au-delà des niveaux fonctionnels, un contrôleur de domaine qui exécute Windows Server 2012 fournit des fonctionnalités supplémentaires qui ne sont pas disponibles sur un contrôleur de domaine qui exécute une version antérieure de Windows Server. Par exemple, un contrôleur de domaine qui exécute Windows Server 2012 peut servir pour le clonage de contrôleur de domaine virtuel, tandis qu’un contrôleur de domaine qui exécute une version antérieure de Windows Server ne peut pas. Mais le clonage de contrôleur de domaine virtuel et de protections de contrôleur de domaine virtuel dans Windows Server 2012 n’ont pas de condition de n’importe quel niveau fonctionnel.  

> [!NOTE]  
> Microsoft Exchange Server 2013 exige un niveau fonctionnel de la forêt de Windows Server 2003 ou version supérieure.  

## <a name="BKMK_ServerRoles"></a>Interopérabilité de service d’annuaire Active Directory avec d’autres rôles de serveur et les systèmes d’exploitation de Windows

Les services de domaine Active Directory ne sont pas pris en charge sur les systèmes d’exploitation Windows suivants :  
  
- Windows MultiPoint Server  
- Windows Server 2012 Essentials  
  
Les services de domaine Active Directory ne peuvent pas être installés sur un serveur qui exécute également les rôles de serveur ou services de rôle suivants :  
  
- Hyper-V Server  
- Service Broker pour les connexions Bureau à distance  
  
## <a name="BKMK_OpsMasters"></a>Rôles de maître d’opérations

Certaines nouvelles fonctionnalités dans Windows Server 2012 affectent les rôles de maître d’opérations :  

- L’émulateur de contrôleur de domaine principal doit exécuter Windows Server 2012 pour prendre en charge le clonage des contrôleurs de domaine virtuel. Des conditions préalables sont requises pour le clonage de contrôleurs de domaine virtuels. Pour plus d’informations, voir [Virtualisation des services de domaine Active Directory (AD DS)](https://technet.microsoft.com/library/hh831734.aspx).  
- Nouvelles entités de sécurité sont créées lorsque l’émulateur de contrôleur de domaine principal exécute Windows Server 2012.  
- Le maître RID dispose d’une nouvelle fonctionnalité d’émission et de surveillance RID. Parmi les améliorations, citons une meilleure journalisation des événements, des limites plus appropriées et la capacité, en cas d’urgence, d’augmenter l’allocation globale du pool RID d’un bit. Pour plus d’informations, voir [Gestion de l’émission RID](../../ad-ds/manage/Managing-RID-Issuance.md).  

> [!NOTE]  
> S’ils ne sont pas des rôles de maître d’opérations, une autre modification de l’installation d’AD DS est que le rôle de serveur DNS et le catalogue global sont installés par défaut sur tous les contrôleurs de domaine qui exécutent Windows Server 2012.  

## <a name="BKMK_Virtual"></a>Virtualisation de contrôleurs de domaine

Améliorations dans début AD DS dans Windows Server 2012 permettent une virtualisation plus fiable des contrôleurs de domaine et la possibilité de cloner des contrôleurs de domaine. Le clonage de contrôleurs de domaine permet quant à lui de déployer rapidement des contrôleurs de domaine supplémentaires dans un nouveau domaine et de profiter d’autres avantages. Pour plus d’informations, consultez [Introduction aux Services de domaine Active Directory &#40;AD DS&#41; virtualisation &#40;niveau 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  

## <a name="BKMK_Admin"></a>Administration de serveurs Windows Server 2012

Utilisez le [à distance serveur outils d’Administration pour Windows 8](https://www.microsoft.com/download/details.aspx?id=28972) pour gérer les contrôleurs de domaine et d’autres serveurs qui exécutent Windows Server 2012. Vous pouvez exécuter les outils Administration de serveur distant de Windows Server 2012 sur un ordinateur qui exécute Windows 8.  

## <a name="BKMK_AppCompat"></a>Compatibilité des applications

Le tableau suivant affiche des applications Microsoft intégrées à Active Directory courantes. Le tableau indique les versions de Windows Server sur lesquelles les applications peuvent être installées et si l’introduction des contrôleurs de domaine Windows Server 2012 a une incidence sur la compatibilité des applications.  

|Produit|Notes|  
|-----------|---------|  
|[Microsoft Configuration Manager 2007](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|Configuration Manager 2007 avec SP2 (inclut Configuration Manager 2007 R2 et Configuration Manager 2007 R3) :<br /><br />-Windows 8 Pro<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter **Remarque :** Même si ces produits sont entièrement pris en charge en tant que clients, il n’est pas prévu d’ajouter une prise en charge pour les déployer en tant que systèmes d’exploitation en utilisant la fonctionnalité de déploiement de système d’exploitation Configuration Manager 2007. Par ailleurs, aucun serveur de site ni système de site ne sera pris en charge sur n’importe quelle référence de Windows Server 2012.|  
|[Microsoft SharePoint 2007](https://support.microsoft.com/kb/2728964)|Microsoft Office SharePoint Server 2007 n’est pas pris en charge pour l’installation sur Windows Server 2012.|  
|[Microsoft SharePoint 2010](https://support.microsoft.com/kb/2724471)|SharePoint 2010 Service Pack 2 est requis pour installer et utiliser <br />SharePoint 2010 sur les serveurs Windows Server 2012<br /><br />SharePoint 2010 Foundation Service Pack 2 est requis pour installer et faire fonctionner SharePoint 2010 Foundation sur les serveurs Windows Server 2012<br /><br />Le processus d’installation de SharePoint Server 2010 (sans les Service Packs) échoue sur Windows Server 2012<br /><br />Le programme d’installation préalable de SharePoint Server 2010 (PrerequisiteInstaller.exe) échoue avec l’erreur « ce programme a des problèmes de compatibilité ». En cliquant sur « Exécuter le programme sans aide » affiche l’erreur « vérification que SharePoint peut être installé &#124; SharePoint Server 2010 (sans les service packs) ne peut pas être installé sur Windows Server 2012. »|  
|[Microsoft SharePoint 2013](https://technet.microsoft.com/library/cc262485(v=office.15).aspx)|Configuration minimale requise pour un serveur de base de données dans une batterie<br /><br />Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Entreprise ou Datacenter, ou édition 64 bits de Windows Server 2012 Standard ou Datacenter<br /><br />Configuration minimale requise pour un serveur unique avec une base de données intégrée :<br /><br />Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Entreprise ou Datacenter, ou édition 64 bits de Windows Server 2012 Standard ou Datacenter<br /><br />Configuration minimale requise pour des serveurs Web frontaux et des serveurs d’applications dans une batterie :<br /><br />Édition 64 bits de Windows Server 2008 R2 Service Pack 1 (SP1) Standard, Entreprise ou Datacenter, ou édition 64 bits de Windows Server 2012 Standard ou Datacenter.|  
|[Microsoft System Center Configuration Manager 2012](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Configuration Manager Service Pack 1 :<br /><br />Microsoft ajoutera les systèmes d’exploitation suivants à notre matrice de prise en charge des clients avec la mise sur le marché du Service Pack 1 :<br /><br />-Windows 8 Pro<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter<br /><br />Tous les rôles de serveur de site (notamment les serveurs de site, les fournisseurs SMS et les points de gestion) peuvent être déployés sur des serveurs avec les éditions des systèmes d’exploitation ci-dessous :<br /><br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[Microsoft Lync Server 2013](https://technet.microsoft.com/library/gg412883.aspx)|Lync Server 2013 requiert Windows Server 2008 R2 ou Windows Server 2012. Il ne peut pas être exécuté sur une installation minimale Il peut être exécuté sur des [serveurs virtuels](https://technet.microsoft.com/library/gg399035.aspx).|  
|[Lync Server 2010](https://support.microsoft.com/kb/2777359)|Lync Server 2010 peut être installé sur une nouvelle installation (et non une installation mise à niveau) de Windows Server 2012 si les [mises à jour cumulatives pour Lync Server datées d’octobre 2012](https://support.microsoft.com/?kbid=2493736) sont installées. La mise à niveau du système d’exploitation vers Windows Server 2012 pour une installation existante de Lync Server 2010 n’est pas prise en charge. Le serveur de conversation de groupe Microsoft Lync Server 2010 n’est pas non plus pris en charge sur Windows Server 2012.|  
|[System Center 2012 Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|System Center 2012 Endpoint Protection Service Pack 1 mettra à jour la matrice de prise en charge des clients pour inclure les systèmes d’exploitation suivants<br /><br />-Windows 8 Pro<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|[System Center 2012 Forefront Endpoint Protection](http://blogs.technet.com/b/configmgrteam/archive/2012/09/10/support-questions-about-windows-8-and-windows-server-2012.aspx)|FEP 2010 avec le correctif cumulatif 1 mettra à jour la matrice de prise en charge des clients pour inclure les systèmes d’exploitation suivants<br /><br />-Windows 8 Pro<br />-Windows 8 entreprise<br />-Windows Server 2012 Standard<br />-Windows Server 2012 Datacenter|  
|Forefront Threat Management Gateway (TMG)|L’exécution de TMG est prise en charge uniquement sur Windows Server 2008 et Windows Server 2008 R2. Pour plus d’informations, voir [Configuration requise pour Forefront TMG](https://technet.microsoft.com/library/dd896981.aspx).|  
|Windows Server Update Services|Cette version de WSUS prend déjà en charge les ordinateurs Windows 8 ou Windows Server 2012 comme clients.|  
|Services WSUS (Windows Server Update Services) 3.0|L’article de mise à jour [2734608](https://support.microsoft.com/kb/2734608) de la Base de connaissances permet aux serveurs qui exécutent WSUS (Windows Server Update Services) 3.0 SP2 de fournir des mises à jour aux ordinateurs qui exécutent Windows 8 ou Windows Server 2012 : **Remarque :** Les clients avec des environnements WSUS 3.0 SP2 autonomes ou des environnements System Center Configuration Manager 2007 Service Pack 2 avec WSUS 3.0 SP2 nécessitent [2734608](https://support.microsoft.com/kb/2734608) pour gérer correctement les ordinateurs Windows 8 ou Windows Server 2012 comme clients.|  
|[Exchange 2013](https://technet.microsoft.com/library/bb691354.aspx)|Les éditions Standard et Datacenter de Windows Server 2012 sont prises en charge pour les rôles suivants : contrôleur de schéma, serveur de catalogue global, contrôleur de domaine, rôle de serveur de boîtes aux lettres et d’accès au client<br /><br />Niveau fonctionnel de forêt : Windows Server 2003 ou version ultérieure<br /><br />Source : configuration requise pour Exchange 2013|  
|Exchange 2010|[Source : Exchange 2010 Service Pack 3](https://blogs.technet.com/b/exchange/archive/2012/09/25/announcing-exchange-2010-service-pack-3.aspx)<br /><br />Exchange 2010 avec Service Pack 3 peut être installé sur les serveurs membres Windows Server 2012.<br /><br />La [configuration requise pour Exchange 2010](https://technet.microsoft.com/library/aa996719(EXCHG.141).aspx) répertorie les derniers contrôleur de schéma, serveur de catalogue global et contrôleur de domaine pris en charge comme Windows Server 2008 R2.<br /><br />Niveau fonctionnel de forêt : Windows Server 2003 ou version ultérieure|  
|SQL Server 2012|Source : Article de la Base de connaissances [2681562](https://support.microsoft.com/kb/2681562)<br /><br />SQL Server 2012 RTM est pris en charge sur Windows Server 2012.|  
|SQL Server 2008 R2|Source : Article de la Base de connaissances [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiert SQL Server 2008 R2 avec Service Pack 1 ou version ultérieure pour une installation sur Windows Server 2012.|  
|SQL Server 2008|Source : Article de la Base de connaissances [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Requiert SQL Server 2008 avec Service Pack 3 ou version ultérieure pour une installation sur Windows Server 2012.|  
|SQL Server 2005|Source : Article de la Base de connaissances [2681562](https://support.microsoft.com/kb/2681562)<br /><br />Pas de prise en charge pour une installation sur Windows Server 2012.|  

## <a name="BKMK_KnownIssues"></a>Problèmes connus

Le tableau suivant répertorie les problèmes connus associés à l’installation des services de domaine Active Directory.  

||||  
|-|-|-|  
|Numéro et titre de l’article de la Base de connaissances|Domaine technologique concerné|Problème/description|  
|[2830145](https://support.microsoft.com/kb/2830145): SID S-1-18-1 et SID S-1-18-2 ne peuvent pas être mappés à des ordinateurs Windows 7 ni Windows Server 2008 R2 dans un environnement de domaine|Gestion des services AD DS/Compatibilité des applications|Les applications qui mappent SID S-1-18-1 et SID S-1-18-2, nouveautés de Windows Server 2012, risquent d'échouer car les identificateurs de sécurité (SID) ne peuvent pas être résolus sur des ordinateurs Windows 7 ni Windows Server 2008 R2. Pour résoudre ce problème, installez le correctif logiciel sur les ordinateurs Windows 7 et Windows Server 2008 R2 du domaine.|  
|[2737129](https://support.microsoft.com/kb/2737129): La préparation des stratégies de groupe n'est pas effectuée quand vous préparez automatiquement un domaine existant pour Windows Server 2012|Installation des services de domaine Active Directory|La commande Adprep /domainprep /gpprep n’est pas exécutée automatiquement dans le cadre de l’installation du premier contrôleur de domaine qui exécute Windows Server 2012 dans un domaine. Si elle n’a jamais été exécutée auparavant dans le domaine, elle doit être exécutée manuellement.|  
|[2737416](https://support.microsoft.com/kb/2737416): Le déploiement de contrôleur de domaine Windows PowerShell multiplie les avertissements|Installation des services de domaine Active Directory|Des avertissements peuvent s’afficher pendant la validation des conditions préalables, puis réapparaître pendant l’installation.|  
|[2737424](https://support.microsoft.com/kb/2737424): L'erreur « Le format du nom de domaine spécifié n'est pas valide » s'affiche quand vous tentez de supprimer les services de domaine Active Directory d'un contrôleur de domaine|Installation des services de domaine Active Directory|Cette erreur s’affiche si vous supprimez le dernier contrôleur d’un domaine dans lequel des comptes de contrôleur de domaine en lecture seule créés au préalable existent toujours. Ce problème concerne Windows Server 2012, Windows Server 2008 R2 et Windows Server 2008.|  
|[2737463](https://support.microsoft.com/kb/2737463): Le contrôleur de domaine ne démarre pas, l'erreur c00002e2 se produit ou le message « Choisir une option » s'affiche|Installation des services de domaine Active Directory|Un contrôleur de domaine ne démarre pas, car un administrateur a utilisé Dism.exe, Pkgmgr.exe ou Ocsetup.exe pour supprimer le rôle DirectoryServices-DomainController.|  
|[2737516](https://support.microsoft.com/kb/2737516): Limites de la vérification de l'installation à partir du support dans le Gestionnaire de serveur Windows Server 2012|Installation des services de domaine Active Directory|La vérification de l’installation à partir du support peut avoir des limites, comme décrit dans l’article de la Base de connaissances.|  
|[2737535](https://support.microsoft.com/kb/2737535): L'applet de commande Install-AddsDomainController retourne une erreur de définition de paramètres pour le contrôleur de domaine en lecture seule|Installation des services de domaine Active Directory|Une erreur peut s’afficher lorsque vous essayez d’attacher un serveur à un compte de contrôleur de domaine en lecture seule si vous spécifiez des arguments qui figurent déjà sur ce compte créé au préalable.|  
|[2737560](https://support.microsoft.com/kb/2737560): Erreur « Impossible d'effectuer un contrôle de conflit de schéma Exchange » et échec de la vérification de la configuration requise|Installation des services de domaine Active Directory|La vérification de la configuration requise échoue lorsque vous configurez le premier contrôleur de domaine Windows Server 2012 dans un domaine existant, car les contrôleurs de domaine n’ont pas le droit d’ouverture de session SeServiceLogonRight pour le service réseau, ou parce que les protocoles WMI ou DCOM sont bloqués.|  
|[2737797](https://support.microsoft.com/kb/2737797): Le module AddsDeployment avec l'argument -Whatif affiche des résultats DNS incorrects|Installation des services de domaine Active Directory|Le paramètre - WhatIf montre DNS server ne sera pas installé, mais il sera.|  
|[2737807](https://support.microsoft.com/kb/2737807): Le bouton Suivant n'est pas disponible dans la page Options du contrôleur de domaine|Installation des services de domaine Active Directory|Le bouton Suivant est désactivé sur la page Options du contrôleur de domaine, car l’adresse IP du contrôleur de domaine cible ne mappe à aucun site ni sous-réseau existant, ou parce que le mot de passe du mode de restauration des services d’annuaire (DSRM) n’est pas tapé et confirmé correctement.|  
|[2737935](https://support.microsoft.com/kb/2737935): L'installation d'Active Directory s'arrête au niveau de l'étape « Création de l'objet Paramètres NTDS »|Installation des services de domaine Active Directory|L’installation se bloque, car le mot de passe Administrateur local correspond au mot de passe Administrateur de domaine, ou parce que des problèmes réseau empêchent l’achèvement de la réplication critique.|  
|[2738060](https://support.microsoft.com/kb/2738060): Le message d'erreur « Accès refusé » s'affiche quand vous créez un domaine enfant à distance en utilisant Install-AddsDomain|Installation des services de domaine Active Directory|Le message d’erreur s’affiche lorsque vous exécutez Install-ADDSDomain avec l’applet de commande Invoke-Command si DNSDelegationCredential a un mot de passe incorrect.|  
|[2738697](https://support.microsoft.com/kb/2738697): Erreur de configuration du contrôleur de domaine « Le serveur n'est pas opérationnel » quand vous configurez un serveur à l'aide du Gestionnaire de serveur|Installation des services de domaine Active Directory|Ce message d’erreur s’affiche lorsque vous essayez d’installer les services de domaine Active Directory sur un ordinateur de groupe de travail, car l’authentification NTLM est désactivée.|  
|[2738746](https://support.microsoft.com/kb/2738746): Vous recevez des erreurs d'accès refusé après avoir ouvert une session sur un compte de domaine d'administrateur local|Installation des services de domaine Active Directory|Lorsque vous ouvrez une session à l’aide d’un compte Administrateur local et non du compte Administrateur intégré, puis créez un domaine, le compte n’est pas ajouté au groupe Administrateurs du domaine.|  
|[2743345](https://support.microsoft.com/kb/2743345): « Le fichier spécifié est introuvable », erreur Adprep /gpprep, ou l'outil se bloque|Installation des services de domaine Active Directory|Ce message d’erreur s’affiche lorsque vous exécutez adprep /gpprep, car le maître d’infrastructure implémente un espace de noms dissocié|  
|[2743367](https://support.microsoft.com/kb/2743367): Erreur Adprep « Pas une application Win32 valide » sur Windows Server 2003, version 64 bits|Installation des services de domaine Active Directory|Ce message d’erreur s’affiche, car la commande Adprep Windows Server 2012 ne peut pas être exécutée sur Windows Server 2003.|  
|[2753560](https://support.microsoft.com/kb/2753560): Erreurs d'installation ADMT 3.2 et PES 3.1 sur Windows Server 2012|ADMT|ADMT 3.2 ne peut pas être installé sur Windows Server 2012 par conception.|  
|[2750857](https://support.microsoft.com/kb/2750857): Les rapports de diagnostic de la réplication DFS ne s'affichent pas correctement dans Internet Explorer 10|Réplication DFS|Le rapport de diagnostic de la réplication DFS ne s’affiche pas correctement en raison de modifications dans Internet Explorer 10.|  
|[2741537](https://support.microsoft.com/kb/2741537): Les mises à jour des stratégies de groupe à distance sont visibles pour les utilisateurs|Stratégie de groupe|Cela est dû à l’exécution de tâches planifiées dans le contexte de chaque utilisateur qui est connecté. La conception du Planificateur de tâches Windows requiert une invite interactive dans ce scénario.|  
|[2741591](https://support.microsoft.com/kb/2741591): Les fichiers ADM ne sont pas présents dans SYSVOL dans l'option de l'état d'infrastructure de la console de gestion des stratégies de groupe (GPMC)|Stratégie de groupe|Réplication de la stratégie de groupe peut signaler « réplication en cours d’exécution », car l’état d’Infrastructure de la console GPMC ne suit pas les règles de filtrage personnalisées.|  
|[2737880](https://support.microsoft.com/kb/2737880): Erreur « Impossible de démarrer le service » lors de la configuration des services de domaine Active Directory|Clonage de contrôleur de domaine virtuel|Ce message d’erreur s’affiche lors de l’installation ou de la suppression des services de domaine Active Directory, ou du clonage, car le service du serveur de rôles DS est désactivé.|  
|[2742836](https://support.microsoft.com/kb/2742836): Deux baux DHCP sont créés pour chaque contrôleur de domaine quand vous utilisez la fonctionnalité de clonage de contrôleur de domaine virtuel|Clonage de contrôleur de domaine virtuel|Cette situation se produit car le contrôleur de domaine cloné a reçu un bail avant le clonage et un autre à la fin du clonage.|  
|[2742844](https://support.microsoft.com/kb/2742844): Le clonage de contrôleur de domaine échoue et le serveur redémarre en mode de restauration des services d'annuaire (DSRM) dans Windows Server 2012|Clonage de contrôleur de domaine virtuel|Le contrôleur de domaine cloné démarre en mode de restauration des services d’annuaire (DSRM), car le clonage a échoué pour l’une des raisons signalées dans l’article de la Base de connaissances.|  
|[2742874](https://support.microsoft.com/kb/2742874): Le clonage de contrôleur de domaine ne recrée pas tous les noms de principal du service|Clonage de contrôleur de domaine virtuel|Certains noms de principal du service en trois parties ne sont pas recréés sur le contrôleur de domaine cloné en raison d’une limitation lorsque le domaine change de nom.|  
|[2742908](https://support.microsoft.com/kb/2742908): Erreur « Aucun serveur d'accès n'est disponible » après le clonage de contrôleur de domaine|Clonage de contrôleur de domaine virtuel|Ce message d’erreur s’affiche lorsque vous essayez d’ouvrir une session après le clonage d’un contrôleur de domaine virtualisé, car le clonage a échoué et le contrôleur de domaine a démarré en mode de restauration des services d’annuaire (DSRM). Ouvrez une session sous le nom .\administrateur pour résoudre le problème de clonage.|  
|[2742916](https://support.microsoft.com/kb/2742916): Le clonage de contrôleur de domaine échoue avec l'erreur 8610 dans dcpromo.log|Clonage de contrôleur de domaine virtuel|Le clonage échoue, car l’émulateur de contrôleur de domaine principal (PDC) n’a pas effectué de réplication entrante de la partition de domaine, probablement parce que le rôle a été transféré.|  
|[2742927](https://support.microsoft.com/kb/2742927): Erreur New-AdDcCloneConfig « L'index était hors limites »|Clonage de contrôleur de domaine virtuel|L’erreur s’affiche une fois que vous avez exécuté l’applet de commande New-ADDCCloneConfigFile lors du clonage de contrôleurs de domaine virtuels, parce que l’applet de commande n’a pas été exécutée à partir d’une invite de commandes avec élévation de privilèges ou parce que votre jeton d’accès ne contient pas le groupe Administrateurs.|  
|[2742959](https://support.microsoft.com/kb/2742959): Le clonage de contrôleur de domaine échoue avec l’erreur 8437 : « Un paramètre non valide a été spécifié pour cette opération de réplication »|Clonage de contrôleur de domaine virtuel|Le clonage a échoué parce qu’un nom de clone non valide ou un nom NetBIOS dupliqué a été spécifié.|  
|[2742970](https://support.microsoft.com/kb/2742970): Le clonage de contrôleur de domaine échoue sans mode de restauration des services d'annuaire (DSRM), source dupliquée ni ordinateur clone|Clonage de contrôleur de domaine virtuel|Le contrôleur de domaine virtuel cloné démarre en mode de restauration des services d’annuaire (DSRM), en utilisant un nom dupliqué comme contrôleur de domaine source, car le fichier DCCloneConfig.xml n’a pas été créé à l’emplacement approprié ou parce que le contrôleur de domaine source a été redémarré avant le clonage.|  
|[2743278](https://support.microsoft.com/kb/2743278): Erreur de clonage de contrôleur de domaine 0x80041005|Clonage de contrôleur de domaine virtuel|Le contrôleur de domaine cloné démarre en mode de restauration des services d’annuaire (DSRM) car un seul serveur WINS a été spécifié. Si un serveur WINS est spécifié, des serveurs WINS préféré et auxiliaire doivent tous deux être spécifiés.|  
|[2745013](https://support.microsoft.com/kb/2745013): Message d'erreur « Le serveur n'est pas opérationnel » si vous exécutez New-AdDcCloneConfigFile dans Windows Server 2012|Clonage de contrôleur de domaine virtuel|Ce message d’erreur s’affiche une fois que vous avez exécuté l’applet de commande New-ADDCCloneConfigFile, car le serveur ne peut pas contacter de serveur de catalogue global.|  
|[2747974](https://support.microsoft.com/kb/2747974): L'événement de clonage de contrôleur de domaine 2224 fournit des instructions incorrectes|Clonage de contrôleur de domaine virtuel|L’ID d’événement 2224 indique de façon incorrecte que les comptes de service administrés doivent être supprimés avant le clonage. Les comptes de service administrés autonomes doivent être supprimés, mais les comptes de service administrés de groupe ne bloquent pas le clonage.|  
|[2748266](https://support.microsoft.com/kb/2748266) : Vous ne pouvez pas déverrouiller un lecteur chiffré par BitLocker après la mise à niveau vers Windows 8|BitLocker|Vous recevez une erreur « Application introuvable » lorsque vous essayez de déverrouiller un lecteur sur un ordinateur qui a été mis à niveau à partir de Windows 7.|  

## <a name="see-also"></a>Voir aussi

[Ressources d’évaluation de Windows Server 2012](https://technet.microsoft.com/evalcenter/hh708766.aspx)  
[Guide d’évaluation de Windows Server 2012](https://download.microsoft.com/download/5/B/2/5B254183-FA53-4317-B577-7561058CEF42/WS%202012%20Evaluation%20Guide.pdf)  
[Installer et déployer Windows Server 2012](https://technet.microsoft.com/library/hh831620.aspx)  
