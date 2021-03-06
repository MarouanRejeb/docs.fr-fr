---
title: 'Procédure : Se connecter à une base de données'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c33d74b3-530d-421b-a121-96786dd263a5
ms.openlocfilehash: f04726bc12fdfbca530ee5533d5b8969addf962e
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62037841"
---
# <a name="how-to-connect-to-a-database"></a>Procédure : Se connecter à une base de données
Le <xref:System.Data.Linq.DataContext> est le conduit principal par lequel vous vous connectez à une base de données, récupérez des objets de celle-ci et soumettez des modifications. Vous utilisez le <xref:System.Data.Linq.DataContext> tout comme vous utiliseriez un [!INCLUDE[vstecado](../../../../../../includes/vstecado-md.md)] <xref:System.Data.SqlClient.SqlConnection>. En fait, le <xref:System.Data.Linq.DataContext> est initialisé avec une connexion ou une chaîne de connexion que vous fournissez. Pour plus d’informations, consultez [DataContext, méthodes (Concepteur O/R)](/visualstudio/data-tools/datacontext-methods-o-r-designer).  
  
 Le rôle de <xref:System.Data.Linq.DataContext> est de traduire vos demandes d'objets en requêtes SQL sur la base de données, puis de rassembler les objets parmi les résultats. Le <xref:System.Data.Linq.DataContext> active [!INCLUDE[vbteclinqext](../../../../../../includes/vbteclinqext-md.md)] en implémentant le même modèle d’opérateur que les opérateurs de requête standard, tels que `Where` et `Select`.  
  
> [!IMPORTANT]
>  Il est primordial de maintenir une connexion sécurisée. Pour plus d’informations, consultez [sécurité dans LINQ to SQL](../../../../../../docs/framework/data/adonet/sql/linq/security-in-linq-to-sql.md).  
  
## <a name="example"></a>Exemple  
 Dans l'exemple suivant, le <xref:System.Data.Linq.DataContext> est utilisé pour se connecter à l'exemple de base de données Northwind et récupérer des lignes de clients dont la ville est Londres.  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#1)]
 [!code-vb[DLinqCommunicatingWithDatabase#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#1)]  
  
 Chaque table de base de données est représentée comme une collection `Table` disponible par le biais de la méthode <xref:System.Data.Linq.DataContext.GetTable%2A> en utilisant la classe d’entité pour l’identifier.  
  
## <a name="example"></a>Exemple  
 Il est recommandé de déclarer un <xref:System.Data.Linq.DataContext> fortement typé au lieu de s'appuyer sur la classe <xref:System.Data.Linq.DataContext> de base et sur la méthode <xref:System.Data.Linq.DataContext.GetTable%2A>. Un <xref:System.Data.Linq.DataContext> fortement typé déclare toutes les collections `Table` comme membres du contexte, comme dans l'exemple suivant.  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#2)]
 [!code-vb[DLinqCommunicatingWithDatabase#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#2)]  
  
 Vous pouvez exprimer ensuite plus simplement la requête de clients de Londres comme suit :  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#5)]
 [!code-vb[DLinqCommunicatingWithDatabase#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#5)]  
  
## <a name="see-also"></a>Voir aussi

- [Communication avec la base de données](../../../../../../docs/framework/data/adonet/sql/linq/communicating-with-the-database.md)
