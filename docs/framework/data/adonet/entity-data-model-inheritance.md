---
title: "Entity Data Model&#160;: h&#233;ritage | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-ado"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 42c7ef24-710a-4af9-8493-cd41c399ecb0
caps.latest.revision: 3
author: "JennieHubbard"
ms.author: "jhubbard"
manager: "jhubbard"
caps.handback.revision: 3
---
# Entity Data Model&#160;: h&#233;ritage
Le modèle EDM \(Entity Data Model\) prend en charge l'héritage pour les [types d'entité](../../../../docs/framework/data/adonet/entity-type.md).  L'héritage dans le modèle EDM est semblable à l'héritage pour les classes dans les langages de programmation orientés objet.  Comme pour les classes dans les langages orientés objet, vous pouvez définir un type d'entité \(*type dérivé*\) qui hérite d'un autre type d'entité \(*type de base*\) dans un modèle conceptuel.  Toutefois, contrairement aux classes dans la programmation orientée objet, le type dérivé hérite toujours l'ensemble des [propriétés](../../../../docs/framework/data/adonet/property.md) et des [propriétés de navigation](../../../../docs/framework/data/adonet/navigation-property.md) du type de base dans un modèle conceptuel.  Vous ne pouvez pas remplacer les propriétés héritées dans un type dérivé.  
  
 Dans un modèle conceptuel, vous pouvez générer des hiérarchies d'héritage dans lesquelles un type dérivé hérite d'un autre type dérivé.  Le type en haut de la hiérarchie \(celui qui n'est pas un type dérivé dans la hiérarchie\) est appelé *type racine*.  Dans une hiérarchie d'héritage, la [clé d'entité](../../../../docs/framework/data/adonet/entity-key.md) doit être définie sur le type racine.  
  
 Vous ne pouvez pas générer de hiérarchies d'héritage dans lesquelles un type dérivé hérite de plusieurs types.  Par exemple, dans un modèle conceptuel avec un type d'entité `Book`, vous pouvez définir les types dérivés `FictionBook` et `NonFictionBook` qui héritent de `Book`.  Toutefois, vous ne pouvez pas ensuite définir un type qui hérite à la fois du type `FictionBook` et du type `NonFictionBook`.  
  
## Exemple  
 Le diagramme suivant montre un modèle conceptuel avec quatre types d'entité : `Book`, `FictionBook`, `Publisher` et `Author`.  Le type d'entité `FictionBook` est un type dérivé qui hérite du type d'entité `Book`.  Le type `FictionBook` hérite les propriétés `ISBN (Key)`, `Title` et `Revision`, et définit une propriété supplémentaire appelée `Genre`.  
  
 ![Héritage](../../../../docs/framework/data/adonet/media/inheritanceexample.gif "InheritanceExample")  
  
 [ADO.NET Entity Framework](../../../../docs/framework/data/adonet/ef/index.md) utilise un langage spécifique à un domaine \(DSL\), appelé [CSDL](../../../../docs/framework/data/adonet/ef/language-reference/csdl-specification.md) \(Conceptual Schema Definition Language\), pour définir des modèles conceptuels.  Le CSDL suivant définit un type d'entité, `FictionBook`, qui hérite du type `Book` \(comme dans le diagramme ci\-dessus\) :  
  
 [!code-xml[EDM_Example_Model#DerivedType](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books5.edmx#derivedtype)]  
  
## Voir aussi  
 [Concepts clés d'Entity Data Model](../../../../docs/framework/data/adonet/entity-data-model-key-concepts.md)   
 [Entity Data Model](../../../../docs/framework/data/adonet/entity-data-model.md)