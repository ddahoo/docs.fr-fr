---
title: "Traduction des op&#233;rateurs de requ&#234;te standard | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: a60c30fa-1e68-45fe-b984-f6abb9ede40e
caps.latest.revision: 2
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 2
---
# Traduction des op&#233;rateurs de requ&#234;te standard
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] traduit les opérateurs de requête standard en commandes SQL.  Le processeur de requêtes de la base de données détermine la sémantique d'exécution de la traduction SQL.  
  
 Les opérateurs de requête standard sont définis par rapport à des *séquences*.  Une séquence est *ordonnée* et s'appuie sur l'identité de référence pour chaque élément de la séquence.  Pour plus d'informations, consultez [Standard Query Operators Overview](../../../../../../ocs/visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md).  
  
 SQL traite essentiellement des *jeux de valeurs non ordonnés*.  Le classement est généralement une opération de post\-traitement déclarée explicitement appliquée au résultat final d'une requête plutôt qu'à des résultats intermédiaires.  L'identité est définie par des valeurs.  Pour cette raison, les requêtes SQL sont censées traiter les multijeux \(*conteneurs*\) plutôt que les *jeux*.  
  
 Les paragraphes suivants décrivent les différences entre les opérateurs de requête standard et leur traduction SQL pour le fournisseur SQL Server de [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].  
  
## Prise en charge des opérateurs  
  
### Concat  
 La méthode <xref:System.Linq.Enumerable.Concat%2A> est définie pour des multijeux ordonnés lorsque les ordres du récepteur et de l'argument sont identiques.  <xref:System.Linq.Enumerable.Concat%2A> fonctionne comme `UNION ALL` sur les multijeux suivis de l'ordre courant.  
  
 L'étape finale est le classement dans SQL avant que les résultats ne soient générés.  <xref:System.Linq.Enumerable.Concat%2A> ne conserve pas l'ordre de ses arguments.  Pour garantir un classement approprié, vous devez classer explicitement les résultats de <xref:System.Linq.Enumerable.Concat%2A>.  
  
### Intersect, Except, Union  
 Les méthodes <xref:System.Linq.Enumerable.Intersect%2A> et <xref:System.Linq.Enumerable.Except%2A> sont bien définies sur les jeux uniquement.  La sémantique pour les multijeux n'est pas définie.  
  
 La méthode <xref:System.Linq.Enumerable.Union%2A> est définie pour les multijeux comme la concaténation non ordonnée des multijeux \(le résultat de la clause UNION ALL dans SQL\).  
  
### Take, Skip  
 Les méthodes <xref:System.Linq.Enumerable.Take%2A> et <xref:System.Linq.Enumerable.Skip%2A> sont bien définies sur les *jeux ordonnés* uniquement.  La sémantique des jeux non ordonnés ou des multijeux n'est pas définie.  
  
