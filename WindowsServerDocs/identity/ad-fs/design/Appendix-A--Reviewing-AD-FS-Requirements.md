---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: Annexe A - examen des exigences en matière de AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e90df713f08dd387a2438b34839d16efe6e470f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191694"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Annexe A : examen de la configuration requise pour AD FS

Afin que les partenaires organisationnels dans votre déploiement Active Directory Federation Services (ADFS) puissent collaborer efficacement, vous devez tout d’abord vous assurer que votre infrastructure de réseau d’entreprise est configurée pour prendre en charge la configuration requise pour AD FS pour les comptes, nom résolution et certificats. AD FS présente les conditions requises suivantes :  
  
> [!TIP]  
> Vous trouverez des liens vers des ressources AD FS supplémentaires à la page [Plan de contenu AD FS 2.0](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) sur Microsoft TechNet Wiki. Cette page est gérée par les membres de la communauté AD FS et est régulièrement surveillée par l'équipe de produit AD FS.  
  
## <a name="hardware-requirements"></a>Configuration matérielle requise  
Les conditions matérielles minimales et recommandées suivantes s’appliquent aux ordinateurs de proxy de serveur de fédération et serveur de fédération.  
  
|Configuration matérielle|Configuration minimale requise|Configuration recommandée|  
|------------------------|-----------------------|---------------------------|  
|Vitesse du processeur|Cœur unique, 1 gigahertz (GHz)|quadruple cœur, 2 GHz|  
|RAM|1 Go|4 Go|  
|Espace disque|50 Mo|100 Mo|  
  
## <a name="software-requirements"></a>Configuration logicielle requise  
AD FS s’appuie sur les fonctionnalités de serveur intégré dans le système d’exploitation Windows Server® 2012.  
  
> [!NOTE]  
> Les services de rôle Service de fédération et Proxy du service de fédération ne peuvent pas coexister sur le même ordinateur.  
  
## <a name="certificate-requirements"></a>Exigences de certificat  
Les certificats jouent le rôle le plus important dans la sécurisation des communications entre les serveurs de fédération, les serveurs proxy de fédération, les applications prenant en charge les revendications et les clients web. La configuration requise pour les certificats varie selon que vous configurez un ordinateur serveur de fédération ou un ordinateur serveur proxy de fédération, comme décrit dans cette section.  
  
### <a name="federation-server-certificates"></a>Certificats des serveurs de fédération  
Les serveurs de fédération ont besoin des certificats indiqués dans le tableau suivant.  
  
|Type de certificat|Description|Ce que vous devez savoir avant d'effectuer le déploiement|  
|--------------------|---------------|------------------------------------------|  
|Certificat SSL (Secure Sockets Layer)|Certificat SSL (Secure Sockets Layer) standard qui permet de sécuriser les communications entre les serveurs de fédération et les clients.|Ce certificat doit être lié au site web par défaut dans Internet Information Services (IIS) pour un serveur de fédération ou un serveur proxy de fédération.  Dans le cas d'un Serveur proxy de fédération, vous devez configurer la liaison dans IIS avant d'exécuter l'Assistant Configuration du serveur proxy de fédération.<br /><br />**Recommandation :** ce certificat devant être approuvé par les clients d'AD FS, utilisez un certificat d'authentification serveur émis par une autorité de certification publique (tierce), comme VeriSign. **Conseil :** Le nom de sujet de ce certificat permet de représenter le nom du service de fédération pour chaque instance d'AD FS que vous déployez. Vous pouvez donc choisir pour tout nouveau certificat émis par une autorité de certification un nom de sujet qui reflète le nom de votre entreprise ou organisation auprès des partenaires.|  
|Certificat de communication du service|Ce certificat active la sécurité de message WCF pour sécuriser les communications entre les serveurs de fédération.|Par défaut, le certificat SSL est utilisé en tant que certificat de communication du service.  Vous pouvez modifier ce paramétrage à l'aide de la console Gestion AD FS.|  
|Certificat de signature de jetons|Certificat X509 standard qui permet de signer de manière sécurisée tous les jetons émis par le serveur de fédération.|Le certificat de signature de jetons doit contenir une clé privée et être lié à une source digne de confiance dans le service de fédération. Par défaut, AD FS crée un certificat auto-signé. Toutefois, vous pouvez modifier ce paramétrage à tout moment au profit d'un certificat émis par une autorité de certification à l'aide du composant logiciel enfichable Gestion AD FS, suivant les besoins de votre organisation.|  
|Certificat de déchiffrement de jeton|Certificat SSL standard qui permet de déchiffrer les jetons entrants chiffrés par un serveur de fédération partenaire. Il est également publié dans les métadonnées de fédération.|Par défaut, AD FS crée un certificat auto-signé. Toutefois, vous pouvez modifier ce paramétrage à tout moment au profit d'un certificat émis par une autorité de certification à l'aide du composant logiciel enfichable Gestion AD FS, suivant les besoins de votre organisation.|  
  
