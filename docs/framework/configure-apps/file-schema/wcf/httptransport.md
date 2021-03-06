---
title: <httpTransport>
ms.date: 03/30/2017
ms.assetid: 8b30c065-b32a-4fa3-8eb4-5537a9c6b897
ms.openlocfilehash: 7a06772f078f9187298ad0b4b9c8b0f51c849928
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61756063"
---
# <a name="httptransport"></a>\<httpTransport>
Spécifie un transport HTTP pour la transmission des messages SOAP d’une liaison personnalisée.  
  
 \<system.serviceModel>  
\<bindings>  
\<customBinding>  
\<binding>  
\<httpTransport>  
  
## <a name="syntax"></a>Syntaxe  
  
```xml  
<httpTransport allowCookies="Boolean"
               authenticationScheme="Digest/Negotiate/Ntlm/Basic/Anonymous"
               bypassProxyOnLocal="Boolean"
               hostnameComparisonMode="StrongWildcard/Exact/WeakWildcard"
               keepAliveEnabled="Boolean"
               maxBufferSize="Integer"
               proxyAddress="Uri"
               proxyAuthenticationScheme="None/Digest/Negotiate/Ntlm/Basic/Anonymous"
               realm="String"
               transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
               unsafeConnectionNtlmAuthentication="Boolean"
               useDefaultWebProxy="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Attributs et éléments  
 Les sections suivantes décrivent des attributs, des éléments enfants et des éléments parents.  
  
### <a name="attributes"></a>Attributs  
  
|Attribut|Description|  
|---------------|-----------------|  
|allowCookies|Valeur booléenne qui spécifie si le client accepte les cookies et les propage dans de futures demandes. La valeur par défaut est `false`.<br /><br /> Vous pouvez utiliser cet attribut lorsque vous interagissez avec les services Web ASMX qui utilisent des cookies. De cette manière, vous avez la certitude que les cookies retournés par le serveur sont automatiquement copiés dans toutes les futures demandes du client pour ce service.|  
|authenticationScheme|Spécifie le protocole utilisé pour authentifier des demandes du client qui sont traitées par un écouteur HTTP. Les valeurs valides sont les suivantes :<br /><br /> -Digest : Spécifie l’authentification Digest.<br />-Par négociation : Négocie avec le client afin de déterminer le schéma d’authentification. Si le client et le serveur prennent tous les deux en charge Kerberos, ce protocole est utilisé ; sinon, NTLM est utilisé.<br />-Ntlm : Spécifie l'authentification NTLM.<br />-De base : Spécifie l'authentification de base.<br />-Anonyme : Spécifie l'authentification anonyme.<br /><br /> La valeur par défaut est Anonymous. Cet attribut est de type <xref:System.Net.AuthenticationSchemes>. Cet attribut ne peut être défini qu'une fois.|  
|bypassProxyOnLocal|Valeur booléenne qui indique s'il faut ignorer le serveur proxy pour les adresses locales. La valeur par défaut est `false`.<br /><br /> Une adresse locale est une adresse sur le réseau local ou l'intranet.<br /><br /> Windows Communication Foundation (WCF) ignore toujours le proxy si l’adresse de service commence par `http://localhost`.<br /><br /> Utilisez le nom d'hôte plutôt que localhost si vous souhaitez que les clients passent par un proxy lorsqu'ils communiquent avec des services sur le même ordinateur.|  
|hostnameComparisonMode|Spécifie le mode de comparaison du nom d'hôte HTTP utilisé pour analyser des URI. Les valeurs valides sont :<br /><br /> -StrongWildcard : (« + ») correspond à tous les noms d’hôtes possibles dans le contexte du schéma spécifié, le port et l’URI relatif.<br />-Exact : aucun des caractères génériques<br />-WeakWildcard : («\*») correspond au nom d’hôte possible dans le contexte de schéma spécifié, le port et le UIR relatif qui n’ont pas été trouvées explicitement ou via un mécanisme générique fort.<br /><br /> Cet attribut est de type <xref:System.ServiceModel.HostNameComparisonMode>. La valeur par défaut est <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>.|  
|KeepAliveEnabled|Valeur booléenne qui spécifie si une connexion persistante doit être établie avec la ressource Internet.|  
|maxBufferSize|Entier positif qui spécifie la taille maximale de la mémoire tampon. La valeur par défaut est 524 288.|  
|proxyAddress|URI qui spécifie l'adresse du proxy HTTP. Si `useSystemWebProxy` est `true`, ce paramètre doit avoir la valeur `null`. La valeur par défaut est `null`.|  
|proxyAuthenticationScheme|Spécifie le protocole utilisé pour l'authentification des demandes du client qui sont traitées par un proxy HTTP. Les valeurs valides sont les suivantes :<br /><br /> -None : Aucune authentification n’est effectuée.<br />-Digest : Spécifie l’authentification Digest.<br />-Par négociation : Négocie avec le client afin de déterminer le schéma d’authentification. Si le client et le serveur prennent tous les deux en charge Kerberos, ce protocole est utilisé ; sinon, NTLM est utilisé.<br />-Ntlm : Spécifie l'authentification NTLM.<br />-De base : Spécifie l'authentification de base.<br />-Anonyme : Spécifie l'authentification anonyme.<br /><br /> La valeur par défaut est Anonymous. Cet attribut est de type <xref:System.Net.AuthenticationSchemes>. Notez que <xref:System.Net.AuthenticationSchemes.IntegratedWindowsAuthentication?displayProperty=nameWithType> n’est pas pris en charge.|  
|realm|Chaîne qui spécifie le domaine à utiliser sur le proxy/serveur. La valeur par défaut est une chaîne vide.<br /><br /> Les serveurs utilisent des domaines pour partitionner des ressources protégées. Chaque partition peut posséder son propre schéma d'authentification et/ou sa base de données d'autorisation. Les domaines sont utilisés uniquement pour les authentifications Digest et de base. Lorsqu'un client est correctement authentifié, l'authentification est valide pour toutes les ressources contenues dans un domaine donné. Pour obtenir une description détaillée des domaines, consultez la RFC 2617 à le [site Web IETF](https://www.ietf.org).|  
|transferMode|Spécifie si les messages sont mis en mémoire tampon ou transmis en continu ou s'il s'agit d'une demande ou d'une réponse. Les valeurs valides sont les suivantes :<br /><br /> -Mise en mémoire tampon : Les messages de demande et de réponse sont mis en mémoire tampon.<br />-Transmis en continu : Les messages de demande et de réponse sont transmis en continu.<br />-   StreamedRequest: Le message de demande est transmis en continu et le message de réponse est mis en mémoire tampon.<br />-   StreamedResponse: Le message de demande est mis en mémoire tampon et le message de réponse est transmis en continu.<br /><br /> La valeur par défaut est Buffered. Cet attribut est de type <xref:System.ServiceModel.TransferMode>.|  
|unsafeConnectionNtlmAuthentication|Valeur booléenne qui spécifie si le partage de connexion potentiellement dangereux est activé sur le serveur. La valeur par défaut est `false`. S'il est activé, l'authentification NTLM est exécutée une fois sur chaque connexion TCP.|  
|useDefaultWebProxy|Valeur booléenne qui spécifie si les paramètres proxy à l'échelle de l'ordinateur sont utilisés plutôt que ceux spécifiques à l'utilisateur. La valeur par défaut est `true`.|  
  
### <a name="child-elements"></a>Éléments enfants  
 Aucun.  
  
### <a name="parent-elements"></a>Éléments parents  
  
|Élément|Description|  
|-------------|-----------------|  
|[\<binding>](../../../../../docs/framework/misc/binding.md)|Définit toutes les fonctions de liaison d’une liaison personnalisée.|  
  
## <a name="remarks"></a>Notes  
 L'élément `httpTransport` constitue le point de départ pour créer une liaison personnalisée qui implémente le protocole de transport HTTP. HTTP est le principal transport utilisé à des fins d'interopérabilité. Ce transport est pris en charge par Windows Communication Foundation (WCF) pour garantir l’interopérabilité avec les autres piles de services Web non - WCF.  
  
## <a name="see-also"></a>Voir aussi

- <xref:System.ServiceModel.Configuration.HttpTransportElement>
- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transports](../../../../../docs/framework/wcf/feature-details/transports.md)
- [Choix d’un transport](../../../../../docs/framework/wcf/feature-details/choosing-a-transport.md)
- [Liaisons](../../../../../docs/framework/wcf/bindings.md)
- [Extension de liaisons](../../../../../docs/framework/wcf/extending/extending-bindings.md)
- [Liaisons personnalisées](../../../../../docs/framework/wcf/extending/custom-bindings.md)
- [\<customBinding>](../../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)
