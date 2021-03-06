---
title: "Modification d’arborescences XML (LINQ to XML) (Visual Basic) | Documents Microsoft"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: 4ae511a5-4fc9-4178-9c8e-761357deae3f
caps.latest.revision: 3
author: dotnet-bot
ms.author: dotnetcontent
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: cf605973e68230e9e3e09f0abf6de49635cd5845
ms.lasthandoff: 03/13/2017


---
# <a name="modifying-xml-trees-linq-to-xml-visual-basic"></a>Modification d’arborescences XML (LINQ to XML) (Visual Basic)
[!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] est un magasin en mémoire pour une arborescence XML. Une fois que vous chargez ou une arborescence XML à partir d’une source [!INCLUDE[sqltecxlinq](../../../../csharp/programming-guide/concepts/linq/includes/sqltecxlinq_md.md)] vous permet de modifier cette arborescence sur place, puis de le sérialiser l’arborescence, peut-être l’enregistrer dans un fichier ou l’envoyer à un serveur distant.  
  
 Lorsque vous modifiez une arborescence sur place, vous utilisez certaines méthodes, telles que <xref:System.Xml.Linq.XContainer.Add%2A>.</xref:System.Xml.Linq.XContainer.Add%2A>  
  
 Il existe cependant une autre approche, qui consiste à utiliser la construction fonctionnelle pour générer une nouvelle arborescence avec une forme différente. Cette approche peut s'avérer plus robuste et plus facile à développer, selon les types de modifications que vous devez apporter à votre arborescence XML et selon la taille de l'arborescence. La première rubrique dans cette section compare ces deux approches.  
  
## <a name="in-this-section"></a>Dans cette section  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Comparaison de la modification d’arborescence XML en mémoire et de la Construction fonctionnelle (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/in-memory-xml-tree-modification-vs-functional-construction.md)|Compare la modification d’une arborescence XML en mémoire à la construction fonctionnelle.|  
|[Ajout d’éléments, attributs et nœuds à une arborescence XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/adding-elements-attributes-and-nodes-to-an-xml-tree.md)|Fournit des informations sur l’ajout d’éléments, d’attributs ou de nœuds à une arborescence XML.|  
|[Modification d’éléments, d’attributs et de nœuds dans une arborescence XML](../../../../visual-basic/programming-guide/concepts/linq/modifying-elements-attributes-and-nodes-in-an-xml-tree.md)|Fournit des informations sur la modification d'éléments, d'attributs ou de nœuds existants.|  
|[Suppression d’éléments, attributs et nœuds d’une arborescence XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/removing-elements-attributes-and-nodes-from-an-xml-tree.md)|Fournit des informations sur la suppression d’éléments, d’attributs ou de nœuds d’une arborescence XML.|  
|[Maintenance de paires nom/valeur (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/maintaining-name-value-pairs.md)|Décrit comment maintenir des informations d'applications qu'il est préférable de conserver sous la forme de paires nom/valeur, telles que des informations de configuration ou des paramètres globaux.|  
|[Comment : modifier le Namespace pour toute une arborescence XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-change-the-namespace-for-an-entire-xml-tree.md)|Montre comment déplacer une arborescence XML d’un espace de noms à un autre.|  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de programmation (LINQ to XML) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/programming-guide-linq-to-xml.md)