> [!CAUTION]  
> Les certificats utilisés pour la signature de jeton et pour le déchiffrement de jeton sont essentiels pour la stabilité du service de fédération. Comme la perte ou la suppression non planifiée d'un certificat configuré à cette fin peut perturber le service, vous devez sauvegarder tous les certificats de ce type.  
  
Pour plus d'informations sur les certificats utilisés par les serveurs de fédération, consultez [Certificate Requirements for Federation Servers](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificats des serveurs proxy de fédération  
Les serveurs proxy de fédération ont besoin des certificats indiqués dans le tableau suivant.  
  
|Type de certificat|Description|Ce que vous devez savoir avant d'effectuer le déploiement|  
|--------------------|---------------|------------------------------------------|  
|Certificat d'authentification serveur|Certificat SSL (Secure Sockets Layer) standard qui permet de sécuriser les communications entre un serveur proxy de fédération et les ordinateurs client Internet.|Vous devez lier ce certificat au site web par défaut dans Internet Information Services (IIS) avant d'exécuter l'Assistant Configuration du serveur proxy de fédération AD FS.<br /><br />**Recommandation :** ce certificat devant être approuvé par les clients d'AD FS, utilisez un certificat d'authentification serveur émis par une autorité de certification publique (tierce), comme VeriSign.<br /><br />**Conseil :** Le nom de sujet de ce certificat permet de représenter le nom du service de fédération pour chaque instance d'AD FS que vous déployez. Vous pouvez donc choisir un nom de sujet qui reflète le nom de votre entreprise ou organisation auprès des partenaires.|  
  
Pour plus d'informations sur les certificats utilisés par les serveurs proxy de fédération, consultez [Exigences de certificat pour les serveurs proxy de fédération](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Configuration requise des navigateurs  
Bien que tout navigateur web actuel doté de la fonctionnalité JavaScript puisse être configuré en tant que client AD FS, les pages web fournies par défaut n'ont été testées que par rapport à Internet Explorer versions 7.0, 8.0 et 9.0, Mozilla Firefox 3.0 et Safari 3.1 sur Windows. JavaScript doit être activé, et les cookies doivent être autorisés pour que la connexion et la déconnexion via un navigateur fonctionnent correctement.  
  
L’équipe de produit AD FS chez Microsoft testé avec succès les configurations de navigateur et le système d’exploitation dans le tableau suivant.  
  
|Navigateur|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|Non testé|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS prend en charge les versions 32 bits et 64 bits de tous les navigateurs indiqués dans le tableau ci-dessus.  
  
