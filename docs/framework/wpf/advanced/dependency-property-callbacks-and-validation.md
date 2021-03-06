---
title: Validation et rappels de propriétés de dépendance
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dependency properties [WPF], validation
- coerce value callbacks [WPF]
- callbacks [WPF], validation
- dependency properties [WPF], callbacks
- validation of dependency properties [WPF]
ms.assetid: 48db5fb2-da7f-49a6-8e81-3540e7b25825
ms.openlocfilehash: 95a40b4a357b1a601eced6c8e5214871b95fcbd2
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61928555"
---
# <a name="dependency-property-callbacks-and-validation"></a>Validation et rappels de propriétés de dépendance
Cette rubrique décrit comment créer des propriétés de dépendance à l’aide d’autres implémentations personnalisées pour des fonctionnalités liées aux propriétés telles que la détermination de la validation, les rappels effectués chaque fois que la valeur effective de la propriété change, et la non prise en compte des possibles influences extérieures sur la détermination de la valeur. Elle décrit également les scénarios où il est nécessaire de développer les comportements par défaut du système de propriétés à l’aide de ces techniques.  

<a name="prerequisites"></a>   
## <a name="prerequisites"></a>Prérequis  
 Cette rubrique part du principe que vous comprenez les scénarios de base de l’implémentation d’une propriété de dépendance et que vous savez comment les métadonnées sont appliquées à une propriété de dépendance personnalisée. Pour plus d’informations sur le contexte, consultez [Propriétés de dépendance personnalisées](custom-dependency-properties.md) et [Métadonnées de propriété de dépendance](dependency-property-metadata.md).  
  
