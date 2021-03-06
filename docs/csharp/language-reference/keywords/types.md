---
title: Types - Référence C#
ms.custom: seodec18
ms.date: 07/20/2015
helpviewer_keywords:
- types [C#]
- data types [C#], type system
ms.assetid: 16b984df-f417-4e02-b1e6-4589d4a614ea
ms.openlocfilehash: 41406e74a207f289968017f1f9869b23b165c34b
ms.sourcegitcommit: bdd930b5df20a45c29483d905526a2a3e4d17c5b
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/11/2018
ms.locfileid: "53236659"
---
# <a name="types-c-reference"></a>Types (référence C#)

Le système de types C# comporte les catégories suivantes :

- [Types valeur](value-types.md)

- [Types référence](reference-types.md)

- [Types de pointeur](../../programming-guide/unsafe-code-pointers/pointer-types.md)

 Les variables de types valeur stockent des données alors que les variables de types référence stockent les références aux données réelles. Les instances de types référence sont également considérées comme des objets. Les types pointeur ne peuvent être utilisés qu’en mode [unsafe](unsafe.md).

 Il est possible de convertir un type valeur en un type référence, puis de revenir au type valeur, en effectuant une conversion [boxing et unboxing](../../../csharp/programming-guide/types/boxing-and-unboxing.md). Vous ne pouvez pas convertir un type référence en type valeur, sauf s’il s’agit d’un type valeur boxed.

 Cette section présente également [void](void.md).

## <a name="see-also"></a>Voir aussi

- [Référence C#](../index.md)
- [Guide de programmation C#](../../programming-guide/index.md)
- [Mots clés C#](index.md)
- [Tableaux de référence des types](reference-tables-for-types.md)
- [Cast et conversions de types](../../programming-guide/types/casting-and-type-conversions.md)
- [Types](../../programming-guide/types/index.md)