### <a name="cookies"></a>Cookies  
AD FS crée des cookies persistants et de session qui doivent être stockés sur les ordinateurs clients pour fournir des fonctionnalités telles que la connexion, la déconnexion et l'authentification unique. Le navigateur client doit donc être configuré de manière à accepter les cookies. Les cookies utilisés pour l'authentification sont toujours des cookies de session HTTPS (Secure Hypertext Transfer Protocol) écrits pour le serveur d'origine. Si le navigateur client n'est pas configuré de manière à autoriser ces cookies, AD FS ne peut pas fonctionner correctement. Les cookies persistants permettent de conserver le fournisseur de revendications choisi par l'utilisateur. Vous pouvez les désactiver à l'aide d'un paramètre de configuration dans le fichier de configuration des pages de connexion AD FS.  
  
Pour des raisons de sécurité, la prise en charge de TLS/SSL est nécessaire.  
  
## <a name="network-requirements"></a>Configuration requise pour le réseau  
Configuration des services réseau suivants correctement est essentielle pour réussir le déploiement d’AD FS dans votre organisation.  
  
### <a name="tcpip-network-connectivity"></a>Connectivité réseau TCP/IP  
Pour AD FS fonctionne, une connectivité réseau TCP/IP doit exister entre le client ; un contrôleur de domaine ; et les ordinateurs qui hébergent le Service de fédération, le Proxy de Service de fédération (quand celui-ci est utilisé) et l’Agent Web AD FS.  
  
### <a name="dns"></a>DNS  
Le service réseau principal qui est essentiel au fonctionnement d’AD FS, autres que les Services de domaine Active Directory (AD DS), est le système DNS (Domain Name). Quand DNS est déployé, les utilisateurs peuvent se connecter aux ordinateurs et autres ressources sur les réseaux IP à l'aide de noms d'ordinateur conviviaux faciles à retenir.  
  
 Windows Server 2008 utilise DNS pour la résolution de nom au lieu de la résolution de noms Windows Internet Service NetBIOS WINS (Name) qui a été utilisée dans les réseaux Windows NT 4.0. Vous pouvez toujours utiliser WINS pour les applications qui le nécessitent. Toutefois, les services AD DS et AD FS nécessitent la résolution de nom DNS.  
  
Le processus de configuration DNS pour prendre en charge AD FS varie selon que :  
  
-   Votre organisation possède déjà une infrastructure DNS. Dans la plupart des scénarios, la configuration DNS existante permet aux clients de navigateur web reliés à votre réseau d'entreprise d'accéder à Internet. Internet access et résolution de noms étant indispensables d’AD FS, cette infrastructure est supposée être en place pour votre déploiement AD FS.  
  
-   Vous envisagez d'ajouter un serveur fédéré à votre réseau d'entreprise. Pour l'authentification des utilisateurs sur le réseau d'entreprise, les serveurs DNS internes dans la forêt du réseau d'entreprise doivent être configurés de manière à retourner le nom CNAME du serveur interne qui exécute le service de fédération. Pour plus d'informations, voir [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Vous envisagez d'ajouter un serveur proxy fédéré à votre réseau de périmètre. Lorsque vous souhaitez authentifier des comptes d’utilisateurs qui sont trouvent dans le réseau d’entreprise de votre organisation partenaire d’identité, les serveurs DNS internes dans la forêt du réseau d’entreprise doivent être configurés pour retourner l’enregistrement CNAME du serveur proxy de fédération interne. Pour plus d’informations sur la façon de configurer DNS pour prendre en charge l’ajout de serveurs proxy de fédération, consultez [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Vous configurez DNS pour un environnement lab de test. Si vous envisagez de n’utiliser AD FS dans un environnement de laboratoire de test où aucun serveur DNS de racine unique faisant autorité, il est probable que vous devrez configurer des redirecteurs DNS afin que les requêtes de noms entre deux ou plusieurs forêts soient correctement redirigées. Pour obtenir des informations générales sur la façon de configurer un environnement de laboratoire de test AD FS, consultez [AD FS pas à pas et Guides](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Configuration requise pour les magasins d'attributs  
AD FS nécessite au moins un magasin d’attributs à utiliser pour l’authentification des utilisateurs et l’extraction des revendications de sécurité pour ces utilisateurs. Pour obtenir la liste des magasins d'attributs pris en charge par AD FS, consultez [Rôle des magasins d'attributs](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) dans le Guide de conception AD FS.  
  
