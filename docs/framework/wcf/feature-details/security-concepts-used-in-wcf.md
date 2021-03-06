---
title: Concepts de sécurité utilisés dans WCF
ms.date: 03/30/2017
ms.assetid: 3b9dfcf5-4bf1-4f35-9070-723171c823a1
ms.openlocfilehash: 3ef2b9c104fa15de17a769c9ca9354e5cef085bf
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61990871"
---
# <a name="security-concepts-used-in-wcf"></a>Concepts de sécurité utilisés dans WCF
Sécurité de Windows Communication Foundation (WCF) est basée sur des concepts déjà en cours d’utilisation et déployée dans diverses infrastructures de sécurité.  
  
 WCF prend en charge quelques-unes de ces infrastructures, telles que couche SSL (Secure Sockets) sur HTTP (HTTPS). Toutefois, WCF va au-delà de la prise en charge des infrastructures de sécurité existantes en implémentant des standards de sécurité interopérables plus récents (tels que WS-Security) sur les messages encodés en SOAP. Toutefois, qu'il s'agisse de mécanismes existants ou de nouveaux standards interopérables, les concepts de sécurité sous-jacents sont dans ces deux cas les mêmes. Par conséquent, comprendre les concepts qui sous-tendent les infrastructures existantes et les standards plus récents est un atout essentiel à l'implémentation du meilleur modèle de sécurité pour une application.  
  
## <a name="introduction-to-security-for-wcf-web-services"></a>Introduction à la sécurité pour WCF Web Services  
 Le groupe Microsoft Patterns and Practices a rédigé un livre blanc approfondi sur les conseils de sécurité WCF qui est disponible en téléchargement ici : [Guide de sécurité WCF](https://go.microsoft.com/fwlink/?LinkId=210210). Ce livre blanc décrit les concepts de sécurité fondamentaux liés aux services Web, aux principes de sécurité WCF clés ainsi qu'aux scénarios d'application Intranet et Internet.  
  
## <a name="industry-wide-security-specifications"></a>Spécifications de sécurité de l'industrie  
  
### <a name="public-key-infrastructure"></a>Infrastructure à clé publique  
 L’infrastructure à clé publique (PKI) est un système de certificats numériques, d’autorités de certification et autres autorités d’immatriculation qui vérifient et authentifient chaque partie impliquée dans une transaction électronique via l’utilisation du chiffrement à clé publique. Pour plus d’informations, consultez [Services de certificats Windows Server 2008 R2](https://go.microsoft.com/fwlink/?LinkId=210211).  
  
### <a name="kerberos-protocol"></a>Protocole Kerberos  
 Le *protocole Kerberos* est une spécification permettant la création d’un mécanisme de sécurité qui authentifie les utilisateurs dans un domaine Windows. Il permet d'établir un contexte sécurisé avec d'autres entités au sein d'un domaine. Windows 2000 et les plateformes ultérieures utilisent le protocole Kerberos par défaut. La maîtrise des mécanismes du système est utile lors de la création d'un service qui interagira avec les clients intranet. En outre, depuis le *Web Services Security Kerberos Binding* est largement publié, vous pouvez utiliser le protocole Kerberos pour communiquer avec les clients Internet (autrement dit, le protocole Kerberos est interopérable). Pour plus d’informations sur la façon dont le protocole Kerberos est implémenté dans Windows, consultez [Microsoft Kerberos](https://go.microsoft.com/fwlink/?LinkId=210212).  
  
### <a name="x509-certificates"></a>Certificats X.509  
 Les certificats X.509 constituent un formulaire d'informations d'identification principal utilisé dans les applications de sécurité. Pour plus d’informations sur X.509 certificats, consultez [les certificats de clé publique X.509](https://go.microsoft.com/fwlink/?LinkId=210213). Les certificats X.509 sont stockés dans un magasin de certificats. Un ordinateur qui exécute Windows a plusieurs types de magasins de certificats, chacun ayant un but différent. Pour plus d’informations sur les différents magasins, consultez [magasins de certificats](https://go.microsoft.com/fwlink/?LinkID=87787).  
  
## <a name="web-services-security-specifications"></a>Spécifications Web Services Security  
 Les liaisons définies par le système prennent en charge plusieurs spécifications de sécurité communément utilisées pour les services Web. Pour une liste complète des liaisons fournies par le système et les spécifications de services web qu’ils prennent en charge consultez : [Protocoles de services web pris en charge par des liaisons d’interopérabilité fournies par le système](../../../../docs/framework/wcf/feature-details/web-services-protocols-supported-by-system-provided-interoperability-bindings.md)  
  
## <a name="access-control-mechanisms"></a>Mécanismes de contrôle d'accès  
 WCF offre plusieurs méthodes pour contrôler l'accès à un service ou à une opération. Parmi celles-ci :  
  
1. <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
2. Fournisseur d'appartenances ASP.NET  
  
3. Fournisseur de rôle ASP.NET  
  
4. Gestionnaire d'autorisations  
  
5. Modèle d'identité  
  
 Pour plus d’informations sur ce sujet, consultez [mécanismes de contrôle d’accès](../../../../docs/framework/wcf/feature-details/access-control-mechanisms.md)  
  
## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble de la sécurité](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [Modèle de sécurité pour Windows Server AppFabric](https://go.microsoft.com/fwlink/?LinkID=201279&clcid=0x409)
