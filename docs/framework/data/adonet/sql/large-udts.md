---
title: Grands types définis par l'utilisateur
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 420ae24e-762b-4e09-b4c3-2112c470ee49
ms.openlocfilehash: 2114efcc4d39cb4d2ea9ca33d7ff244c81a7097f
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64650575"
---
# <a name="large-udts"></a>Grands types définis par l'utilisateur
Les types définis par l'utilisateur (UDT, User-Defined Types) permettent à un développeur d'étendre le système de type scalaire du serveur, en stockant des objets CLR (Common Language Runtime) dans une base de données SQL Server. Les UDT peuvent contenir plusieurs éléments et, contrairement aux types de données alias traditionnels, avoir des comportements qui consistent en un unique type de données système SQL Server.  
  
> [!NOTE]
>  Vous devez installer le .NET Framework 3.5 SP1 (ou version ultérieure) pour tirer parti de la prise en charge améliorée de SqlClient pour les UDT volumineux.  
  
 Auparavant, les UDT étaient limités à une taille maximale de 8 Ko. Dans SQL Server 2008, cette restriction a été supprimée pour les UDT ayant un format <xref:Microsoft.SqlServer.Server.Format.UserDefined>.  
  
 Pour la documentation complète relative aux types définis par l'utilisateur, consultez la version de la documentation en ligne de SQL Server correspondant à la version de SQL Server que vous utilisez.  
  
 **Documentation en ligne de SQL Server**  
  
