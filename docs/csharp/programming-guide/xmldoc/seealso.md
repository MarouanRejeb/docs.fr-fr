---
title: <seealso> - Guide de programmation C#
ms.custom: seodec18
ms.date: 07/20/2015
f1_keywords:
- cref
- <seealso>
- seealso
helpviewer_keywords:
- cref [C#], see also
- seealso C# XML tag
- cref [C#]
- cross-references [C#], tags
- <seealso> C# XML tag
ms.assetid: 8e157f3f-f220-4fcf-9010-88905b080b18
ms.openlocfilehash: cccb338d2dbed7889512428a53804324795c66bd
ms.sourcegitcommit: 5137208fa414d9ca3c58cdfd2155ac81bc89e917
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57498287"
---
# <a name="seealso-c-programming-guide"></a>\<seealso> (Guide de programmation C#)
## <a name="syntax"></a>Syntaxe  
  
```xml  
<seealso cref="member"/>  
```  
  
## <a name="parameters"></a>Paramètres  
 cref = " `member`"  
 Référence à un membre ou un champ qu’il est possible d’appeler à partir de l’environnement de compilation actuel. Le compilateur vérifie que l’élément de code donné existe, et qu’il passe `member` au nom d’élément dans le code XML de sortie. `member` doit être placé entre guillemets doubles (" ").  
  
 Pour plus d’informations sur la création d’une référence cref à un type générique, consultez [\<see>](../../../csharp/programming-guide/xmldoc/see.md).  
  
## <a name="remarks"></a>Remarques  
 La balise \<seealso> vous permet de spécifier le texte que vous souhaitez voir apparaître dans une section Voir aussi. Utilisez [\<see>](../../../csharp/programming-guide/xmldoc/see.md) pour spécifier un lien à partir de l’intérieur du texte.  
  
 Compilez avec [/doc](../../../csharp/language-reference/compiler-options/doc-compiler-option.md) pour placer les commentaires de documentation dans un fichier en vue de les traiter.  
  
## <a name="example"></a>Exemple  
 Pour obtenir un exemple d’utilisation de \<seealso>, consultez [\<summary>](../../../csharp/programming-guide/xmldoc/summary.md).  
  
## <a name="see-also"></a>Voir aussi

- [Guide de programmation C#](../../../csharp/programming-guide/index.md)
- [Balises recommandées pour les commentaires de documentation](../../../csharp/programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)
