---
title: Outil XML Serializer Generator (Sgen.exe)
ms.date: 03/30/2017
ms.assetid: cc1d1f1c-fb26-4be9-885a-3fe84c81cec6
ms.openlocfilehash: 0eb937d003da02322c097d0eda0c3f8769f97dc8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61640151"
---
# <a name="xml-serializer-generator-tool-sgenexe"></a>Outil XML Serializer Generator (Sgen.exe)
L'outil XML Serializer Generator crée un assembly de sérialisation XML pour les types dans un assembly spécifié afin d'améliorer les performances de démarrage d'un <xref:System.Xml.Serialization.XmlSerializer> lorsqu'il sérialise ou désérialise les objets des types spécifiés.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sgen [options]  
```  
  
## <a name="parameters"></a>Paramètres  
  
|Option|Description|  
|------------|-----------------|  
|**/a\[ssembly\]:**_filename_|Génère le code de sérialisation pour tous les types contenus dans l’assembly ou le fichier exécutable spécifié par *nom_fichier*. Un seul nom de fichier peut être fourni. Si cet argument est répété, le dernier nom de fichier est utilisé.|  
|**/c\[ompiler\]:**_options_|Spécifie les options à passer au compilateur C#. Toutes les options csc.exe sont prises en charge à mesure qu'elles sont passées au compilateur. Cette option peut être utilisée pour spécifier que l'assembly doit être signé et pour indiquer le fichier de clé.|  
|**/d\[ebug\]**|Génère une image qui peut être utilisée avec un débogueur.|  
|**/f\[orce\]**|Force l'écrasement par réécriture d'un assembly existant du même nom. La valeur par défaut est **false**.|  
|**/help ou /?**|Affiche la syntaxe et les options de commande de l'outil.|  
|**/k\[eep\]**|Efface la suppression des fichiers source générés et d'autres fichiers temporaires une fois qu'ils ont été compilés dans l'assembly de sérialisation. Cette option peut être utilisée afin de déterminer si l'outil génère le code de sérialisation pour un type particulier.|  
|**/n\[ologo\]**|Supprime l'affichage de la bannière de démarrage Microsoft.|  
|**/o\[ut\]:**_path_|Spécifie le répertoire dans lequel enregistrer l'assembly généré. **Remarque :**  Le nom de l'assembly généré est composé du nom de l'assembly d'entrée suivi de"xmlSerializers.dll".|  
|**/p\[roxytypes\]**|Génère un code de sérialisation uniquement pour les types de proxy de service Web XML.|  
|**/r\[eference\]:**_assemblyfiles_|Spécifie les assemblys référencés par les types qui requièrent la sérialisation XML. Accepte plusieurs fichiers d'assembly séparés par des virgules.|  
|**/s\[ilent\]**|Supprime l'affichage des messages indiquant la réussite des opérations.|  
|**/t\[ype\]:**_type_|Génère un code de sérialisation uniquement pour le type spécifié.|  
|**/v\[erbose\]**|Affiche la sortie en clair pour le débogage. Répertorie les types à partir de l'assembly cible qui ne peuvent pas être sérialisés avec le <xref:System.Xml.Serialization.XmlSerializer>.|  
|**/?**|Affiche la syntaxe et les options de commande de l'outil.|  
  
## <a name="remarks"></a>Notes  
 Lorsque l'outil XML Serializer Generator n'est pas utilisé, un <xref:System.Xml.Serialization.XmlSerializer> génère un code de sérialisation et un assembly de sérialisation pour chacun des types chaque fois qu'une application est exécutée. Pour améliorer les performances de démarrage de la sérialisation XML, utilisez l’outil Sgen.exe pour générer ces assemblys à l’avance. Ces assemblys peuvent ensuite être déployés avec l'application.  
  
 L'outil XML Serializer Generator peut également améliorer les performances des clients qui utilisent des proxies de service Web XML pour communiquer avec les serveurs car le processus de sérialisation n'entraîne pas de dégradation des performances lors du premier chargement du type.  
  
 Ces assemblys générés ne peuvent pas être utilisés du côté serveur d'un service Web. Cet outil est conçu uniquement pour les clients de service Web et les scénarios de sérialisation manuelle.  
  
 Si l'assembly qui contient le type à sérialiser est nommé MyType.dll, l'assembly de sérialisation associé sera alors nommé MyType.XmlSerializers.dll.  
  
## <a name="examples"></a>Exemples  
 La commande suivante crée un assembly nommé Data.XmlSerializers.dll pour sérialiser tous les types contenus dans l'assembly nommé Data.dll.  
  
```  
sgen Data.dll   
```  
  
 L'assembly Data.XmlSerializers.dll peut être référencé à partir du code qui doit sérialiser et désérialiser les types dans Data.dll.  
  
## <a name="see-also"></a>Voir aussi

- [Outils](../../../docs/framework/tools/index.md)
- [Invites de commandes](../../../docs/framework/tools/developer-command-prompt-for-vs.md)