> [!NOTE]  
> Par défaut, AD FS crée automatiquement un magasin d'attributs Active Directory.  
  
La configuration requise pour les magasins d'attributs varie selon que votre organisation fait office de partenaire de compte (hébergeant les utilisateurs fédérés) ou de partenaire de ressource (hébergeant l'application fédérée).  
  
### <a name="adds"></a>AD DS  
Pour AD FS fonctionner correctement, les contrôleurs de domaine dans l’organisation partenaire de compte ou de l’organisation partenaire de ressource doivent exécuter Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 ou Windows Server 2012.  
  
Quand AD FS est installé et configuré sur un ordinateur appartenant à un domaine, le magasin de comptes d'utilisateur Active Directory pour ce domaine peut être sélectionné en guise de magasin d'attributs.  
  
> [!IMPORTANT]  
> Étant donné que AD FS nécessite l’installation de Internet Information Services (IIS), nous vous recommandons d’installer le logiciel AD FS sur un contrôleur de domaine dans un environnement de production pour des raisons de sécurité. Toutefois, cette configuration est prise en charge par le service clientèle de Microsoft.  
  
#### <a name="schema-requirements"></a>Configuration requise pour le schéma  
AD FS ne nécessite pas de modifications de schéma ou du niveau fonctionnel dans AD DS.  
  
#### <a name="functional-level-requirements"></a>Configuration requise pour le niveau fonctionnel  
La plupart des fonctionnalités d'AD FS ne nécessitent aucune modification du niveau fonctionnel dans AD DS. Toutefois, le niveau fonctionnel de domaine Windows Server 2008 ou supérieur est nécessaire au bon fonctionnement de l'authentification de certificat client si le certificat est mappé explicitement sur le compte d'un utilisateur dans AD DS.  
  
#### <a name="service-account-requirements"></a>Configuration requise pour les comptes de service  
Si vous créez une batterie de serveurs de fédération, vous devez d'abord créer dans AD DS un compte de service de domaine dédié utilisable par le service de fédération. Ensuite, vous configurez chaque serveur de fédération au sein de la batterie de manière à utiliser ce compte. Pour plus d'informations sur la façon d'effectuer cette opération, consultez [Configurer manuellement un compte de service pour une batterie de serveurs de fédération](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) dans le Guide de déploiement d'AD FS.  
  
### <a name="ldap"></a>LDAP  
Quand vous utilisez d'autres magasins d'attributs LDAP (Lightweight Directory Access Protocol), vous devez vous connecter à un serveur LDAP qui prend en charge l'authentification intégrée de Windows. En outre, la chaîne de connexion LDAP doit être écrite sous la forme d'une URL LDAP, comme indiqué dans le document RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Pour AD FS fonctionner correctement, les ordinateurs qui hébergent le magasin d’attributs de langage SQL (Structured Query) serveur doivent exécuter Microsoft SQL Server 2005 ou SQL Server 2008. Quand vous utilisez des magasins d'attributs SQL, vous devez également configurer une chaîne de connexion.  
  
### <a name="custom-attribute-stores"></a>Magasins d'attributs personnalisés  
Vous pouvez développer des magasins d'attributs personnalisés dans le cadre de scénarios avancés. Le langage de stratégie intégré à AD FS peut référencer des magasins d'attributs personnalisés de manière à perfectionner les scénarios suivants :  
  
-   Créer des revendications pour un utilisateur authentifié localement  
  
-   Compléter les revendications pour un utilisateur authentifié de manière externe  
  
-   Autoriser un utilisateur à obtenir un jeton  
  
-   Autoriser un service à obtenir un jeton au nom d'un utilisateur  
  
