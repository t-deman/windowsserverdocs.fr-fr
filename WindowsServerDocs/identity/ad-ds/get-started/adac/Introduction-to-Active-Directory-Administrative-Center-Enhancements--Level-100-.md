---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduction to Active Directory Administrative Center Enhancements (Level 100)
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d3b5f3eee338128fb424c28e7280bb7f543c9f58
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443000"
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduction to Active Directory Administrative Center Enhancements (Level 100)

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le centre d’administration Active Directory dans Windows Server inclut des fonctionnalités de gestion pour les éléments suivants :

- [Corbeille Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)
- [Stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)
- [Afficheur d’historique PowerShell Windows](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Corbeille Active Directory

La suppression accidentelle des objets Active Directory est un élément qui affecte souvent les utilisateurs des services de domaine Active Directory (AD DS) et des services AD LDS (Active Directory Lightweight Directory Services). Dans les versions antérieures de Windows Server, avant Windows Server 2008 R2, il pourrait possible de récupérer des objets supprimés accidentellement dans Active Directory, mais les solutions avaient leurs inconvénients.

Dans Windows Server 2008, vous pouviez utiliser la fonctionnalité Sauvegarde Windows Server et la commande de restauration faisant autorité **ntdsutil** pour marquer des objets comme faisant autorité, afin de garantir que les données restaurées soient répliquées sur l’ensemble du domaine. L’inconvénient de la solution de restauration faisant autorité était qu’elle devait être effectuée en mode de restauration des services d’annuaire. Dans ce mode, le contrôleur de domaine en cours de restauration devait rester hors connexion. Il n’était dès lors pas en mesure de traiter les demandes des clients.

Dans Active Directory sous Windows Server 2003 et dans les services AD DS sous Windows Server 2008, il était possible de récupérer les objets Active Directory supprimés par le biais de la réanimation des désactivations. Toutefois, les attributs à valeur liée des objets réanimés (par exemple, les appartenances de groupe des comptes d’utilisateur) qui étaient supprimés physiquement et les attributs à valeur non liée qui étaient supprimés n’étaient pas récupérés Par conséquent, les administrateurs ne pouvaient pas se fier à la réanimation des désactivations comme solution suprême à la suppression accidentelle d’objets. Pour plus d’informations sur la réanimation des désactivations, voir [Réanimation des objets de désactivation Active Directory](https://go.microsoft.com/fwlink/?LinkID=125452).

La corbeille Active Directory, introduite pour la première fois dans Windows Server 2008 R2, s’appuie sur l’infrastructure existante de réanimation des désactivations et améliore votre capacité à préserver et récupérer les objets Active Directory accidentellement supprimés.

Lorsque vous activez la corbeille Active Directory, tous les attributs à valeur liée et à valeur non liée des objets Active Directory supprimés sont préservés et les objets sont restaurés dans leur intégralité dans le même état logique et cohérent qu’ils avaient juste avant la suppression. Par exemple, les comptes d’utilisateurs restaurés retrouvent automatiquement toutes les appartenances aux groupes et les droits d’accès correspondants dont ils disposaient juste avant la suppression, dans et entre les domaines. La corbeille Active Directory fonctionne pour les environnements AD DS et AD LDS. Pour obtenir une description détaillée de la Corbeille Active Directory, consultez [quelles sont les nouveautés dans AD DS : Corbeille Active Directory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**Nouveautés ?** Dans Windows Server 2012 et versions ultérieures, la fonctionnalité Corbeille Active Directory a été améliorée avec une nouvelle interface utilisateur graphique permettant aux utilisateurs de gérer et restaurer des objets supprimés. Les utilisateurs peuvent à présent localiser visuellement une liste d’objets supprimés et les restaurer à leur emplacement d’origine ou à un emplacement souhaité.

Si vous prévoyez d’activer la Corbeille Active Directory dans Windows Server, procédez comme suit :

- Par défaut, la corbeille Active Directory est désactivée. Pour l’activer, vous devez tout d’abord élever le niveau fonctionnel de forêt de votre environnement AD DS ou AD LDS vers Windows Server 2008 R2 ou version ultérieure. Ceci à son tour, exige que tous les contrôleurs de domaine dans la forêt ou tous les serveurs qui hébergent des instances de jeux de configuration AD LDS exécutant Windows Server 2008 R2 ou version ultérieure.
- Le processus d’activation de la corbeille Active Directory est irréversible. Après avoir activé la corbeille Active Directory dans votre environnement, vous ne pouvez plus la désactiver.
- Pour gérer la fonctionnalité Corbeille via une interface utilisateur, vous devez installer la version du centre d’administration Active Directory dans Windows Server 2012.

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer le serveur Administration outils distant (RSAT) pour utiliser la version correcte du centre d’administration Active Directory pour gérer la Corbeille via une interface utilisateur.
    >
    > Pour plus d’informations sur l’installation de serveur distant, consultez l’article [outils d’Administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="active-directory-recycle-bin-step-by-step"></a>Procédure pas à pas relative à la corbeille Active Directory

Dans les étapes suivantes, vous allez utiliser ADAC pour effectuer les tâches suivantes de la Corbeille Active Directory dans Windows Server 2012 :

- [Étape 1 : Augmenter le niveau fonctionnel de forêt](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)
- [Étape 2 : Activer la Corbeille](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)
- [Étape 3 : Créer des utilisateurs de test, de groupe et d’unité d’organisation](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)
- [Étape 4 : Restaurer des objets supprimés](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> Pour effectuer les étapes suivantes, vous devez appartenir au groupe Administrateurs de l’entreprise ou posséder des autorisations équivalentes.

### <a name="bkmk_raise_ffl"></a>Étape 1 : augmenter le niveau fonctionnel de la forêt

Dans cette étape, vous allez augmenter le niveau fonctionnel de la forêt. Vous devez tout d’abord élever le niveau fonctionnel de la forêt cible à Windows Server 2008 R2 au minimum avant d’activer la Corbeille Active Directory.

#### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Pour augmenter le niveau fonctionnel sur la forêt cible

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Cliquez sur le domaine cible dans le volet de navigation gauche et, dans le volet **Tâches** , cliquez sur **Augmenter le niveau fonctionnel de la forêt**. Sélectionnez un niveau fonctionnel de forêt qui est au moins Windows Server 2008 R2 ou version ultérieure, puis **OK**.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Pour le **-identité** argument, spécifiez le nom de domaine DNS complet.

### <a name="bkmk_enable_recycle_bin"></a>Étape 2 : Activer la Corbeille

Dans cette étape, vous allez activer la corbeille pour restaurer des objets supprimés dans AD DS.

#### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Pour activer la corbeille Active Directory dans le Centre d’administration Active Directory du domaine cible

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Dans le volet **Tâches**, cliquez sur **Activer la Corbeille...** dans le volet **Tâches**, cliquez sur **OK** dans la boîte de message d’avertissement, puis cliquez sur **OK** dans le message d’actualisation du Centre d’administration Active Directory.

4. Appuyez sur F5 pour actualiser le Centre d’administration Active Directory.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Étape 3 : créer des utilisateurs, un groupe et une unité d’organisation de test

Dans les procédures suivantes, vous allez créer deux utilisateurs test. Vous créerez ensuite un groupe test et lui ajouterez les utilisateurs test. Vous créerez également une unité d’organisation (OU).

#### <a name="to-create-test-users"></a>Pour créer les utilisateurs test

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Dans le volet **Tâches** , cliquez sur **Nouveau** , puis sur **Utilisateur**.

    ![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4. Entrez les informations suivantes sous **Compte** , puis cliquez sur OK :

   - Nom complet : test1
   - Ouverture de session SamAccountName de l’utilisateur : test1
   - Mot de passe : p@ssword1
   - Confirmez le mot de passe : p@ssword1

5. Répétez les étapes précédentes pour créer un second utilisateur, test2.

#### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Pour créer un groupe test et lui ajouter les utilisateurs

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.
2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.
3. Dans le volet **Tâches** , cliquez sur **Nouveau** , puis sur **Groupe**.
4. Entrez les informations suivantes sous **Groupe**, puis cliquez sur **OK** :

    -   **Nom de groupe : groupe 1**

5. Cliquez sur **group1**, puis, sous le volet **Tâches** , cliquez sur **Propriétés**.
6. Cliquez sur **Membres**et sur **Ajouter**, tapez **test1;test2**, puis cliquez sur **OK**.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Add-ADGroupMember -Identity group1 -Member test1
```

#### <a name="to-create-an-organizational-unit"></a>Pour créer une unité d’organisation

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.
2. Cliquez sur **gérer**, cliquez sur **ajouter des nœuds de Navigation** et sélectionnez le domaine cible approprié dans le **ajouter des nœuds de Navigation** boîte de dialogue, puis cliquez sur ** OK
3. Dans le volet **Tâches**, cliquez sur **Nouveau**, puis sur **Unité d’organisation**.
4. Entrez les informations suivantes sous **Unité d’organisation**, puis cliquez sur **OK** :

   - **NameOU1**

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"
```

### <a name="bkmk_restore_del_obj"></a>Étape 4 : restaurer les objets supprimés

Dans les procédures suivantes, vous allez restaurer des objets supprimés à partir du conteneur **Objets supprimés** à leur emplacement d’origine et à un emplacement différent.

#### <a name="to-restore-deleted-objects-to-their-original-location"></a>Pour restaurer des objets supprimés à leur emplacement d’origine

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Sélectionnez les utilisateurs **test1** et **test2**, cliquez sur **Supprimer** dans le volet **Tâches** , puis cliquez sur **Oui** pour confirmer la suppression.

    ![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

    L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

    ```powershell
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4. Accédez au conteneur **Objets supprimés**, sélectionnez **test2** et **test1**, puis cliquez sur **Restaurer** dans le volet **Tâches**.

5. Pour confirmer que les objets ont été restaurés à leur emplacement d’origine, accédez au domaine cible et vérifiez que les comptes d’utilisateurs sont répertoriés.

    > [!NOTE]
    > Si vous accédez aux **Propriétés** des comptes d’utilisateurs **test1** et **test2** et que vous cliquez sur **Membre de**, vous verrez que leur appartenance aux groupes a été restaurée également.

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

#### <a name="to-restore-deleted-objects-to-a-different-location"></a>Pour restaurer des objets supprimés à un emplacement différent

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Sélectionnez les utilisateurs **test1** et **test2**, cliquez sur **Supprimer** dans le volet **Tâches** , puis cliquez sur **Oui** pour confirmer la suppression.

4. Accédez au conteneur **Objets supprimés**, sélectionnez **test2** et **test1**, puis cliquez sur **Restaurer sur** dans le volet **Tâches**.

5. Sélectionnez **OU1** , puis cliquez sur **OK**.

6. Pour confirmer que les objets ont été restaurés sur **OU1**, accédez au domaine cible, double-cliquez sur **OU1** et vérifiez que les comptes d’utilisateurs sont répertoriés.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Stratégie de mot de passe affinées

Le système d’exploitation Windows Server 2008 fournit aux organisations une méthode pour définir des stratégies de mot de passe et de verrouillage de compte différentes pour des ensembles d’utilisateurs différents dans un domaine. Dans les domaines Active Directory antérieurs à Windows Server 2008, une seule stratégie de mot de passe et stratégie de verrouillage de compte pouvait être appliquée à tous les utilisateurs dans le domaine. Ces stratégies étaient spécifiées dans la stratégie de domaine par défaut pour le domaine. Par conséquent, les organisations qui voulaient des paramètres différents de mot de passe et de verrouillage de compte pour des ensembles d’utilisateurs différents devaient créer un filtre de mot de passe ou déployer plusieurs domaines. Ces deux options sont onéreuses.

Vous pouvez utiliser des stratégies de mot de passe affinées pour spécifier plusieurs stratégies de mot de passe au sein d’un même domaine et appliquer des restrictions différentes pour les stratégies de mot de passe et de verrouillage de compte à des ensembles d’utilisateurs différents dans un domaine. Par exemple, vous pouvez appliquer des paramètres plus stricts à des comptes privilégiés et des paramètres moins stricts aux comptes des autres utilisateurs. Dans d’autres cas, vous pouvez appliquer une stratégie de mot de passe spéciale pour les comptes dont les mots de passe sont synchronisés avec d’autres sources de données. Pour obtenir une description détaillée de la stratégie de mot de passe affinée, voir [AD DS : Stratégies de mot de passe affinées](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**Nouveautés ?**

Dans Windows Server 2012 et le mot de passe affinée plus récente gestion des stratégies est effectuée plus facile et plus visuelle en fournissant une interface utilisateur pour les administrateurs de services AD DS pour les gérer dans ADAC. Les administrateurs peuvent maintenant afficher la stratégie résultante d’un utilisateur donné, afficher et trier toutes les stratégies de mot de passe dans un domaine donné et gérer visuellement les stratégies de mot de passe individuels.

Si vous envisagez d’utiliser des stratégies de mot de passe affinées dans Windows Server 2012, considérez les points suivants :

- Les stratégies de mot de passe affinées appliquent uniquement des objets utilisateur et des groupes de sécurité globaux (ou des objets inetOrgPerson s’ils sont utilisés à la place des objets utilisateur). Par défaut, seuls les membres du groupe Admins du domaine peuvent définir des stratégies de mot de passe affinées. Toutefois, vous pouvez également déléguer la capacité à définir de telles stratégies à d’autres utilisateurs. Le niveau fonctionnel du domaine doit correspondre à Windows Server 2008 ou version supérieure.

- Vous devez utiliser le Windows Server 2012 ou une version plus récente du centre d’administration Active Directory pour administrer les stratégies de mot de passe affinées via une interface utilisateur graphique.

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer le serveur Administration outils distant (RSAT) pour utiliser la version correcte du centre d’administration Active Directory pour gérer la Corbeille via une interface utilisateur.
    >
    > Pour plus d’informations sur l’installation de serveur distant, consultez l’article [outils d’Administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="fine-grained-password-policy-step-by-step"></a>Procédure pas à pas relative aux stratégies de mot de passe affinées

Dans les étapes suivantes, vous utiliserez le Centre d’administration Active Directory pour effectuer les tâches suivantes relatives aux stratégies de mot de passe affinées :

- [Étape 1 : Augmenter le niveau fonctionnel de domaine](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)
- [Étape 2 : Créer des utilisateurs de test, de groupe et d’unité d’organisation](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)
- [Étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)
- [Étape 4 : Afficher un jeu de stratégies pour un utilisateur résultant](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)
- [Étape 5 : Modifier une stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)
- [Étape 6 : Supprimer une stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Pour effectuer les étapes suivantes, vous devez appartenir au groupe Admins du domaine ou posséder des autorisations équivalentes.

#### <a name="bkmk_raise_dfl"></a>Étape 1 : augmenter le niveau fonctionnel du domaine

Dans la procédure suivante, vous allez augmenter le niveau fonctionnel du domaine du domaine cible vers Windows Server 2008 ou version ultérieure. Un niveau fonctionnel du domaine de Windows Server 2008 ou version ultérieure est nécessaire pour activer les stratégies de mot de passe affinées.

##### <a name="to-raise-the-domain-functional-level"></a>Pour augmenter le niveau fonctionnel du domaine

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Cliquez sur le domaine cible dans le volet de navigation gauche et, dans le volet **Tâches** , cliquez sur **Augmenter le niveau fonctionnel du domaine**. Sélectionnez un niveau fonctionnel de forêt qui est au moins Windows Server 2008 ou version ultérieure, puis cliquez sur **OK**.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Étape 2 : créer des utilisateurs, un groupe et une unité d’organisation de test

Pour créer les utilisateurs de test et le groupe requis pour cette étape, suivez les procédures situés ici : [Étape 3 : Créer des utilisateurs de test, de groupe et d’unité d’organisation](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (vous n’avez pas besoin créer l’unité d’organisation pour illustrer une stratégie de mot de passe affinée).

#### <a name="bkmk_create_fgpp"></a>Étape 3 : créer une nouvelle stratégie de mot de passe affinée

Dans la procédure décrite ci-dessous, vous allez créer une nouvelle stratégie de mot de passe affinée à l’aide de l’interface utilisateur du Centre d’administration Active Directory.

##### <a name="to-create-a-new-fine-grained-password-policy"></a>Pour créer une nouvelle stratégie de mot de passe affinée

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Dans le volet de navigation du Centre d’administration Active Directory, ouvrez le conteneur **System**, puis cliquez sur **Password Settings Container**.

4. Dans le volet **Tâches** , cliquez sur **Nouveau**, puis sur **Paramètres de mot de passe**.

    Remplissez ou modifiez les champs de la page de propriétés pour créer un nouvel objet **Paramètres de mot de passe**. Les champs **Nom** et **Priorité** sont requis.

    ![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5. Sous **S’applique directement à**, sur **Ajouter**, tapez **group1**, puis cliquez sur **OK**.

    Cela associe l’objet Stratégie de mot de passe aux membres du groupe global que vous avez créé pour l’environnement de test.

6. Cliquez sur **OK** pour soumettre la création.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Étape 4 : afficher un jeu de stratégies résultant pour un utilisateur

Dans la procédure suivante, vous allez consulter les paramètres de mot de passe résultants pour un utilisateur qui est membre du groupe auquel vous avez attribué une stratégie de mot de passe affinée dans [étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

##### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Pour afficher un jeu de stratégies résultant pour un utilisateur

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Sélectionnez un utilisateur, **test1** qui appartient au groupe, **group1** que vous avez associé une stratégie de mot de passe affinées dans [étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4. Cliquez sur **Afficher les paramètres de mot de passe résultants** dans le volet **Tâches** .

5. Examinez la stratégie des paramètres de mot de passe, puis cliquez sur **Annuler**.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Étape 5 : modifier une stratégie de mot de passe affinée

Dans la procédure suivante, vous allez modifier la stratégie de mot de passe affinée que vous avez créé dans [étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

##### <a name="to-edit-a-fine-grained-password-policy"></a>Pour modifier une stratégie de mot de passe affinée

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Dans le **volet de navigation** du Centre d’administration Active Directory, développez **Système**, puis cliquez sur **Classe d’objets PSC** (Password Settings Container).

4. Sélectionnez la stratégie de mot de passe affinée que vous avez créé dans [étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) et cliquez sur **propriétés** dans le **tâches** volet.

5. Sous **Appliquer l’historique des mots de passe**, modifiez la valeur de **Nombre de mots de passe mémorisés** en spécifiant **30**.

6. Cliquez sur **OK**.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Étape 6 : supprimer une stratégie de mot de passe affinée

##### <a name="to-delete-a-fine-grained-password-policy"></a>Pour supprimer une stratégie de mot de passe affinée

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Dans le volet de navigation du Centre d’administration Active Directory, développez **Système** , puis cliquez sur **Classe d’objets PSC (Password Settings Container)** .

4. Sélectionnez la stratégie de mot de passe affinée que vous avez créé dans [étape 3 : Créer une nouvelle stratégie de mot de passe affinées](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) et dans le **tâches** volet, cliquez sur **propriétés**.

5. Désactivez la case à cocher **Protéger contre la suppression accidentelle** et cliquez sur **OK**.

6. Sélectionnez la stratégie de mot de passe affinée, puis, dans le volet **Tâches** , cliquez sur **Supprimer**.

7. Cliquez sur **OK** dans la boîte de dialogue de confirmation.

![Introduction au centre d’administration d’AD](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)***<em>commandes Windows PowerShell équivalentes</em>***

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

```powershell
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Afficheur d’historique PowerShell Windows

Le Centre d’administration Active Directory est un outil d’interface utilisateur construit par-dessus Windows PowerShell. Dans Windows Server 2012 et versions ultérieures, les administrateurs informatiques peuvent tirer parti de ADAC pour en savoir plus Windows PowerShell pour les applets de commande Active Directory à l’aide de la visionneuse de l’historique de Windows PowerShell. Lorsque des actions sont exécutées dans l’interface utilisateur, la commande Windows PowerShell équivalente est indiquée à l’utilisateur dans la Visionneuse de l’historique de Windows PowerShell. Cela permet aux administrateurs de créer des scripts automatisés et de réduire les tâches répétitives, ce qui a pour effet d’augmenter la productivité informatique. En outre, cette fonctionnalité réduit le temps d’apprentissage de Windows PowerShell pour Active Directory et augmente la confiance des utilisateurs dans l’exactitude de leurs scripts d’automatisation.

Lorsque vous utilisez la visionneuse de l’historique de Windows PowerShell dans Windows Server 2012 ou versions ultérieures, considérez les points suivants :

- Pour utiliser la visionneuse de scripts Windows PowerShell, vous devez utiliser le Windows Server 2012 ou une version plus récente du centre d'

    > [!NOTE]
    > Vous pouvez utiliser **le Gestionnaire de serveur** pour installer le serveur Administration outils distant (RSAT) pour utiliser la version correcte du centre d’administration Active Directory pour gérer la Corbeille via une interface utilisateur.
    >
    > Pour plus d’informations sur l’installation de serveur distant, consultez l’article [outils d’Administration de serveur distant](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

- Avoir une connaissance élémentaire de Windows PowerShell. Par exemple, vous devez savoir comment les pipelines fonctionnent dans Windows PowerShell. Pour plus d’informations sur les pipelines dans Windows PowerShell, voir [Définition et utilisation des pipelines dans Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Procédure pas à pas relative à la Visionneuse de l’historique de Windows PowerShell

Dans la procédure décrite ci-dessous, vous utiliserez la Visionneuse de l’historique de Windows PowerShell dans le Centre d’administration Active Directory pour élaborer un script Windows PowerShell.  Avant de commencer cette procédure, supprimez l’utilisateur **test1** du groupe **group1**.

#### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Pour élaborer un script à l’aide de la Visionneuse de l’historique de PowerShell

1. Cliquez avec le bouton droit sur l’icône de Windows PowerShell, cliquez sur **exécuter en tant qu’administrateur** et type **dsac.exe** pour ouvrir ADAC.

2. Cliquez sur **Gérer**, sur **Ajouter des nœuds de navigation**, puis sélectionnez le domaine cible approprié dans la boîte de dialogue **Ajouter des nœuds de navigation** et cliquez sur **OK**.

3. Développez le volet **Historique de Windows PowerShell** en bas de l’écran du Centre d’administration Active Directory.

4. Sélectionnez l’utilisateur **test1**.

5. Cliquez sur **ajouter au groupe...**  dans le **tâches** volet.

6. Accédez à **group1** et cliquez sur **OK** dans la boîte de dialogue.

7. Accédez au volet **Historique de Windows PowerShell** et localisez la commande qui vient d’être générée.

8. Copiez cette commande et collez-la dans l’éditeur de votre choix pour élaborer votre script.

    Par exemple, vous pouvez modifier la commande pour ajouter un autre utilisateur dans **group1** ou pour ajouter **test1** dans un autre groupe.

## <a name="see-also"></a>Voir aussi

[Avancées de gestion AD DS à l’aide d’Active Directory Administrative Center &#40;niveau 200&#41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)
