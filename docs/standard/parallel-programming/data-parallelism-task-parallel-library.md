---
title: Parallélisme de données (bibliothèque parallèle de tâches)
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallelism, data
ms.assetid: 3f05f33f-f1da-4b16-81c2-9ceff1bef449
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 3450908cadff453df79ab8fcf10285c25dfa50a7
ms.sourcegitcommit: 40364ded04fa6cdcb2b6beca7f68412e2e12f633
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/28/2019
ms.locfileid: "56976224"
---
# <a name="data-parallelism-task-parallel-library"></a>Parallélisme de données (bibliothèque parallèle de tâches)
Le *parallélisme des données* fait référence aux scénarios dans lesquels la même opération est exécutée de manière simultanée (autrement dit, en parallèle) sur les éléments d’un tableau ou d’une collection source. Dans les opérations en parallèle de données, la collection source est partitionnée afin que plusieurs threads puissent fonctionner simultanément sur des segments différents.  
  
 La bibliothèque parallèle de tâches prend en charge le parallélisme des données via la classe <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType>. Cette classe fournit des implémentations parallèles, fondées sur une méthode, des boucles [for](~/docs/csharp/language-reference/keywords/for.md) et [foreach](~/docs/csharp/language-reference/keywords/foreach-in.md) (`For` et `For Each` en Visual Basic). Vous écrivez la logique de boucle d'une boucle <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> ou <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> de la même manière que pour une boucle séquentielle. Vous n’avez pas à créer de threads ou d’éléments de travail de file d’attente. Dans les boucles simples, vous n'avez pas besoin d'acquérir de verrous. La bibliothèque parallèle de tâches gère tous les travaux de bas niveau pour vous. Pour des informations détaillées sur l’utilisation de <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> et <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>, téléchargez le document [Patterns for Parallel Programming : Understanding and Applying Parallel Patterns with the .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=19222) (Modèles de programmation parallèle : présentation et application des modèles parallèles avec .NET Framework 4). L'exemple de code suivant montre une boucle `foreach` simple et son équivalent parallèle.  
  
> [!NOTE]
>  Cette documentation utilise les expressions lambda pour définir les délégués de la bibliothèque parallèle de tâches. Si les expressions lambda en C# ou Visual Basic ne vous sont pas familières, consultez la page [Expressions lambda en PLINQ et dans la bibliothèque parallèle de tâches](../../../docs/standard/parallel-programming/lambda-expressions-in-plinq-and-tpl.md).  
  
 [!code-csharp[TPL#20](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl/cs/tpl.cs#20)]
 [!code-vb[TPL#20](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl/vb/tpl_vb.vb#20)]  
  
 Quand une boucle parallèle s'exécute, la bibliothèque parallèle de tâches partitionne la source de données afin que la boucle puisse s'exécuter simultanément sur plusieurs parties. En arrière-plan, le planificateur de tâches partitionne la tâche en fonction des ressources système et de la charge de travail. Quand cela lui est possible, le planificateur redistribue le travail entre plusieurs threads et processeurs si la charge de travail est déséquilibrée.  
  
> [!NOTE]
>  Vous pouvez également fournir votre propre partitionneur ou planificateur personnalisé. Pour plus d’informations, consultez la page [Partitionneurs personnalisés pour PLINQ et la bibliothèque parallèle de tâches (TPL)](../../../docs/standard/parallel-programming/custom-partitioners-for-plinq-and-tpl.md) et [Planificateurs de tâches](xref:System.Threading.Tasks.TaskScheduler).  
  
 Les méthodes <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> et <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> ont plusieurs surcharges qui vous permettent d'arrêter ou de quitter l'exécution d'une boucle, de surveiller l'état de la boucle sur d'autres threads, de maintenir l'état local de thread, de finaliser des objets locaux de thread, de contrôler le degré d'accès concurrentiel, etc. Les types d'assistance qui permettent ces fonctionnalités incluent <xref:System.Threading.Tasks.ParallelLoopState>, <xref:System.Threading.Tasks.ParallelOptions>, <xref:System.Threading.Tasks.ParallelLoopResult>, <xref:System.Threading.CancellationToken> et <xref:System.Threading.CancellationTokenSource>.  
  
 Pour plus d’informations, consultez la page [Patterns for Parallel Programming: Understanding and Applying Parallel Patterns with the .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=19222) (Modèles de programmation parallèle : présentation et application des modèles parallèles avec .NET Framework 4).  
  
 Le parallélisme des données avec syntaxe déclarative ou de requête est pris en charge par PLINQ. Pour plus d’informations, consultez la page [PLINQ (Parallel LINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md).  
  
## <a name="related-topics"></a>Rubriques connexes  
  
|Titre|Description|  
|-----------|-----------------|  
|[Guide pratique pour écrire une boucle Parallel.For simple](../../../docs/standard/parallel-programming/how-to-write-a-simple-parallel-for-loop.md)|Explique comment écrire une boucle <xref:System.Threading.Tasks.Parallel.For%2A> sur tout tableau ou collection source <xref:System.Collections.Generic.IEnumerable%601> indexable.|  
|[Guide pratique pour écrire une boucle Parallel.ForEach simple](../../../docs/standard/parallel-programming/how-to-write-a-simple-parallel-foreach-loop.md)|Explique comment écrire une boucle <xref:System.Threading.Tasks.Parallel.ForEach%2A> sur toute collection source <xref:System.Collections.Generic.IEnumerable%601>.|  
|[Guide pratique pour arrêter ou quitter une boucle Parallel.For](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/dd460721(v=vs.100))|Explique comment arrêter ou rompre une boucle parallèle afin que tous les threads soient informés de l'action.|  
|[Guide pratique pour écrire une boucle Parallel.For avec des variables locales de thread](../../../docs/standard/parallel-programming/how-to-write-a-parallel-for-loop-with-thread-local-variables.md)|Explique comment écrire une boucle <xref:System.Threading.Tasks.Parallel.For%2A> dans laquelle chaque thread maintient une variable privée qui n’est pas visible pour les autres threads et comment synchroniser les résultats de tous les threads quand la boucle se termine.|  
|[Guide pratique pour écrire une boucle Parallel.ForEach avec des variables locales de partition](../../../docs/standard/parallel-programming/how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)|Explique comment écrire une boucle <xref:System.Threading.Tasks.Parallel.ForEach%2A> dans laquelle chaque thread maintient une variable privée qui n’est pas visible pour les autres threads et comment synchroniser les résultats de tous les threads quand la boucle se termine.|  
|[Guide pratique pour annuler une boucle Parallel.For ou ForEach](../../../docs/standard/parallel-programming/how-to-cancel-a-parallel-for-or-foreach-loop.md)|Explique comment annuler une boucle parallèle à l'aide d'un <xref:System.Threading.CancellationToken?displayProperty=nameWithType>.|  
|[Guide pratique pour accélérer les petits corps de boucles](../../../docs/standard/parallel-programming/how-to-speed-up-small-loop-bodies.md)|Décrit une façon d'accélérer l'exécution quand un corps de boucle est très petit.|  
|[La bibliothèque parallèle de tâches](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)|Fournit une vue d’ensemble de la bibliothèque parallèle de tâches.|  
|[Programmation parallèle](../../../docs/standard/parallel-programming/index.md)|Présente la programmation parallèle dans le .NET Framework.|  
  
## <a name="see-also"></a>Voir aussi

- [Programmation parallèle](../../../docs/standard/parallel-programming/index.md)
