---
title: Création du document Office Open XML source (C#)
ms.date: 07/20/2015
ms.assetid: 653c8cdb-73be-4dc2-927f-924cfb4ed9ed
ms.openlocfilehash: 8b36d119eb2da7445649b8db1132b7deea2c684c
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59322392"
---
# <a name="creating-the-source-office-open-xml-document-c"></a>Création du document Office Open XML source (C#)
Cette rubrique montre comment créer le document WordprocessingML Office Open XML utilisé par les autres exemples de ce didacticiel. Si vous suivez ces instructions, votre sortie correspondra à la sortie fournie dans chaque exemple.  
  
 Toutefois, les exemples présentés dans ce didacticiel fonctionnent avec n'importe quel document WordprocessingML valide.  
  
 Pour créer le document utilisé par ce didacticiel, Microsoft Office 2007 ou une version ultérieure, ou bien Microsoft Office 2003 avec le Module de compatibilité Microsoft Office pour les formats de fichiers Word, Excel et PowerPoint 2007 doit être installé sur votre ordinateur.  
  
## <a name="creating-the-wordprocessingml-document"></a>Création du document WordprocessingML  
  
#### <a name="to-create-the-wordprocessingml-document"></a>Pour créer le document WordprocessingML  
  
1. Créez un nouveau document Microsoft Word.  
  
2. Collez le texte suivant dans le nouveau document :  
  
    ```  
    Parsing WordprocessingML with LINQ to XML  
  
    The following example prints to the console.  
  
    using System;  
  
    class Program {  
        public static void Main(string[] args) {  
            Console.WriteLine("Hello World");  
        }  
    }  
  
    This example produces the following output:  
  
    Hello World  
    ```  
  
3. Mettez en forme la première ligne à l'aide du style « Titre 1 ».  
  
4. Sélectionnez les lignes qui contiennent le code C#. La première ligne commence par le mot clé `using`. La dernière ligne est la dernière accolade fermante. Mettez en forme les lignes avec la police Courrier. Mettez-les en forme avec un nouveau style et nommez le nouveau style « Code ».  
  
5. Pour finir, sélectionnez la ligne entière qui contient la sortie et mettez-la en forme avec le style `Code`.  
  
6. Enregistrez le document et nommez-le SampleDoc.docx.  
  
    > [!NOTE]
    >  Si vous utilisez Microsoft Word 2003, sélectionnez **Document Word 2007** dans la liste déroulante **Type de fichier**.  
  
## <a name="see-also"></a>Voir aussi

- [Tutoriel : manipulation de contenu dans un document WordprocessingML (C#)](../../../../csharp/programming-guide/concepts/linq/tutorial-manipulating-content-in-a-wordprocessingml-document.md)