Quand vous utilisez un magasin d'attributs personnalisé, vous pouvez également être amené à configurer une chaîne de connexion. Dans cette situation, vous pouvez entrer n'importe quel code personnalisé permettant d'établir une connexion à votre magasin d'attributs personnalisé. La chaîne de connexion ainsi créée est un ensemble de paires nom/valeur interprétées comme étant implémentées par le développeur du magasin d'attributs personnalisé.  
  
Pour plus d’informations sur le développement et à l’aide des magasins d’attributs personnalisés, consultez [vue d’ensemble du Store attributs](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Configuration requise pour les applications  
Les serveurs de fédération peuvent communiquer avec les applications de fédération, telles que les applications prenant en charge les revendications, et protéger ces applications.  
  
## <a name="authentication-requirements"></a>Configuration requise pour l'authentification  
AD FS intègre naturellement l’authentification Windows existante, par exemple, l’authentification Kerberos, NTLM, les cartes à puce et les certificats côté client X.509 v3. Les serveurs de fédération utilisent l'authentification Kerberos standard pour authentifier un utilisateur par rapport à un domaine. Les clients peuvent s'authentifier à l'aide de l'authentification basée sur les formulaires, de l'authentification par carte à puce et de l'authentification intégrée de Windows, suivant la façon dont vous configurez l'authentification.  
  
Grâce au rôle de serveur proxy de fédération AD FS, l'utilisateur peut s'authentifier de manière externe à l'aide de l'authentification de client SSL. Vous pouvez également configurer le rôle de serveur de fédération de manière à imposer l'authentification de client SSL, bien que la meilleure façon de rendre l'expérience utilisateur la plus transparente possible consiste généralement à configurer le serveur de fédération de comptes pour utiliser l'authentification intégrée de Windows. Dans cette situation, AD FS n'exerce aucun contrôle sur les informations d'identification dont se sert l'utilisateur pour ouvrir une session de bureau Windows.  
  
### <a name="smart-card-logon"></a>Ouverture de session par carte à puce  
Bien que AD FS puissent appliquer le type d’informations d’identification qu’il utilise pour l’authentification (mots de passe, l’authentification client SSL ou authentification intégrée Windows), ils n’appliquent pas directement l’authentification par carte à puce. Par conséquent, AD FS ne fournit pas une interface utilisateur côté client (IU) pour obtenir des informations d’identification le numéro d’identification personnel de carte à puce. Il s’agit, car les clients basés sur Windows intentionnellement ne fournissent pas de détails des informations d’identification utilisateur pour les serveurs de fédération ou de serveurs Web.  
  
### <a name="smart-card-authentication"></a>Authentification par carte à puce  
L’authentification de carte à puce utilise le protocole Kerberos pour s’authentifier auprès d’un serveur de fédération de compte. AD FS ne peut pas être étendus pour ajouter de nouvelles méthodes d’authentification. Le certificat dans la carte à puce n'est pas obligé d'être lié à une source digne de confiance sur l'ordinateur client. Utilisation d’un certificat basé sur une carte à puce avec AD FS requiert les conditions suivantes :  
  
-   Le lecteur et le fournisseur de services de chiffrement pour la carte à puce doivent fonctionner sur l'ordinateur où se trouve le navigateur.  
  
-   Le certificat de carte à puce doit être lié à une racine approuvée sur le serveur de fédération de compte et le serveur proxy de fédération de compte.  
  
-   Le certificat doit être mappé au compte d'utilisateur dans AD DS à l'aide de l'une des méthodes suivantes :  
  
    -   Le nom du sujet du certificat correspond au nom unique LDAP d'un compte d'utilisateur dans AD DS.  
  
    -   L'extension de l'autre nom de l'objet du certificat possède le nom d'utilisateur principal d'un compte d'utilisateur dans AD DS.  
  
Si la procédure d'authentification le nécessite, vous pouvez également configurer AD FS de manière à créer une revendication qui indique la façon dont l'utilisateur a été authentifié. Une partie de confiance peut ensuite utiliser cette revendication pour prendre une décision d'autorisation.  
  
## <a name="see-also"></a>Voir aussi
[Guide de conception AD FS dans Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
