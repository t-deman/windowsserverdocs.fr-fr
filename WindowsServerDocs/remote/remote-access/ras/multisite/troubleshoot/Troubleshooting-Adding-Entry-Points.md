---
title: Résolution des problèmes d’ajout de points d’entrée
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dcc1037f-1a65-4497-99e6-0df9aef748a8
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51f49364aa4e7a6da6c51b1d8b7da7e37f842190
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67282564"
---
# <a name="troubleshooting-adding-entry-points"></a>Résolution des problèmes d’ajout de points d’entrée

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de résolution des problèmes liés à la commande `Add-DAEntryPoint`. Pour confirmer que l’erreur que vous avez reçue est liée à l’ajout d’un point d’entrée, recherchez l’ID d’événement 10067 dans le journal des événements Windows.  
  
## <a name="missing-remoteaccessserver-parameter"></a>Paramètre RemoteAccessServer manquant  
**Erreur reçue**. Vous devez fournir une valeur pour le paramètre RemoteAccessServer.  
  
**Cause**  
  
Lorsque vous ajoutez un nouveau point d’entrée à un déploiement multisite, vous devez spécifier le paramètre *RemoteAccessServer*, lequel correspond au nom du serveur que vous voulez ajouter en tant que nouveau point d’entrée.  
  
**Solution**  
  
Exécutez la commande et assurez-vous de spécifier le paramètre *RemoteAccessServer* à l’aide du nom du serveur à ajouter en tant que point d’entrée.  
  
## <a name="remote-access-is-not-configured"></a>Accès à distance non configuré  
**Erreur reçue**. Accès à distance n’est pas configuré sur < nom_serveur >. Spécifiez le nom d’un serveur qui appartient à un déploiement multisite.  
  
**Cause**  
  
L’accès à distance n’est pas configuré sur l’ordinateur spécifié par le paramètre *ComputerName* ou sur l’ordinateur sur lequel vous exécutez la commande.  
  
Lorsque vous ajoutez une nouveau point d’entrée à un déploiement multisite, vous devez spécifier deux paramètres : *ComputerName* et *RemoteAccessServer*. Le paramètre *ComputerName* correspond au nom d’un serveur qui fait déjà partie du déploiement multisite et le paramètre *RemoteAccessServer* correspond au nom du serveur que vous voulez ajouter en tant que nouveau point d’entrée. Si l’exécution est effectuée depuis un ordinateur qui fait partie du déploiement multisite, le paramètre ComputerName n’est pas requis.  
  
**Solution**  
  
Exécutez la commande et assurez-vous de spécifier le paramètre *ComputerName* à l’aide du nom du serveur qui est déjà configuré comme faisant partie du déploiement multisite, ou exécutez la commande depuis un ordinateur qui fait partie du déploiement multisite.  
  
## <a name="multisite-not-enabled"></a>Multisite non activé  
**Erreur reçue**. Vous devez activer un déploiement multisite avant d’effectuer cette opération. Pour ce faire, utilisez l’applet de commande `Enable-DAMultiSite`.  
  
**Cause**  
  
Le déploiement multisite n’est pas activé sur le serveur spécifié par le paramètre *ComputerName*. Pour ajouter un nouveau point d’entrée à un déploiement de l’accès à distance, vous devez d’abord activer un déploiement multisite.  
  
**Solution**  
  