<a name="Validation_Callbacks"></a>   
## <a name="validation-callbacks"></a>Rappels de validation  
 Les rappels de validation peuvent être attribués à une propriété de dépendance quand vous l’inscrivez initialement. Le rappel de validation ne fait pas partie des métadonnées de propriété ; Il s’agit d’une entrée directe de la <xref:System.Windows.DependencyProperty.Register%2A> (méthode). Par conséquent, une fois qu’un rappel de validation a été créé pour une propriété de dépendance, il ne peut pas être remplacé par une nouvelle implémentation.  
  
 [!code-csharp[DPCallbackOverride#CurrentDefinitionWithWrapper](~/samples/snippets/csharp/VS_Snippets_Wpf/DPCallbackOverride/CSharp/SDKSampleLibrary/class1.cs#currentdefinitionwithwrapper)]
 [!code-vb[DPCallbackOverride#CurrentDefinitionWithWrapper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DPCallbackOverride/visualbasic/sdksamplelibrary/class1.vb#currentdefinitionwithwrapper)]  
  
 Les rappels sont implémentés de manière à ce qu’une valeur d’objet leur soit fournie. Ils retournent `true` si la valeur fournie est valide pour la propriété ; sinon, ils retournent `false`. Il est supposé que la propriété est du type correct inscrit auprès du système de propriétés. Ainsi, la vérification du type dans les rappels n’est pas effectuée habituellement. Les rappels sont utilisés par le système de propriétés dans diverses opérations différentes. Cela inclut l’initialisation du type initial par valeur par défaut, un changement par programmation en appelant <xref:System.Windows.DependencyObject.SetValue%2A>, ou essaie de substituer des métadonnées avec la nouvelle valeur par défaut fournie. Si le rappel de validation est appelé par l’une de ces opérations et qu’il retourne `false`, une exception est levée. Les développeurs d’application doivent être prêts à gérer ces exceptions. L’une des utilisations courantes des rappels de validation consiste à valider des valeurs d’énumération, ou à contraindre des valeurs d’entiers ou des valeurs doubles quand la propriété définit des mesures qui doivent être supérieures ou égales à zéro.  
  
 Les rappels de validation sont spécifiquement prévus pour être des validateurs de classe, et non des validateurs d’instance. Les paramètres du rappel ne communiquent pas un spécifique <xref:System.Windows.DependencyObject> sur lequel les propriétés à valider sont définies. Par conséquent, les rappels de validation ne sont pas utiles pour appliquer les dépendances « possibles » qui peuvent influencer une valeur de propriété, où la valeur propre à l’instance d’une propriété dépend de facteurs tels que des valeurs propres à l’instance d’autres propriétés ou l’état d’exécution.  
  
 Voici des exemples de code pour un scénario de rappel de validation très simple : confirmation du fait qu’une propriété est typée en tant que le <xref:System.Double> primitive n’est pas <xref:System.Double.PositiveInfinity> ou <xref:System.Double.NegativeInfinity>.  
  
 [!code-csharp[DPCallbackOverride#ValidateValueCallback](~/samples/snippets/csharp/VS_Snippets_Wpf/DPCallbackOverride/CSharp/SDKSampleLibrary/class1.cs#validatevaluecallback)]
 [!code-vb[DPCallbackOverride#ValidateValueCallback](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DPCallbackOverride/visualbasic/sdksamplelibrary/class1.vb#validatevaluecallback)]  
  
<a name="Coerce_Value_Callbacks_and_Property_Changed_Events"></a>   
## <a name="coerce-value-callbacks-and-property-changed-events"></a>Rappels de forçage de valeurs et événements de modification de propriété  
 Forçage de valeurs rappels versement spécifiques <xref:System.Windows.DependencyObject> instance pour les propriétés, comme <xref:System.Windows.PropertyChangedCallback> implémentations qui sont appelées par le système de propriétés chaque fois que la valeur d’une propriété de dépendance change. À l’aide de ces deux rappels en combinaison, vous pouvez créer une série de propriétés sur des éléments où les modifications d’une propriété forcent la réévaluation d’une autre propriété.  
  
 Un scénario typique d’utilisation d’un chaînage de propriétés de dépendance est quand vous définissez une propriété pilotée par l’interface utilisateur où l’élément contient une propriété pour la valeur minimale, une autre propriété pour la valeur maximale et une troisième propriété pour la valeur réelle ou actuelle. Dans ce cas, si la valeur maximale a été modifiée de telle sorte que la valeur actuelle dépasse la nouvelle valeur maximale, vous souhaiterez forcer la valeur actuelle pour qu’elle n’excède pas la nouvelle valeur maximale (et de même en ce qui concerne la valeur minimale).  
  
 Le court exemple de code ci-dessous concerne l’une des trois propriétés de dépendance qui illustrent cette relation. Il montre comment la propriété `CurrentReading` d’un ensemble Min/Max/Current de propriétés Reading est inscrit. Il utilise la validation comme illustré dans la section précédente.  
  
 [!code-csharp[DPCallbackOverride#CurrentDefinitionWithWrapper](~/samples/snippets/csharp/VS_Snippets_Wpf/DPCallbackOverride/CSharp/SDKSampleLibrary/class1.cs#currentdefinitionwithwrapper)]
 [!code-vb[DPCallbackOverride#CurrentDefinitionWithWrapper](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DPCallbackOverride/visualbasic/sdksamplelibrary/class1.vb#currentdefinitionwithwrapper)]  
  
 Le rappel de modification de propriété pour Current est utilisé pour transférer la modification à d’autres propriétés dépendantes, en appelant explicitement les rappels de forçage de valeur inscrits pour ces autres propriétés :  
  
 [!code-csharp[DPCallbackOverride#OnPCCurrent](~/samples/snippets/csharp/VS_Snippets_Wpf/DPCallbackOverride/CSharp/SDKSampleLibrary/class1.cs#onpccurrent)]
 [!code-vb[DPCallbackOverride#OnPCCurrent](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DPCallbackOverride/visualbasic/sdksamplelibrary/class1.vb#onpccurrent)]  
  
 Le rappel de forçage de valeur vérifie les valeurs des propriétés dont dépend potentiellement la propriété actuelle, et force la valeur actuelle si nécessaire :  
  
 [!code-csharp[DPCallbackOverride#CoerceCurrent](~/samples/snippets/csharp/VS_Snippets_Wpf/DPCallbackOverride/CSharp/SDKSampleLibrary/class1.cs#coercecurrent)]
 [!code-vb[DPCallbackOverride#CoerceCurrent](~/samples/snippets/visualbasic/VS_Snippets_Wpf/DPCallbackOverride/visualbasic/sdksamplelibrary/class1.vb#coercecurrent)]  
  
> [!NOTE]
>  Les valeurs par défaut des propriétés ne sont pas forcées. Une valeur de propriété égale à la valeur par défaut peut se produire si une valeur de propriété a toujours sa valeur par défaut initiale, ou via l’effacement d’autres valeurs avec <xref:System.Windows.DependencyObject.ClearValue%2A>.  
  
 Les rappels de forçage de valeur et de modification de propriété font partie des métadonnées de propriété. Ainsi, vous pouvez changer les rappels pour une propriété de dépendance particulière telle qu’elle existe sur un type dérivant du type qui possède la propriété de dépendance, en substituant les métadonnées de cette propriété sur votre type.  
  
<a name="Advanced"></a>   
## <a name="advanced-coercion-and-callback-scenarios"></a>Scénarios de forçage et de rappel avancés  
  
### <a name="constraints-and-desired-values"></a>Contraintes et valeurs souhaitées  
 Le <xref:System.Windows.PropertyMetadata.CoerceValueCallback%2A> rappels seront utilisés par le système de propriétés pour forcer une valeur conformément à la logique que vous déclarez, mais une valeur forcée d’un définie localement propriété conserve une « valeur souhaitée » en interne. Si les contraintes sont basées sur d’autres valeurs de propriété qui peuvent changer de manière dynamique pendant la durée de vie de l’application, les contraintes de forçage sont aussi changées de manière dynamique, et la propriété contrainte peut modifier sa valeur pour qu’elle soit le plus proche possible de la valeur souhaitée étant donné les nouvelles contraintes. La valeur devient la valeur souhaitée si toutes les contraintes sont levées. Vous pouvez introduire potentiellement certains scénarios de dépendance relativement compliqués si vous avez plusieurs propriétés qui dépendent les unes des autres de manière circulaire. Par exemple, dans le scénario Min/Max/Current, vous pouvez choisir de faire en sorte que Minimum et Maximum soient définissables par l’utilisateur. Dans ce cas, vous devrez peut-être forcer la propriété Maximum pour qu’elle soit toujours supérieure à Minimum, et inversement. Mais si cette contrainte est active, et que la valeur de Maximum est forcée sur celle de Minimum, cela laisse Current dans un état non définissable, car cette valeur dépend des deux autres et est limitée à la plage comprise entre ces deux valeurs, qui est égale à zéro. Ensuite, si Maximum ou Minimum est ajustée, Current paraîtra « suivre » l’une des valeurs, car la valeur souhaitée de Current sera encore stockée et tentera d’atteindre la valeur souhaitée à mesure que les contraintes sont assouplies.  
  
 Il n’y a rien de techniquement incorrect avec les dépendances complexes, mais elles peuvent réduire légèrement les performances si elles nécessitent un grand nombre de réévaluations, et peuvent également être déconcertantes pour les utilisateurs si elles affectent directement l’interface utilisateur. Soyez prudent avec les rappels de forçage de valeur et les rappels de modification de propriété, et vérifiez que la contrainte tentée peut être traitée aussi clairement que possible et ne pas « surcontraindre ».  
  
### <a name="using-coercevalue-to-cancel-value-changes"></a>Utilisation de CoerceValue pour annuler des modifications de valeur  
 Le système de propriétés traitera tout <xref:System.Windows.CoerceValueCallback> qui retourne la valeur <xref:System.Windows.DependencyProperty.UnsetValue> comme un cas particulier. Ce cas spécial signifie que la modification de propriété qui a entraîné le <xref:System.Windows.CoerceValueCallback> appelée doit être rejetée par le système de propriétés, et que le système de propriétés doit plutôt signaler toute valeur précédente de la propriété. Ce mécanisme peut être utile pour vérifier que les modifications d’une propriété qui ont été lancées de façon asynchrone sont toujours valides pour l’état actuel de l’objet, et pour supprimer les modifications si ce n’est pas le cas. Un autre scénario possible consiste à supprimer sélectivement une valeur en fonction du composant de détermination de la valeur de propriété responsable du signalement de la valeur. Pour ce faire, vous pouvez utiliser la <xref:System.Windows.DependencyProperty> passé dans le rappel et l’identificateur de propriété comme entrée pour <xref:System.Windows.DependencyPropertyHelper.GetValueSource%2A>, puis traiter le <xref:System.Windows.ValueSource>.  
  
## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble des propriétés de dépendance](dependency-properties-overview.md)
- [Métadonnées de propriété de dépendance](dependency-property-metadata.md)
- [Propriétés de dépendance personnalisées](custom-dependency-properties.md)
