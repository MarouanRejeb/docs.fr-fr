---
title: 'Procédure : Écrire du texte dans un fichier'
ms.date: 01/04/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- writing text to files
- I/O [.NET Framework], writing text to files
- streams, writing text to files
- data streams, writing text to files
ms.assetid: 060cbe06-2adf-4337-9e7b-961a5c840208
author: mairaw
ms.author: mairaw
ms.openlocfilehash: 59b33c24c2821d0aef17f5feb67c1178810b939e
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64647788"
---
# <a name="how-to-write-text-to-a-file"></a>Procédure : Écrire du texte dans un fichier
Cette rubrique présente différentes façons d’écrire du texte dans un fichier pour une application .NET. 

Les classes et méthodes suivantes sont généralement utilisées pour écrire du texte dans un fichier :  
  
- <xref:System.IO.StreamWriter> contient des méthodes permettant d’écrire dans un fichier de façon synchrone (<xref:System.IO.StreamWriter.Write%2A> et <xref:System.IO.TextWriter.WriteLine%2A>) ou asynchrone (<xref:System.IO.StreamWriter.WriteAsync%2A> et <xref:System.IO.StreamWriter.WriteLineAsync%2A>).  
  
- <xref:System.IO.File> fournit des méthodes statiques pour écrire du texte dans un fichier, comme <xref:System.IO.File.WriteAllLines%2A> et <xref:System.IO.File.WriteAllText%2A>, ou pour ajouter du texte à un fichier, comme <xref:System.IO.File.AppendAllLines%2A>, <xref:System.IO.File.AppendAllText%2A> et <xref:System.IO.File.AppendText%2A>.  
  
- <xref:System.IO.Path> concerne les chaînes qui contiennent des informations relatives au chemin d’un fichier ou d’un répertoire. Elle contient la méthode <xref:System.IO.Path.Combine%2A> et, dans .NET Core versions 2.1 et ultérieures, les méthodes <xref:System.IO.Path.Join%2A> et <xref:System.IO.Path.TryJoin%2A>, qui permettent la concaténation de chaînes pour générer un chemin de fichier ou de répertoire.

> [!NOTE]
> Les exemples suivants ne montrent que la quantité minimale de code nécessaire. Une application réelle fournit généralement une vérification des erreurs et une gestion des exceptions plus robuste.  
  
## <a name="example-synchronously-write-text-with-streamwriter"></a>Exemple : écrire du texte avec StreamWriter de façon synchrone

L’exemple suivant montre comment utiliser la classe <xref:System.IO.StreamWriter> pour écrire, ligne par ligne, du texte de façon synchrone dans un nouveau fichier. Étant donné que l’objet <xref:System.IO.StreamWriter> est déclaré et instancié dans une instruction `using`, la méthode <xref:System.IO.StreamWriter.Dispose%2A> est appelée, ce qui vide et ferme automatiquement le flux.  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/write.cs)] 
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/write.vb)]  

## <a name="example-synchronously-append-text-with-streamwriter"></a>Exemple : ajouter du texte avec StreamWriter de façon synchrone

L’exemple suivant montre comment utiliser la classe <xref:System.IO.StreamWriter> pour ajouter du texte de façon synchrone au fichier texte créé dans le premier exemple.   

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/append.cs)] 
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/append.vb)]  

## <a name="example-asynchronously-write-text-with-streamwriter"></a>Exemple : écrire du texte avec StreamWriter de façon asynchrone

L’exemple suivant montre comment écrire du texte de façon asynchrone dans un nouveau fichier à l’aide de la classe <xref:System.IO.StreamWriter> . Pour appeler la méthode <xref:System.IO.StreamWriter.WriteAsync%2A>, l’appel de méthode doit se trouver dans une méthode `async`. L’exemple C# nécessite C# 7.1 ou version ultérieure, qui ajoute la prise en charge du modificateur `async` sur le point d’entrée du programme. 

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/async.cs)] 
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/async.vb)]  

## <a name="example-write-and-append-text-with-the-file-class"></a>Exemple : écrire et ajouter du texte avec la classe File

L’exemple suivant montre comment écrire du texte dans un nouveau fichier et ajouter de nouvelles lignes de texte à ce même fichier à l’aide de la classe <xref:System.IO.File> . Les méthodes <xref:System.IO.File.WriteAllText%2A> et <xref:System.IO.File.AppendAllLines%2A> ouvrent et ferment automatiquement le fichier. Si le chemin que vous fournissez à la méthode <xref:System.IO.File.WriteAllText%2A> existe déjà, le fichier est remplacé.  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/file.cs)] 
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/file.vb)]  

## <a name="see-also"></a>Voir aussi

- <xref:System.IO.StreamWriter>
- <xref:System.IO.Path>
- <xref:System.IO.File.CreateText%2A?displayProperty=nameWithType>
- [Guide pratique pour énumérer des répertoires et des fichiers](../../../docs/standard/io/how-to-enumerate-directories-and-files.md)
- [Guide pratique pour lire et écrire dans un fichier de données créé récemment](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)
- [Guide pratique pour ouvrir un fichier journal et y ajouter des éléments](../../../docs/standard/io/how-to-open-and-append-to-a-log-file.md)
- [Guide pratique pour lire le texte d’un fichier](../../../docs/standard/io/how-to-read-text-from-a-file.md)
- [E/S de fichier et de flux](../../../docs/standard/io/index.md)