Activez un déploiement multisite à l’aide de l’applet de commande `Enable-DaMultiSite`. Pour plus d’informations, voir [Déployer l’accès à distance multisite](https://technet.microsoft.com/library/hh831664.aspx).  
  
## <a name="ipv6-prefix-issues"></a>Problèmes de préfixe IPv6  
  
-   **Problème 1**  
  
    **Erreur reçue**. IPv6 est déployé dans le réseau interne, mais vous n’avez spécifié un préfixe IPv6 de client.  
  
    **Cause**  
  
    Le protocole IPv6 est déployé sur le réseau d’entreprise et un préfixe IP-HTTPS est requis. Cependant, aucun préfixe n’a été spécifié dans le paramètre *ClientIPv6Prefix* du nouveau point d’entrée.  
  
    **Solution**  
  
    1.  Attribuez un préfixe IP-HTTPS unique au nouveau point d’entrée et assurez-vous que les paquets ciblés vers une adresse IP sous ce préfixe sont acheminés vers le serveur que vous ajoutez.  
  
    2.  Exécutez l’applet de commande `Add-DAEntryPoint` et spécifiez le préfixe IP-HTTPS dans le paramètre *ClientIPv6Prefix*.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Le préfixe IPv6 du client est déjà en cours d’utilisation par un autre point d’entrée. Donnez une autre valeur.  
  
    **Cause**  
  
    Le préfixe IP-HTTPS spécifié dans le paramètre *ClientIPv6Prefix* est déjà utilisé par un différent point d’entrée.  
  
    **Solution**  
  
    1.  Attribuez un préfixe IP-HTTPS unique au nouveau point d’entrée et assurez-vous que les paquets ciblés vers une adresse IP sous ce préfixe sont acheminés vers le serveur que vous ajoutez.  
  
    2.  Exécutez l’applet de commande `Add-DAEntryPoint` et spécifiez le préfixe IP-HTTPS dans le paramètre *ClientIPv6Prefix*.  
  
## <a name="connectto-address"></a>Adresse ConnectTo  
**Erreur reçue**. L’adresse (< adresse_connectto >) à laquelle les clients DirectAccess se connecter sur le serveur d’accès distant est identique à l’adresse du serveur emplacement réseau. Donnez une autre valeur.  
  
**Cause**  
  
L’adresse ConnectTo et celle du serveur Emplacement réseau sont les mêmes.  
  
**Solution**  
  
L’adresse ConnectTo doit pouvoir être résolue sur Internet afin de permettre aux ordinateurs clients de se connecter via le protocole IP-HTTPS. L’adresse du serveur Emplacement réseau doit pouvoir être résolue sur le réseau d’entreprise, mais pas sur Internet. Assurez-vous que l’adresse du serveur Emplacement réseau et l’adresse ConnectTo ne sont pas identiques. Sélectionnez des adresses différentes et réessayez.  
  
## <a name="directaccess-or-vpn-already-installed"></a>DirectAccess ou réseau privé virtuel déjà installé  
**Erreur reçue**. Une installation VPN a été détectée sur le serveur < nom_serveur >. Spécifiez un autre serveur sur lequel Remote Access n’est pas installé ou supprimez la configuration VPN du serveur.  
  
Ou  
  
Accès à distance est déjà installé sur le serveur < nom_serveur >. Spécifiez un autre serveur qui n’exécute pas DirectAccess, ou supprimez la configuration DirectAccess existante du serveur.  
  
**Cause**  
  
DirectAccess ou un réseau privé virtuel est déjà installé sur le nouveau point d’entrée. Vous ne pouvez pas ajouter un point d’entrée configuré à un déploiement multisite.  
  
**Solution**  
  
Pour ajouter un serveur à un déploiement multisite, vous devez installer le rôle Accès à distance sur le serveur mais DirectAccess et le réseau privé virtuel ne doivent pas être configurés.  
  
Exécutez la commande et assurez-vous que sur le serveur que vous spécifiez dans le paramètre *RemoteAccessServer*, DirectAccess ou un réseau privé virtuel ne sont pas configurés.  
  
## <a name="ipsec-root-certificate"></a>Certificat racine IPsec  
**Erreur reçue**. Le certificat racine IPsec configuré ne peut pas se trouver sur le serveur < nom_serveur >.  
  
**Cause**  
  
Le certificat de l’autorité de certification racine ou intermédiaire qui émet les certificats d’ordinateur est introuvable sur le serveur que vous essayez d’ajouter au déploiement.  
  
**Solution**  
  
Dans la console Gestion de l’accès à distance, dans **Étape 2 : Serveur d’accès à distance**, cliquez sur **Modifier**, puis dans la page **Authentification**, sous **Utiliser les certificats d’ordinateur**, assurez-vous que le certificat sélectionné est valide. Si le certificat est valide, assurez-vous qu’il se trouve sous l’autorité de certification racine approuvée sur le serveur que vous voulez ajouter, puis réessayez.  
  
> [!NOTE]  
> Le certificat doit être le même certificat avec la même empreinte numérique.  
  
Si le certificat n’est pas valide, sélectionnez un certificat valide qui est configuré en tant qu’autorité de certification racine approuvée sur tous les serveurs d’accès à distance.  
  
## <a name="mixing-ipv6-and-ipv4-entry-points"></a>Mélange de points d’entrée IPv6 et IPv4  
Lors de la première installation de DirectAccess, la carte réseau interne est inspectée afin de déterminer si le réseau contient des adresses IPv4 uniquement (réseau uniquement IPv4), des adresses IPv6 et IPv4 ou des adresses IPv6 uniquement (réseau uniquement IPv6). Ces informations sont utilisées pour déterminer le type de déploiement (IPv4 uniquement, IPv6+IPv4 ou IPv6 uniquement).  
  
-   **Problème 1**  
  
    **Avertissement reçu**. Le serveur d’accès à distance en cours d’ajout est configuré avec les adresses IPv4 et IPv6. Il s’agit d’un déploiement IPv4 uniquement et l’accès à distance ignorera les adresses IPv6.  
  
    **Cause**  
  
    Lors de la première installation de ce déploiement, le réseau interne a été détecté en tant que réseau IPv4 uniquement. Dans un déploiement multisite, les différents points d’entrée sont supposés se trouver dans différents sous-réseaux dont les caractéristiques sont différentes. Par conséquent, bien que le déploiement soit configuré en tant que déploiement IPv4 uniquement, il peut contenir un point d’entrée situé dans un sous-réseau IPv6+IPv4. Toutefois, bien que le point d’entrée doivent être ajouté au déploiement, DirectAccess ignore les adresses IPv6 configurées sur l’interface interne de nouveau point d’entrée.  
  
    **Solution**  
  
    Si le réseau interne entier est configuré avec des adresses IPv6 et IPv4, envisagez de passer à un déploiement IPv6+IPv4 pour tirer parti des technologies IPv6. Voir « Transition depuis une pure IPv4 à un réseau d’entreprise IPv6 + IPv4 » dans [étape 3 : Planifier le déploiement multisite](assetId:///19d49dbf-1786-47bb-ab97-f0458c53d91d).  
  
-   **Problème 2**  
  
    **Erreur reçue**. Les cartes réseau interne de serveurs d’accès distant de ce déploiement multisite sont configurées avec des adresses IPv4. Vous devez également configurer le point d’entrée que vous ajoutez avec une adresse IPv4 sur la carte réseau interne.  
  
    **Cause**  
  
    Lors de la première installation de ce déploiement, le réseau interne a été détecté en tant que réseau IPv4 uniquement. L’accès à distance a détecté que le point d’entrée que vous essayez d’ajouter est configuré avec uniquement des adresses IPv6 sur son réseau interne. Ceci n’est pas autorisé dans un déploiement IPv4 uniquement.  
  
    **Solution**  
  
    Si le réseau entier est déjà configuré avec des adresses IPv6, vous devez passer à un déploiement IPv6+IPv4 ou IPv6 uniquement. Voir « Planifier la transition vers IPv6 lorsque l’accès à distance multisite est déployé ».  
  
-   **Problème 3**  
  
    **Erreur reçue**. Ce point d’entrée se trouve dans un réseau IPv4, mais les points d’entrée précédents sont situés dans un réseau IPv6. Connectez ce point d’entrée au réseau IPv6 avant de l’ajouter au même déploiement multisite.  
  
    **Cause**  
  
    Lors de la première installation de ce déploiement, il a été détecté que le réseau interne était un réseau IPv6+IPv4 ou IPv6 uniquement. Il a été détecté que seules des adresses IPv4 sont configurées sur le réseau interne du nouveau point d’entrée que vous essayez d’ajouter. Ceci n’est pas autorisé dans les déploiements IPv6+IPv4 ou IPv6 uniquement.  
  
    **Solution**  
  
    Configurez le nouveau point d’entrée avec des adresses IPv6, puis ajoutez-le au déploiement multisite.  
  
-   **Problème 4**  
  
    **Avertissement reçu**. La carte réseau interne sur le serveur d’accès à distance n’est pas configurée avec une adresse IPv4. DNS64 et NAT64 ne seront pas configurés sur ce serveur. Les clients DirectAccess peuvent accéder uniquement aux serveurs internes IPv6.  
  
    **Cause**  
  
    Lors de la première installation de ce déploiement, il a été détecté que le réseau interne était un réseau IPv6+IPv4. Dans ce mode de déploiement, DNS64 et NAT64 sont activés de sorte à autoriser les ordinateurs clients à accéder aux ordinateurs situés sur le réseau interne qui sont configurés avec des adresses IPv4 uniquement.  
  
    Lors de l’ajout du nouveau point d’entrée, l’accès à distance a détecté que l’interface interne située sur le nouvel ordinateur possède uniquement des adresses IPv6. Pour configurer DNS64 et NAT64, une adresse IPv4 est requise afin d’acheminer les paquets depuis le serveur d’accès à distance vers l’ordinateur uniquement IPv4. Étant donné qu’il n’existe aucune adresse IP de ce type sur le nouvel ordinateur, NAT64 et DNS64 ne sont pas configurés sur le serveur d’accès à distance. Par conséquent, les ordinateurs clients qui accèdent au réseau d’entreprise via DirectAccess à l’aide de ce point d’entrée ne sont pas en mesure d’accéder aux serveurs IPv4 uniquement sur le réseau interne. Pour plus d’informations sur la transition vers un réseau IPv6 + IPv4 ou un réseau IPv6 uniquement, voir « Planifier la transition vers IPv6 lorsque de l’accès à distance multisite est déployé ».  
  
    **Solution**  
  
    Ajoutez une adresse IPv4 au nouveau serveur d’accès à distance pour garantir le bon fonctionnement de DNS64 et NAT64.  
  
## <a name="domain-issues-with-the-servergponame"></a>Problèmes de domaine avec le paramètre ServerGpoName  
  
-   **Problème 1**  
  
    **Erreur reçue**. Le domaine spécifié dans le paramètre ServerGpoName < objet_de_stratégie_de_groupe_du_serveur > n’existe pas. Spécifiez le domaine < nom_domaine > à la place.  
  
    **Cause**  
  
    Le nom de domaine qui fait partie du nom de l’objet de stratégie de groupe du serveur qui a été envoyé par l’administrateur est introuvable.  
  
    **Solution**  
  
    Vérifiez que vous avez correctement tapé le nom du domaine. Si le nom de domaine est correct, réessayez en utilisant le nom de domaine complet.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Le serveur de stratégie de groupe doit se trouver dans le domaine du serveur accès à distance. Spécifiez le domaine < nom_domaine > dans le paramètre ServerGpoName.  
  
    **Cause**  
  
    Le domaine de l’objet de stratégie de groupe du serveur n’est pas le même que celui auquel le serveur d’accès à distance appartient.  
  
    **Solution**  
  
    L’objet de stratégie de groupe du serveur doit se situer dans le même domaine que celui du serveur d’accès à distance. Utilisez le nom de domaine du serveur pour l’objet de stratégie de groupe du serveur, puis réessayez.  
  
## <a name="split-brain-dns"></a>DNS « split brain »  
**Avertissement reçu**. L’entrée NRPT pour le suffixe DNS < suffixe_dns > contient le nom public utilisé par les ordinateurs clients pour se connecter au serveur d’accès à distance. Ajoutez le nom < adresse_connectto > en tant qu’exemption dans la table NRPT.  
  
**Cause**  
  
Vous utilisez DNS « split brain ». Pour autoriser des clients à se connecter à l’aide du protocole IP-HTTPS, vous devez vous assurer que l’adresse ConnectTo sélectionnée est exempte dans les règles NRPT.  
  
**Solution**  
  
En cas de déploiement multisite, assurez-vous que toutes les adresses ConnectTo des différents points d’entrées sont exemptes des règles NRPT.  
  
Pour exempter une adresse dans les règles NRPT :  
  
1.  Dans la console Gestion de l’accès à distance, sous **Étape 3 : Serveurs d’infrastructure**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant **Installation du serveur d’infrastructure**, dans la page **DNS**, double-cliquez sur la table pour entrer un nouveau suffixe de nom.  
  
3.  Dans la boîte de dialogue **Adresses des serveurs DNS**, dans Suffixe DNS, entrez l’adresse ConnectTo du point d’entrée, puis cliquez sur **Appliquer**.  
  
Lorsque vous ajoutez des suffixes de noms sans spécifier d’adresse de serveur, le suffixe est traité comme une exemption NRPT.  
  
## <a name="saving-server-gpo-settings"></a>Enregistrement des paramètres de l’objet de stratégie de groupe serveur  
**Erreur reçue**. Une erreur s’est produite lors de l’enregistrement des paramètres d’accès à distance à l’objet de stratégie de groupe < nom_objet_de_stratégie_de_groupe >.  
  
Pour corriger cette erreur, voir l’enregistrement des paramètres du GPO de serveur dans [Multisite de l’activation de résolution des problèmes](https://technet.microsoft.com/library/jj591658.aspx).  
  
## <a name="gpo-updates-cannot-be-applied"></a>Impossible d’appliquer les mises à jour d’objet de stratégie de groupe  
**Avertissement reçu**. Mises à jour de l’objet stratégie de groupe ne peut pas être appliqués sur < nom_serveur >. Les modifications apportées ne prendront effet qu’après la prochaine actualisation de la stratégie.  
  
**Cause**  
  
Une erreur s’est produite lors de la tentative d’actualisation des stratégies sur l’ordinateur spécifié. Par conséquent, les modifications apportées ne prendront effet qu’après la prochaine actualisation de la stratégie.  
  
**Solution**  
  
Pour forcer une actualisation des stratégies, exécutez `gpupdate /force` sur l’ordinateur spécifié.  
  