1. [Types CLR définis par l’utilisateur](https://go.microsoft.com/fwlink/?LinkId=98366)  
  
## <a name="retrieving-udt-schemas-using-getschema"></a>Récupération de schémas UDT à l'aide de GetSchema  
 La méthode <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> de <xref:System.Data.SqlClient.SqlConnection> retourne les informations de schéma de la base de données dans un objet <xref:System.Data.DataTable>. Pour plus d’informations, consultez [Collections de schémas SQL Server](../../../../../docs/framework/data/adonet/sql-server-schema-collections.md).  
  
### <a name="getschematable-column-values-for-udts"></a>Valeurs de colonne GetSchemaTable Column pour les UDT  
 La méthode <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A> d'un objet <xref:System.Data.SqlClient.SqlDataReader> retourne un objet <xref:System.Data.DataTable> qui décrit les métadonnées des colonnes. Le tableau suivant décrit les différences dans les métadonnées des colonnes pour les UDT volumineux entre SQL Server 2005 et SQL Server 2008.  
  
|Colonne SqlDataReader|SQL Server 2005|SQL Server 2008 et versions ultérieures|  
|--------------------------|---------------------|-------------------------------|  
|`ColumnSize`|Selon le cas|Selon le cas|  
|`NumericPrecision`|255|255|  
|`NumericScale`|255|255|  
|`DataType`|`Byte[]`|Instance UDT|  
|`ProviderSpecificDataType`|`SqlTypes.SqlBinary`|Instance UDT|  
|`ProviderType`|21 (`SqlDbType.VarBinary`)|29 (`SqlDbType.Udt`)|  
|`NonVersionedProviderType`|29 (`SqlDbType.Udt`)|29 (`SqlDbType.Udt`)|  
|`DataTypeName`|`SqlDbType.VarBinary`|Nom spécifié en tant qu’en trois parties *Database.SchemaName.TypeName*.|  
|`IsLong`|Selon le cas|Selon le cas|  
  
## <a name="sqldatareader-considerations"></a>Considérations relatives à SqlDataReader  
 L'objet <xref:System.Data.SqlClient.SqlDataReader> a été étendu depuis SQL Server 2008 pour prendre en charge la récupération des valeurs UDT volumineuses. La manière dont un <xref:System.Data.SqlClient.SqlDataReader> traite les valeurs UDT volumineuses par dépend de la version de SQL Server que vous utilisez, ainsi que du `Type System Version` spécifié dans la chaîne de connexion. Pour plus d'informations, consultez <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
 Les méthodes suivantes de <xref:System.Data.SqlClient.SqlDataReader> retournent un <xref:System.Data.SqlTypes.SqlBinary> au lieu d'un type défini par l'utilisateur lorsque `Type System Version` est défini à SQL Server 2005 :  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificFieldType%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetProviderSpecificValues%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetSqlValues%2A>  
  
 Les méthodes suivantes retournent un tableau de `Byte[]` au lieu d'un UDT lorsque `Type System Version` est défini à SQL Server 2005 :  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValue%2A>  
  
- <xref:System.Data.SqlClient.SqlDataReader.GetValues%2A>  
  
 Notez qu'aucune conversion n'est effectuée pour la version actuelle d'ADO.NET.  
  
## <a name="specifying-sqlparameters"></a>Spécification de SqlParameters  
 Les propriétés <xref:System.Data.SqlClient.SqlParameter> suivantes ont été étendues pour fonctionner avec des UDT volumineux.  
  
|Propriété SqlParameter|Description|  
|---------------------------|-----------------|  
|<xref:System.Data.SqlClient.SqlParameter.Value%2A>|Obtient ou définit un objet qui représente la valeur du paramètre. La valeur par défaut correspond à la valeur null. La propriété peut être de type `SqlBinary`, `Byte[]` ou un objet managé.|  
|<xref:System.Data.SqlClient.SqlParameter.SqlValue%2A>|Obtient ou définit un objet qui représente la valeur du paramètre. La valeur par défaut correspond à la valeur null. La propriété peut être de type `SqlBinary`, `Byte[]` ou un objet managé.|  
|<xref:System.Data.SqlClient.SqlParameter.Size%2A>|Obtient ou définit la taille de la valeur de paramètre à résoudre. La valeur par défaut est 0. La propriété peut être un nombre entier qui représente la taille de la valeur de paramètre. Pour les UDT volumineux, il peut s'agir de la taille réelle de l'UDT ou -1 pour inconnu.|  
  
## <a name="retrieving-data-example"></a>Exemple d'extraction de données  
 Le fragment de code suivant montre comment extraire des données UDT volumineuses. La variable `connectionString` suppose une connexion valide à une base de données SQL Server, et la variable `commandString` suppose une instruction SELECT valide avec la colonne de clé primaire apparaissant en premier.  
  
```csharp  
using (SqlConnection connection = new SqlConnection(   
    connectionString, commandString))  
{  
  connection.Open();  
  SqlCommand command = new SqlCommand(commandString);  
  SqlDataReader reader = command.ExecuteReader();  
  while (reader.Read())  
  {  
    // Retrieve the value of the Primary Key column.  
    int id = reader.GetInt32(0);  
  
    // Retrieve the value of the UDT.  
    LargeUDT udt = (LargeUDT)reader[1];  
  
    // You can also use GetSqlValue and GetValue.  
    // LargeUDT udt = (LargeUDT)reader.GetSqlValue(1);  
    // LargeUDT udt = (LargeUDT)reader.GetValue(1);  
  
    Console.WriteLine(  
     "ID={0} LargeUDT={1}", id, udt);  
  }  
reader.close  
}  
```  
  
```vb  
Using connection As New SqlConnection( _  
    connectionString, commandString)  
    connection.Open()  
    Dim command As New SqlCommand(commandString, connection)  
    Dim reader As SqlDataReader  
    reader = command.ExecuteReader  
  
    While reader.Read()  
      ' Retrieve the value of the Primary Key column.  
      Dim id As Int32 = reader.GetInt32(0)  
  
      ' Retrieve the value of the UDT.  
      Dim udt As LargeUDT = CType(reader(1), LargeUDT)  
  
     ' You can also use GetSqlValue and GetValue.  
     ' Dim udt As LargeUDT = CType(reader.GetSqlValue(1), LargeUDT)  
     ' Dim udt As LargeUDT = CType(reader.GetValue(1), LargeUDT)  
  
      ' Print values.  
      Console.WriteLine("ID={0} LargeUDT={1}", id, udt)  
    End While  
    reader.Close()  
End Using  
```  
  
## <a name="see-also"></a>Voir aussi

- [Configuration des paramètres et des types de données des paramètres](../../../../../docs/framework/data/adonet/configuring-parameters-and-parameter-data-types.md)
- [Récupération des informations de schéma de base de données](../../../../../docs/framework/data/adonet/retrieving-database-schema-information.md)
- [Mappages de types de données SQL Server](../../../../../docs/framework/data/adonet/sql-server-data-type-mappings.md)
- [Données binaires et de valeur élevée SQL Server](../../../../../docs/framework/data/adonet/sql/sql-server-binary-and-large-value-data.md)
- [Fournisseurs managés ADO.NET et centre de développement DataSet](https://go.microsoft.com/fwlink/?LinkId=217917)