> [!NOTE]
>  <xref:System.Linq.Enumerable.Take%2A> et <xref:System.Linq.Enumerable.Skip%2A> sont soumis à certaines limites lorsqu'ils sont utilisés dans des requêtes SQL Server 2000.  Pour plus d'informations, consultez « Exceptions d'ignorance \(Skip\) et d'acceptation \(Take\) dans SQL Server 2000 » dans [Résolution des problèmes](../../../../../../docs/framework/data/adonet/sql/linq/troubleshooting.md).  
  
 Du fait des restrictions des classements dans SQL, [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] tente de déplacer le classement de l'argument de ces méthodes vers le résultat de la méthode.  Prenons l'exemple de la requête [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] suivante :  
  
 [!code-csharp[DLinqSQOTranslation#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSQOTranslation/cs/Program.cs#1)]
 [!code-vb[DLinqSQOTranslation#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSQOTranslation/vb/Module1.vb#1)]  
  
 Le SQL généré pour ce code déplace le classement à la fin, comme suit :  
  
```  
SELECT TOP 1 [t0].[CustomerID], [t0].[CompanyName],  
FROM [Customers] AS [t0]  
WHERE (NOT (EXISTS(  
    SELECT NULL AS [EMPTY]  
    FROM (  
        SELECT TOP 1 [t1].[CustomerID]  
        FROM [Customers] AS [t1]  
        WHERE [t1].[City] = @p0  
        ORDER BY [t1].[CustomerID]  
        ) AS [t2]  
    WHERE [t0].[CustomerID] = [t2].[CustomerID]  
    ))) AND ([t0].[City] = @p1)  
ORDER BY [t0].[CustomerID]  
```  
  
 Il devient évident que le classement spécifié doit être cohérent lorsque <xref:System.Linq.Enumerable.Take%2A> et <xref:System.Linq.Enumerable.Skip%2A> sont enchaînés.  Si ce n'est pas le cas, les résultats ne sont pas définis.  
  
 <xref:System.Linq.Enumerable.Take%2A> et <xref:System.Linq.Enumerable.Skip%2A> sont bien définis pour des arguments intégraux constants non négatifs basés sur la spécification de l'opérateur de requête standard.  
  
### Opérateurs sans traduction  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ne traduit pas les méthodes suivantes.  La raison la plus courante est la différence entre les multijeux non ordonnés et les séquences.  
  
|Opérateurs|Raisonnement|  
|----------------|------------------|  
|<xref:System.Linq.Enumerable.TakeWhile%2A>, <xref:System.Linq.Enumerable.SkipWhile%2A>|Les requêtes SQL fonctionnent sur des multijeux et non sur des séquences.  `ORDER BY` doit être la dernière clause appliquée aux résultats.  Il n'existe donc aucune traduction à caractère général pour ces deux méthodes.|  
|<xref:System.Linq.Enumerable.Reverse%2A>|La traduction de cette méthode est possible pour un jeu ordonné mais n'est pas fournie actuellement par [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].|  
|<xref:System.Linq.Enumerable.Last%2A>, <xref:System.Linq.Enumerable.LastOrDefault%2A>|La traduction de ces méthodes est possible pour un jeu ordonné mais n'est pas fournie actuellement par [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].|  
|<xref:System.Linq.Enumerable.ElementAt%2A>, <xref:System.Linq.Enumerable.ElementAtOrDefault%2A>|Les requêtes SQL fonctionnent sur des multijeux et non sur des séquences indexables.|  
|<xref:System.Linq.Enumerable.DefaultIfEmpty%2A> \(surcharge avec argument par défaut\)|En général, il est impossible de spécifier une valeur par défaut pour un tuple arbitraire.  Les valeurs null pour les tuples sont possibles dans certains cas par le biais de jointures externes.|  
  
## Traduction d'expressions  
  
### Sémantique Null  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] n'impose pas de sémantique de comparaison null sur SQL.  Les opérateurs de comparaison sont traduits syntaxiquement dans leurs équivalents SQL.  Pour cette raison, la sémantique reflète la sémantique SQL définie par le serveur ou les paramètres de connexion.  Par exemple, deux valeurs Null sont considérées comme différentes selon les paramètres SQL Server, mais vous pouvez modifier les paramètres pour changer la sémantique.  [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ne tient pas compte des paramètres du serveur lorsqu'il traduit des requêtes.  
  
 Une comparaison avec le littéral null est traduite dans la version SQL appropriée \(`is null` ou `is not null`\).  
  
 La valeur `null` dans le classement est définie par SQL Server.  [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ne change pas le classement.  
  
### Agrégats  
 La méthode d'agrégation \(opérateur de requête standard\) <xref:System.Linq.Enumerable.Sum%2A> prend la valeur zéro pour une séquence vide ou contenant uniquement des valeurs null.  Dans [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)], la sémantique de SQL reste inchangée et <xref:System.Linq.Enumerable.Sum%2A> prend la valeur `null` plutôt que la valeur zéro pour une séquence vide ou contenant uniquement des valeurs null.  
  
 Les limitations SQL sur les résultats intermédiaires s'appliquent aux agrégats dans [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].  Le <xref:System.Linq.Enumerable.Sum%2A> de quantités d'entiers 32 bits n'est pas calculé à partir des résultats 64 bits. Un dépassement de capacité peut se produire pour une traduction [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] de <xref:System.Linq.Enumerable.Sum%2A>, même si l'implémentation de l'opérateur de requête standard n'entraîne pas de dépassement de capacité pour la séquence en mémoire correspondante.  
  
 De même, la traduction [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] de <xref:System.Linq.Enumerable.Average%2A> pour des valeurs entières est calculée comme un `integer` et non comme un `double`.  
  
### Arguments d'entité  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] permet d'utiliser des types d'entité dans les méthodes <xref:System.Linq.Enumerable.GroupBy%2A> et <xref:System.Linq.Enumerable.OrderBy%2A>.  Dans la traduction de ces opérateurs, l'utilisation d'un argument d'un type est considérée comme équivalente à la spécification de tous les membres de ce type.  Par exemple, le code suivant est équivalent :  
  
 [!code-csharp[DLinqSQOTranslation#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSQOTranslation/cs/Program.cs#2)]
 [!code-vb[DLinqSQOTranslation#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSQOTranslation/vb/Module1.vb#2)]  
  
### Arguments égaux\/comparables  
 L'implémentation des méthodes suivantes nécessite l'égalité des arguments :  
  
 <xref:System.Linq.Enumerable.Contains%2A>  
  
 <xref:System.Linq.Enumerable.Skip%2A>  
  
 <xref:System.Linq.Enumerable.Union%2A>  
  
 <xref:System.Linq.Enumerable.Intersect%2A>  
  
 <xref:System.Linq.Enumerable.Except%2A>  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] prend en charge l'égalité et la comparaison pour les arguments *plats*, mais pas pour les arguments qui contiennent des séquences ou correspondent à des séquences.  Un argument plat est un type qui peut être mappé à une ligne SQL.  Une projection d'un ou de plusieurs types d'entité qui peuvent être déterminés statiquement comme ne contenant pas de séquence est considérée comme un argument plat.  
  
 Des exemples d'arguments plats sont présentés ci\-dessous :  
  
 [!code-csharp[DLinqSQOTranslation#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSQOTranslation/cs/Program.cs#3)]
 [!code-vb[DLinqSQOTranslation#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSQOTranslation/vb/Module1.vb#3)]  
  
 Des éléments d'arguments non plats \(hiérarchiques\) sont présentés ci\-dessous.  
  
 [!code-csharp[DLinqSQOTranslation#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSQOTranslation/cs/Program.cs#4)]
 [!code-vb[DLinqSQOTranslation#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSQOTranslation/vb/Module1.vb#4)]  
  
### Traduction de fonctions Visual Basic  
 Les fonctions d'assistance suivantes utilisées par le compilateur Visual Basic sont traduites en opérateurs et en fonctions SQL correspondants :  
  
 `CompareString`  
  
 `DateTime.Compare`  
  
 `Decimal.Compare`  
  
 `IIf (in Microsoft.VisualBasic.Interaction)`  
  
 Méthodes de conversion :  
  
|||||  
|-|-|-|-|  
|ToBoolean|ToSByte|ToByte|ToChar|  
|ToCharArrayRankOne|ToDate|ToDecimal|ToDouble|  
|ToInteger|ToUInteger|ToLong|ToULong|  
|ToShort|ToUShort|ToSingle|ToString|  
  
## Prise en charge de l'héritage  
  
### Restrictions du mappage d'héritage  
 Pour plus d'informations, consultez [Procédure : mapper des hiérarchies d'héritage](../../../../../../docs/framework/data/adonet/sql/linq/how-to-map-inheritance-hierarchies.md).  
  
### Héritage dans les requêtes  
 Les casts C\# sont pris en charge uniquement dans la projection.  Les casts utilisés ailleurs ne sont pas traduits et sont ignorés.  Hormis les noms de fonction SQL, SQL effectue uniquement l'équivalent du <xref:System.Convert> CLR.  Autrement dit, SQL peut modifier la valeur d'un type en un autre.  Le cast CLR n'a pas d'équivalent car il n'existe pas de concept de réinterprétation des mêmes bits que ceux d'un autre type.  C'est pourquoi un cast C\# fonctionne uniquement en local.  Il n'est pas mis à distance.  
  
 Les opérateurs `is` et `as` et la méthode `GetType` ne sont pas restreints à l'opérateur `Select`.  Ils peuvent également être utilisés dans d'autres opérateurs de requête.  
  
## Prise en charge de SQL Server 2008  
 À partir du .NET Framework version 3.5 SP1, LINQ to SQL prend en charge le mappage aux nouveaux types de date et d'heure introduits avec SQL Server 2008.  Cependant, il existe certaines limites concernant les opérateurs de requête LINQ to SQL pris en charge pour les valeurs mappées à ces nouveaux types.  
  
### Opérateurs de requête non pris en charge  
 Les opérateurs de requête suivants ne sont pas pris en charge pour les valeurs mappées aux nouveaux types de date et d'heure SQL Server : `DATETIME2`, `DATE`, `TIME` et `DATETIMEOFFSET`.  
  
-   `Aggregate`  
  
-   `Average`  
  
-   `LastOrDefault`  
  
-   `OfType`  
  
-   `Sum`  
  
 Pour plus d'informations sur le mappage à ces types de date et d'heure SQL Server, consultez [Mappage de type SQL\-CLR](../../../../../../docs/framework/data/adonet/sql/linq/sql-clr-type-mapping.md).  
  
## Prise en charge de SQL Server 2005  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ne prend pas en charge les fonctionnalités SQL Server 2005 suivantes :  
  
-   Procédures stockées écrites pour le CLR SQL.  
  
-   Type défini par l'utilisateur.  
  
-   Fonctionnalités de requête XML.  
  
## Prise en charge de SQL Server 2000  
 Les limitations [!INCLUDE[ss2k](../../../../../../includes/ss2k-md.md)] suivantes \(par rapport à [!INCLUDE[sqprsqext](../../../../../../includes/sqprsqext-md.md)]\) affectent la prise en charge de [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)].  
  
### Opérateurs Cross Apply et Outer Apply  
 Ces opérateurs ne sont pas disponibles dans [!INCLUDE[ss2k](../../../../../../includes/ss2k-md.md)].  [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] tente une série de réécritures pour les remplacer par des jointures appropriées.  
  
 `Cross Apply` et `Outer Apply` sont générés pour les navigations dans les relations.  Le jeu des requêtes pour lequel ces réécritures sont possibles n'est pas bien défini.  Le jeu de requêtes minimal pris en charge pour [!INCLUDE[ss2k](../../../../../../includes/ss2k-md.md)] est donc celui qui n'implique pas de navigation dans les relations.  
  
### text \/ ntext  
 Les types de données `text`\/`ntext` ne peuvent pas être utilisés dans certaines opérations de requête sur `varchar(max)`\/`nvarchar(max)`, qui sont prises en charge par [!INCLUDE[sqprsqext](../../../../../../includes/sqprsqext-md.md)].  
  
 Aucune résolution n'est disponible pour cette limitation.  Précisément, vous ne pouvez pas utiliser `Distinct()` sur des résultats contenant des membres mappés à des colonnes `text` ou `ntext`.  
  
### Comportement déclenché par les sous\-requêtes  
 Le classeur [!INCLUDE[ss2k](../../../../../../includes/ss2k-md.md)] \(via SP4\) présente des spécificités qui sont déclenchées par les sous\-requêtes.  Le jeu de requêtes SQL qui déclenche ces spécificités n'est pas bien défini.  C'est pourquoi vous ne pouvez pas définir le jeu des requêtes [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] qui peut entraîner des exceptions SQL Server.  
  
### Opérateurs Skip et Take  
 <xref:System.Linq.Enumerable.Take%2A> et <xref:System.Linq.Enumerable.Skip%2A> sont soumis à certaines restrictions lorsqu'ils sont utilisés dans des requêtes sur [!INCLUDE[ss2k](../../../../../../includes/ss2k-md.md)].  Pour plus d'informations, consultez « Exceptions d'ignorance \(Skip\) et d'acceptation \(Take\) dans SQL Server 2000 » dans [Résolution des problèmes](../../../../../../docs/framework/data/adonet/sql/linq/troubleshooting.md).  
  
## Matérialisation d'objet  
 La matérialisation crée des objets CLR à partir de lignes retournées par une ou plusieurs requêtes SQL.  
  
-   Les appels suivants sont *exécutés localement* dans le cadre de la matérialisation :  
  
    -   Constructeurs  
  
    -   Méthodes `ToString` dans les projections  
  
    -   Casts de type dans les projections  
  
-   Les méthodes qui suivent la méthode <xref:System.Linq.Enumerable.AsEnumerable%2A> sont *exécutées localement*.  Cette méthode ne provoque pas d'exécution immédiate.  
  
-   Vous pouvez utiliser un `struct` comme type de retour d'un résultat de requête ou comme membre du type de résultat.  Les entités doivent être des classes.  Les types anonymes sont matérialisés en instances de classe, mais les structs nommés \(non entités\) peuvent être utilisés dans la projection.  
  
-   Un membre du type de retour d'un résultat de requête peut être de type <xref:System.Linq.IQueryable%601>.  Il est matérialisé en collection locale.  
  
-   Les méthodes suivantes provoquent la *matérialisation immédiate* de la séquence à laquelle les méthodes s'appliquent :  
  
    -   <xref:System.Linq.Enumerable.ToList%2A>  
  
    -   <xref:System.Linq.Enumerable.ToDictionary%2A>  
  
    -   <xref:System.Linq.Enumerable.ToArray%2A>  
  
## Voir aussi  
 [Reference](../../../../../../docs/framework/data/adonet/sql/linq/reference.md)   
 [Retourner ou ignorer des éléments d'une séquence](../../../../../../docs/framework/data/adonet/sql/linq/return-or-skip-elements-in-a-sequence.md)   
 [Concaténer deux séquences](../../../../../../docs/framework/data/adonet/sql/linq/concatenate-two-sequences.md)   
 [Retourner la différence définie entre deux séquences](../../../../../../docs/framework/data/adonet/sql/linq/return-the-set-difference-between-two-sequences.md)   
 [Retourner l'intersection définie de deux séquences](../../../../../../docs/framework/data/adonet/sql/linq/return-the-set-intersection-of-two-sequences.md)   
 [Retourner l'union définie de deux séquences](../../../../../../docs/framework/data/adonet/sql/linq/return-the-set-union-of-two-sequences.md)