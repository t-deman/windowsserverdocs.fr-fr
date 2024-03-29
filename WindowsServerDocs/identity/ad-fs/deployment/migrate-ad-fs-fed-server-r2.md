---
title: Migration du serveur de fédération 2.0 AD FS
description: Fournit des informations sur la migration d’un serveur AD FS vers Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 46d0b643d786443093e0dfeafe4cddde3278ff76
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444622"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrer le serveur de fédération 2.0 AD FS à AD FS sur Windows Server 2012 R2

Pour migrer un serveur de fédération AD FS qui appartient à une batterie de serveurs AD FS à nœud unique, une batterie WIF ou une batterie de serveurs SQL Server vers Windows Server 2012 R2, vous devez effectuer les tâches suivantes :  
  
1.  [Exporter et sauvegarder les données de configuration AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Créer une batterie de serveurs de fédération Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importer les données de configuration d’origine dans la batterie de serveurs Windows Server 2012 R2 AD FS](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exporter et sauvegarder les données de configuration AD FS  
 Pour exporter les paramètres de configuration AD FS, effectuez les procédures suivantes :  
  
###  <a name="to-export-service-settings"></a>Pour exporter les paramètres de service  
  
1.  Assurez-vous que vous avez accès aux certificats suivants et à leurs clés privées dans un fichier .pfx :  
  
    -   certificat SSL utilisé par la batterie de serveurs de fédération à migrer ;  
  
    -   certificat de communications de service (s’il est différent du certificat SSL) utilisé par la batterie de serveurs de fédération à migrer ;  
  
    -   tous les certificats de chiffrement/déchiffrement de jeton ou de signature de jetons de partie tiers utilisés par la batterie de serveurs de fédération à migrer.  
  
Pour rechercher le certificat SSL, ouvrez la console de gestion des services Internet (IIS), sélectionnez **Site Web par défaut** dans le volet gauche, cliquez sur **Liaisons…** dans le volet **Actions** , recherchez et sélectionnez la liaison https, cliquez sur **Modifier**, puis sur **Afficher**.  
  
Vous devez exporter le certificat SSL utilisé par le service de fédération et sa clé privée vers un fichier .pfx. Pour plus d’informations, voir [Exporter la partie clé privée d’un certificat d’authentification serveur](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Si vous envisagez de déployer le Service d’inscription de périphérique dans le cadre de l’exécution des services AD FS dans Windows Server 2012 R2, vous devez obtenir un nouveau certificat SSL. Pour plus d'informations, consultez [Enroll an SSL Certificate for AD FS](enroll-an-ssl-certificate-for-ad-fs.md) et [Configure a federation server with Device Registration Service](configure-a-federation-server-with-device-registration-service.md).  
  
Pour afficher les certificats de signature de jetons, de déchiffrement de jeton et de communications de service qui sont utilisés, exécutez la commande Windows PowerShell suivante pour créer une liste de tous les certificats utilisés dans un fichier :  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2. Exportez les propriétés du service de fédération AD FS, telles que le nom du service de fédération, le nom complet du service de fédération et l’identificateur du serveur de fédération dans un fichier.  
  
Pour exporter les propriétés du service de fédération, ouvrez Windows PowerShell et exécutez la commande suivante : 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

Le fichier de sortie doit contenir les valeurs de configuration importantes suivantes :  
 
|**Nom de propriété du Service de fédération comme indiqué par Get-ADFSProperties**|**Nom de propriété du Service de fédération dans la console de gestion AD FS**|
|-----|-----|  
|HostName|Nom du service de fédération|  
|Identificateur|Identificateur du service de fédération|  
|DisplayName|Nom complet du service de fédération|  
  
3. Sauvegardez le fichier de configuration de l’application. Parmi d’autres paramètres, ce fichier contient la chaîne de connexion à la base de données de stratégies.  
  
Pour sauvegarder le fichier de configuration de l’application, vous devez copier manuellement le fichier `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` à un emplacement sécurisé sur un serveur de sauvegarde.  
  
> [!NOTE]
>  Notez la chaîne de connexion à la base de données dans ce fichier, située immédiatement après "policystore connectionstring=". Si la chaîne de connexion spécifie une base de données SQL Server, la valeur est nécessaire lors de la restauration de la configuration AD FS d’origine sur le serveur de fédération.  
>   
>  Voici un exemple de chaîne de connexion WID : `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Voici un exemple de chaîne de connexion SQL Server : `"Data Source=databasehostname;Integrated Security=True"`.  
  
4. Enregistrez l’identité du compte de service de fédération AD FS et le mot de passe de ce compte.  
  
Pour rechercher la valeur d’identité, examinez la colonne **Ouvrir une session en tant que** de **Service Windows AD FS 2.0** dans la console des **services** et enregistrez manuellement cette valeur.  
  
> [!NOTE]
>  Pour un service de fédération autonome, le compte NETWORK SERVICE intégré est utilisé.  Dans ce cas, vous n’avez pas besoin de mot de passe.  
  
5. Exportez la liste des points de terminaison AD FS activés dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante : 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6. Exportez toutes les descriptions des revendications personnalisées dans un fichier.  
  
Pour ce faire, ouvrez Windows PowerShell et exécutez la commande suivante : 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7. Si vous disposez de paramètres personnalisés comme useRelayStateForIdpInitiatedSignOn configurés dans le fichier web.config, veillez à sauvegarder le fichier web.config à titre de référence. Vous pouvez copier le fichier à partir du répertoire qui est mappé au chemin d’accès virtuel **/adfs/ls** dans les services Internet (IIS). L'emplacement par défaut est le répertoire **%systemdrive%\inetpub\adfs\ls** .  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Pour exporter les approbations de fournisseur de revendications et approbations de partie de confiance  
  
1.  Pour exporter AD FS approbations de fournisseur de revendications et approbations de partie de confiance, vous devez vous connecter en tant qu’administrateur (Toutefois, pas en tant qu’administrateur de domaine) sur votre fédération, serveur et exécuter la commande Windows PowerShell suivante script qui est situé dans le **media / server_support/adfs** dossier du CD d’installation de Windows Server 2012 R2 : `export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  Le script d’exportation accepte les paramètres suivants :  
> 
> - Export-FederationConfiguration.ps1-chemin d’accès < chaîne\> [-ComputerName < chaîne\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>]  
>   -   Export-FederationConfiguration.ps1-chemin d’accès < chaîne\> [-ComputerName < chaîne\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >]  
>   -   Export-FederationConfiguration.ps1-chemin d’accès < chaîne\> [-ComputerName < chaîne\>] [-Credential < pscredential\>] [-Force] [-CertificatePassword < securestring\>] [- RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>**  : l’applet de commande exporte uniquement les approbations de partie de confiance dont les identificateurs sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de partie de confiance n’est exportée. Si aucun des éléments RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName et ClaimsProviderTrustName n’est spécifié, le script exporte toutes les approbations de partie de confiance et approbations de fournisseur de revendications.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>**  : l’applet de commande exporte uniquement les approbations de fournisseur de revendications dont les identificateurs sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de fournisseur de revendications n’est exportée.  
> 
>   **-RelyingPartyTrustName <string[]>**  : l’applet de commande exporte uniquement les approbations de partie de confiance dont les noms sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de partie de confiance n’est exportée.  
> 
>   **-ClaimsProviderTrustName <string[]>**  : l’applet de commande exporte uniquement les approbations de fournisseur de revendications dont les noms sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de fournisseur de revendications n’est exportée.  
> 
>   **-Path < chaîne\>**  -le chemin d’accès à un dossier qui contiendra les fichiers exportés.  
> 
>   **-ComputerName < chaîne\>**  -Spécifie le nom d’hôte de serveur STS. La valeur par défaut est l'ordinateur local. Si vous migrez AD FS 2.0 ou AD FS dans Windows Server 2012 vers AD FS dans Windows Server 2012 R2, il s’agit du nom d’hôte du serveur AD FS hérité.  
> 
>   **-Credential < PSCredential\>**  -spécifie un compte d’utilisateur qui a l’autorisation d’effectuer cette action. La valeur par défaut est l’utilisateur actuel.  
> 
>   **-Force** : spécifie de ne pas demander de confirmation de l’utilisateur.  
> 
>   **-CertificatePassword < SecureString\>**  -spécifie un mot de passe pour l’exportation des clés privées des certificats AD FS. Si cette valeur n’est pas spécifiée, le script demande un mot de passe si un certificat AD FS avec une clé privée doit être exporté.  
> 
>   **Entrées** : Aucune  
> 
>   **Sorties** : chaîne. Cette applet de commande retourne le chemin d’accès du dossier d’exportation. Vous pouvez diriger l’objet retourné vers Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Pour sauvegarder les magasins d’attributs personnalisés  
  
1.  Vous devez exporter manuellement tous les magasins d’attributs personnalisés que vous souhaitez conserver dans votre nouvelle batterie AD FS dans Windows Server 2012 R2.  
  
> [!NOTE]
>  Dans Windows Server 2012 R2, AD FS requiert des magasins d’attributs personnalisés qui sont basées sur .NET Framework 4.0 ou version ultérieure. Suivez les instructions dans [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) pour installer et configurer .Net Framework 4.5.  
  
Vous pouvez trouver des informations sur les magasins d'attributs personnalisés utilisés par AD FS en exécutant la commande Windows PowerShell suivante : 

``` powershell
Get-ADFSAttributeStore
```  

Les étapes de la mise à niveau ou de la migration des magasins d’attributs personnalisés varient.  
  
2. Vous devez exporter manuellement tous les fichiers .dll des magasins d’attributs personnalisés que vous souhaitez conserver dans votre nouvelle batterie AD FS dans Windows Server 2012 R2. Les étapes de la mise à niveau ou de la migration des fichiers .dll des magasins d’attributs personnalisés varient.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Créer une batterie de serveurs de fédération Windows Server 2012 R2  
  
1.  Installer le système d’exploitation Windows Server 2012 R2 sur un ordinateur que vous souhaitez pour fonctionner comme un serveur de fédération, puis ajoutez le rôle serveur AD FS. Pour plus d’informations, voir [Installer le service de rôle AD FS](install-the-ad-fs-role-service.md). Configurez ensuite votre nouveau service de fédération via l’Assistant Configuration du service de fédération Active Directory ou via Windows PowerShell. Pour plus d’informations, voir « Configurer le premier serveur de fédération dans une nouvelle batterie de serveurs de fédération » dans [Configurer un serveur de fédération](configure-a-federation-server.md).  

Tout en effectuant cette étape, vous devez suivre les instructions ci-dessous :  
  
-   Vous devez disposer de privilèges d’administrateur de domaine afin de configurer votre service de fédération.  
  
-   Vous devez utiliser le même nom de service de fédération (nom de batterie) que celui utilisé dans AD FS 2.0 ou AD FS dans Windows Server 2012. Si vous n’utilisez pas le même nom de service de fédération, les certificats que vous avez sauvegardés ne fonctionnera pas dans le service de fédération Windows Server 2012 R2 que vous cherchez à configurer.  
  
-   Indiquez s’il s’agit d’une batterie de serveurs de fédération WID ou SQL Server. S’il s’agit d’une batterie SQL, spécifiez le nom de l’instance et l’emplacement de la base de données SQL Server.  
  
-   Vous devez fournir un fichier pfx contenant le certificat d’authentification serveur SSL que vous avez sauvegardé dans le cadre de la préparation du processus de migration AD FS.  
  
-   Vous devez spécifier la même identité de compte de service que celle utilisée dans AD FS 2.0 ou AD FS dans la batterie Windows Server 2012.  
  
2. Une fois que le nœud initial est configuré, vous pouvez ajouter d’autres nœuds à votre nouvelle batterie. Pour plus d’informations, voir « Ajouter un serveur de fédération à une batterie de serveurs de fédération existante » dans [Configure a Federation Server](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importer les données de configuration d’origine dans la batterie AD FS Windows Server 2012 R2  
 Maintenant que vous avez une batterie de serveurs de fédération AD FS en cours d’exécution dans Windows Server 2012 R2, vous pouvez importer les données de configuration AD FS d’origine dedans.  
  
1.  Importez et configurez d’autres certificats AD FS personnalisés, y compris les certificats de chiffrement/déchiffrement de jeton et de signature de jetons inscrits en externe, et le certificat de communications de service s’il est différent du certificat SSL.  
  
Dans la console de gestion AD FS, sélectionnez **Certificats**. Vérifiez les certificats de communications de service, de chiffrement/déchiffrement de jeton et de signature de jetons en les comparant tous aux valeurs que vous avez exportées dans le fichier certificates.txt lors de la préparation de la migration.  
  
Pour modifier les certificats de signature de jetons ou de déchiffrement de jeton en remplaçant les certificats auto-signés par défaut par des certificats externes, vous devez d’abord désactiver la fonctionnalité de substitution automatique de certificats qui est activée par défaut. Pour ce faire, vous pouvez utiliser la commande Windows PowerShell suivante :  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2. Configurez tous les paramètres de service AD FS personnalisés, par exemple AutoCertificateRollover ou la durée de vie SSO, à l’aide de l’applet de commande Set-AdfsProperties.  
  
3. Pour importer des approbations de partie de confiance AD FS et les approbations de fournisseur de revendications, vous devez être connecté en tant qu’administrateur (Toutefois, pas en tant qu’administrateur de domaine) sur votre fédération, serveur et exécuter la commande Windows PowerShell suivante script qui est situé dans le dossier \support\adfs du CD d’installation de Windows Server 2012 R2 :  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  Le script d’importation accepte les paramètres suivants :  
> 
> - Import-FederationConfiguration.ps1 -Path <string\> [-ComputerName <string\>] [-Credential <pscredential\>] [-Force] [-LogPath <string\>] [-CertificatePassword <securestring\>]  
>   -   Importer-FederationConfiguration.ps1-chemin d’accès < chaîne\> [-ComputerName < chaîne\>] [-Credential < pscredential\>] [-Force] [-LogPath < chaîne\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustIdentifier < string [] >] [-ClaimsProviderTrustIdentifier < string [] >  
>   -   Importer-FederationConfiguration.ps1-chemin d’accès < chaîne\> [-ComputerName < chaîne\>] [-Credential < pscredential\>] [-Force] [-LogPath < chaîne\>] [-CertificatePassword < securestring \>] [-RelyingPartyTrustName < string [] >] [-ClaimsProviderTrustName < string [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>**  : l’applet de commande importe uniquement les approbations de partie de confiance dont les identificateurs sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de partie de confiance n’est importée. Si aucun des éléments RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName et ClaimsProviderTrustName n’est spécifié, le script importe toutes les approbations de partie de confiance et approbations de fournisseur de revendications.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>**  : l’applet de commande importe uniquement les approbations de fournisseur de revendications dont les identificateurs sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de fournisseur de revendications n’est importée.  
> 
>   **-RelyingPartyTrustName <string[]>**  : l’applet de commande importe uniquement les approbations de partie de confiance dont les noms sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de partie de confiance n’est importée.  
> 
>   **-ClaimsProviderTrustName <string[]>**  : l’applet de commande importe uniquement les approbations de fournisseur de revendications dont les noms sont spécifiés dans le tableau de chaînes. Par défaut, AUCUNE des approbations de fournisseur de revendications n’est importée.  
> 
>   **-Path < chaîne\>**  -le chemin d’accès à un dossier qui contient les fichiers de configuration à importer.  
> 
>   **-LogPath < chaîne\>**  -le chemin d’accès à un dossier qui contiendra le fichier journal d’importation. Un fichier journal nommé import.log sera créé dans ce dossier.  
> 
>   **-ComputerName < chaîne\>**  -Spécifie le nom d’hôte du serveur STS. La valeur par défaut est l'ordinateur local. Si vous migrez AD FS 2.0 ou AD FS dans Windows Server 2012 vers AD FS dans Windows Server 2012 R2, ce paramètre doit avoir pour valeur le nom d’hôte du serveur AD FS hérité.  
> 
>   **-Credential < PSCredential\>** -spécifie un compte d’utilisateur qui a l’autorisation d’effectuer cette action. La valeur par défaut est l’utilisateur actuel.  
> 
>   **-Force** : spécifie de ne pas demander de confirmation de l’utilisateur.  
> 
>   **-CertificatePassword < SecureString\>**  -spécifie un mot de passe pour l’importation de clés privées des certificats AD FS. Si cette valeur n’est pas spécifiée, le script demande un mot de passe si un certificat AD FS avec une clé privée doit être importé.  
> 
>   **Entrées** : chaîne. Cette commande prend le chemin d’accès du dossier d’importation comme entrée. Vous pouvez diriger Export-FederationConfiguration vers cette commande.  
> 
>   **Sorties :** Aucun.  
  
La présence d’espaces en fin de chaîne dans la propriété WSFedEndpoint d’une approbation de partie de confiance peut provoquer l’échec du script d’importation. Dans ce cas, supprimez manuellement les espaces du fichier avant l’importation. Par exemple, les entrées suivantes provoquent des erreurs :  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  Si vous disposez de règles de revendication personnalisées (autres que les règles par défaut AD FS) sur l’approbation de fournisseur de revendications Active Directory dans le système source, elles ne seront pas migrées par les scripts. Il s’agit, car Windows Server 2012 R2 a de nouvelles valeurs par défaut. Toutes les règles personnalisées doivent être fusionnées en les ajoutant manuellement à l’approbation de fournisseur de revendications Active Directory dans la batterie de serveurs Windows Server 2012 R2.  
  
4. Configurez tous les paramètres de point de terminaison AD FS personnalisés. Dans la console de gestion AD FS, sélectionnez **Points de terminaison**. Vérifiez les points de terminaison AD FS activés par rapport à la liste des points de terminaison AD FS activés que vous avez exportés dans un fichier lors de la préparation de la migration AD FS.  
  
    \- et -  
  
    Configurez toutes les descriptions des revendications personnalisées. Dans la console de gestion AD FS, sélectionnez **Descriptions des revendications**. Vérifiez la liste des descriptions des revendications AD FS par rapport à la liste des descriptions des revendications que vous avez exportées dans un fichier pendant la préparation de la migration AD FS. Ajoutez toutes les descriptions des revendications personnalisées incluses dans votre fichier, mais qui ne figurent pas dans la liste par défaut dans AD FS. Notez que l’identificateur de revendication dans la console de gestion est mappé au type de revendication dans le fichier.  
  
5. Installez et configurez tous les magasins d’attributs personnalisés sauvegardés. En tant qu’administrateur, vérifiez que les fichiers binaires des magasins d’attributs personnalisés sont mis à niveau vers .NET Framework 4.0 ou version ultérieure avant de mettre à jour la configuration AD FS pour pointer vers eux.  
  
6. Configurez les propriétés du service qui sont mappées aux paramètres du fichier web.config hérités.  
  
   -   Si **useRelayStateForIdpInitiatedSignOn** a été ajouté à la **web.config** enregistrée dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés de service suivantes dans AD FS dans Batterie de serveurs Windows Server 2012 R2 :  
  
       -   AD FS dans Windows Server 2012 R2 inclut un **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** fichier. Créez un élément avec la même syntaxe que la **web.config** élément de fichier : `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Ajoutez cet élément dans le cadre de **< microsoft.identityserver.web >** section de la **Microsoft.IdentityServer.Servicehost.exe.config** fichier.  
  
   -   Si **< persistIdentityProviderInformation activée = » true&#124;false » lifetimeInDays = enablewhrPersistence « 90 » = « true&#124;false » /\>**  a été ajouté à la **web.config** fichier dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés de service suivantes dans AD FS dans la batterie de serveurs Windows Server 2012 R2 :  
  
       1.  Dans AD FS dans Windows Server 2012 R2, exécutez la commande Windows PowerShell suivante : `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
   -   Si **< singleSignOn activé = » true&#124;false » /\>**  a été ajouté à la **web.config** fichier dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, il est inutile de définir n’importe quel service supplémentaire propriétés dans AD FS dans la batterie de serveurs Windows Server 2012 R2. L’authentification unique est activée par défaut dans AD FS dans la batterie de serveurs Windows Server 2012 R2.  
  
   -   Si les paramètres localAuthenticationTypes ont été ajoutés à la **web.config** enregistrée dans AD FS 2.0 ou AD FS dans la batterie de serveurs Windows Server 2012, vous devez configurer les propriétés de service suivantes dans AD FS dans la batterie de serveurs Windows Server 2012 R2 :  
  
       -   Intégré, les formulaires, TlsClient, liste de base transformer dans AD FS équivalents dans Windows Server 2012 R2 a des paramètres de stratégie d’authentification globaux pour prendre en charge les deux types d’authentification et proxy de fédération. Ces paramètres peuvent être configurés dans le composant logiciel enfichable Gestion AD FS sous **Stratégies d’authentification**.  
  
   Après avoir importé les données de configuration d’origine, vous pouvez personnaliser les pages de connexion AD FS si nécessaire. Pour plus d'informations, voir [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Étapes suivantes
 [Migrer des Services de rôle Active Directory Federation Services vers Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Préparation à la migration du serveur de fédération AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migration de serveur Proxy de fédération AD FS](migrate-fed-server-proxy-r2.md)   
 [Vérification de la Migration des services AD FS vers Windows Server 2012 R2](verify-ad-fs-migration.md)