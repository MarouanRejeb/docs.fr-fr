---
title: Élément <gcConcurrent>
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/gcConcurrent
- http://schemas.microsoft.com/.NetConfiguration/v2.0#gcConcurrent
helpviewer_keywords:
- container tags, <gcConcurrent> element
- gcConcurrent element
- <gcConcurrent> element
ms.assetid: 503f55ba-26ed-45ac-a2ea-caf994da04cd
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 0e2be4d9384f1e1ef73ce6064184aa2621a517a8
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61674101"
---
# <a name="gcconcurrent-element"></a>\<gcConcurrent > élément

Spécifie si le common language runtime exécute l'opération garbage collection sur un thread distinct.

\<configuration>\
\<runtime>\
\<gcConcurrent>

## <a name="syntax"></a>Syntaxe

```xml
<gcConcurrent
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>Attributs et éléments

Les sections suivantes décrivent des attributs, des éléments enfants et des éléments parents.

### <a name="attributes"></a>Attributs

|Attribut|Description|
|---------------|-----------------|
|`enabled`|Attribut requis.<br /><br /> Spécifie si le runtime exécute l’opération garbage collection simultanément.|

## <a name="enabled-attribute"></a>attribut activé

|Value|Description|
|-----------|-----------------|
|`false`|Ne s’exécute pas le garbage collection simultanément.|
|`true`|Exécute l'opération garbage collection simultanément. Il s'agit de la valeur par défaut.|

### <a name="child-elements"></a>Éléments enfants

Aucun.

### <a name="parent-elements"></a>Éléments parents

|Élément|Description|
|-------------|-----------------|
|`configuration`|Élément racine de chaque fichier de configuration utilisé par le Common Language Runtime et les applications .NET Framework.|
|`runtime`|Contient des informations sur les liaisons d’assembly et l’opération garbage collection.|

## <a name="remarks"></a>Notes

Dans les versions antérieures à .NET Framework 4, le garbage collection de station de travail prenait en charge le garbage collection simultané, qui exécutait l'opération garbage collection en arrière-plan sur un thread distinct. Dans .NET Framework 4, le garbage collection simultané a été remplacé par le garbage collection d'arrière-plan pour effectuer l'opération de la même manière. Depuis .NET Framework 4.5, le garbage collection d'arrière-plan est disponible dans le garbage collection de serveur. Le `<gcConcurrent>` élément contrôle si le runtime exécute soit simultané ou en arrière-plan le garbage collection, s’il est disponible, ou s’il exécute le garbage collection de premier plan.

### <a name="to-disable-background-garbage-collection"></a>Pour désactiver le garbage collection d’arrière-plan

> [!WARNING]
> Depuis .NET Framework 4, le garbage collection simultané est remplacé par le garbage collection d'arrière-plan. Les termes du contrat *simultanées* et *arrière-plan* sont utilisés indifféremment dans la documentation .NET Framework. Pour désactiver le garbage collection d'arrière-plan, utilisez l'élément `<gcConcurrent>` comme indiqué dans cet article.

Par défaut, le runtime utilise le garbage collection simultané ou d’arrière-plan, dont la latence est optimisée. Si votre application implique une grande interaction avec l'utilisateur, laissez le garbage collection simultané activé pour minimiser le temps d'interruption de l'application pendant l'exécution de l'opération garbage collection. Si vous définissez l’attribut `enabled` de l’élément `<gcConcurrent>` avec la valeur `false`, le runtime utilise le garbage collection non simultané, dont le débit est optimisé. Le fichier de configuration suivant désactive le garbage collection d’arrière-plan.

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="false"/>
   </runtime>
</configuration>
```

 S’il existe un `<gcConcurrentSetting>` définissant dans le fichier de configuration machine, il définit la valeur par défaut pour toutes les applications .NET Framework. Ce paramètre se substitue au paramètre du fichier de configuration de l'application.

 Pour plus d’informations sur simultané et garbage collection d’arrière-plan, consultez le [le garbage collection simultané](../../../../standard/garbage-collection/fundamentals.md#concurrent-garbage-collection) section dans le [Fundamentals of Garbage Collection](../../../../standard/garbage-collection/fundamentals.md) article.

## <a name="example"></a>Exemple

L’exemple suivant active le garbage collection simultané :

```xml
<configuration>
   <runtime>
      <gcConcurrent enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>Voir aussi

- [Schéma des paramètres d’exécution](index.md)
- [Schéma des fichiers de configuration](../index.md)
- [Principes de base du Garbage Collection](../../../../standard/garbage-collection/fundamentals.md)
