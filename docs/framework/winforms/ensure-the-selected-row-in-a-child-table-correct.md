---
title: "Comment&#160;: s&#39;assurer que la ligne s&#233;lectionn&#233;e dans une table enfant reste au bon emplacement | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-winforms"
ms.tgt_pltfrm: ""
ms.topic: "article"
dev_langs: 
  - "jsharp"
helpviewer_keywords: 
  - "mise en cache (.NET Framework), position enfant"
  - "position enfant"
  - "lignes de la table enfant (sélection)"
  - "position actuelle de l'enfant"
  - "liaison de données (.NET Framework), sélection de ligne"
  - "maître/détails (mode dans Windows Forms)"
  - "mode maître/détails"
  - "parent/enfant (mode dans Windows Forms)"
  - "réinitialiser la position de l'enfant"
  - "position de ligne (Windows Forms)"
ms.assetid: c5fa2562-43a4-46fa-a604-52d8526a87bd
caps.latest.revision: 12
author: "dotnet-bot"
ms.author: "dotnetcontent"
manager: "wpickett"
caps.handback.revision: 12
---
# Comment&#160;: s&#39;assurer que la ligne s&#233;lectionn&#233;e dans une table enfant reste au bon emplacement
Souvent, lors de l'utilisation de la liaison de données dans des Windows Forms, vous affichez des données dans ce que l'on appelle une vue parent\/enfant ou maître\/détails.  Cela fait référence à un scénario de liaison de données où les données de la même source sont affichées dans deux contrôles.  La modification de la sélection dans un contrôle entraîne la modification des données affichées dans le deuxième contrôle.  Par exemple, le premier contrôle peut contenir une liste de clients et le second une liste de commandes associées au client sélectionné dans le premier contrôle.  
  
 À compter de .NET Framework version 2.0, quand vous affichez des données dans une vue parent\/enfant, vous devrez peut\-être effectuer des étapes supplémentaires pour vous assurer que la ligne actuellement sélectionnée dans la table enfant n'est pas réinitialisée à la première ligne de la table.  Pour cela, vous devez mettre en cache la position de la table enfant et la réinitialiser une fois que la table parente a changé.  En général, la réinitialisation de l'enfant se produit la première fois qu'un champ sur une ligne de la table parente change.  
  
### Pour mettre en cache la position actuelle de l'enfant  
  
1.  Déclarez une variable entière pour stocker la position de liste de l'enfant et une variable booléenne pour stocker une valeur indiquant s'il faut mettre en cache la position de l'enfant.  
  
     [!code-csharp[System.Windows.Forms.CurrencyManagerReset#4](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/CS/Form1.cs#4)]
     [!code-vb[System.Windows.Forms.CurrencyManagerReset#4](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/VB/Form1.vb#4)]  
  
2.  Gérez l'événement <xref:System.Windows.Forms.CurrencyManager.ListChanged> pour le <xref:System.Windows.Forms.CurrencyManager> de la liaison et vérifiez s'il y a un <xref:System.ComponentModel.ListChangedType> égal à <xref:System.ComponentModel.ListChangedType>.  
  
3.  Vérifiez la position actuelle du <xref:System.Windows.Forms.CurrencyManager>.  Si elle est supérieure à la première entrée de la liste \(en général 0\), enregistrez\-la dans une variable.  
  
     [!code-csharp[System.Windows.Forms.CurrencyManagerReset#2](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.CurrencyManagerReset#2](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/VB/Form1.vb#2)]  
  
4.  Gérez l'événement <xref:System.Windows.Forms.BindingManagerBase.CurrentChanged> de la liste parente pour le gestionnaire de devise parent.  Dans le gestionnaire, définissez la valeur booléenne pour indiquer qu'il ne s'agit pas d'un scénario de mise en cache.  Si l'événement <xref:System.Windows.Forms.BindingManagerBase.CurrentChanged> se produit, la modification du parent est une modification de position dans la liste et non une modification de valeur d'élément.  
  
     [!code-csharp[System.Windows.Forms.CurrencyManagerReset#5](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/CS/Form1.cs#5)]
     [!code-vb[System.Windows.Forms.CurrencyManagerReset#5](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/VB/Form1.vb#5)]  
  
### Pour réinitialiser la position de l'enfant  
  
1.  Gérez l'événement <xref:System.Windows.Forms.BindingManagerBase.PositionChanged> pour le <xref:System.Windows.Forms.CurrencyManager> de la liaison enfant.  
  
2.  Réinitialisez la position de la table enfant à la position mise en cache enregistrée lors de la procédure précédente.  
  
     [!code-csharp[System.Windows.Forms.CurrencyManagerReset#3](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.CurrencyManagerReset#3](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/VB/Form1.vb#3)]  
  
## Exemple  
 L'exemple suivant montre comment enregistrer la position actuelle sur le <xref:System.Windows.Forms.CurrencyManager> pour une table enfant et réinitialiser la position après qu'une modification a été effectuée sur la table parente.  Cet exemple contient deux contrôles <xref:System.Windows.Forms.DataGridView> liés à deux tables dans un <xref:System.Data.DataSet> à l'aide d'un composant <xref:System.Windows.Forms.BindingSource>.  Une relation est établie entre les deux tables et la relation est ajoutée au <xref:System.Data.DataSet>.  La position dans la table enfant est initialement définie sur la troisième ligne à des fins de démonstration.  
  
 [!code-csharp[System.Windows.Forms.CurrencyManagerReset#1](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/CS/Form1.cs#1)]
 [!code-vb[System.Windows.Forms.CurrencyManagerReset#1](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.CurrencyManagerReset/VB/Form1.vb#1)]  
  
 Pour tester l'exemple de code, procédez comme suit :  
  
1.  Exécutez l'exemple.  
  
2.  Assurez\-vous que la case **Cache et réinitialisation de position** est cochée.  
  
3.  Cliquez sur le bouton **Effacer le champ parent** pour provoquer une modification dans un champ de la table parente.  Notez que la ligne sélectionnée dans la table enfant ne change pas.  
  
4.  Fermez puis réexécutez l'exemple.  Cette opération est nécessaire car le comportement de réinitialisation se produit uniquement lors de la première modification sur la ligne parente.  
  
5.  Décochez la case **Cache et réinitialisation de position**.  
  
6.  Cliquez sur le bouton **Effacer le champ parent**.  Notez que la ligne sélectionnée dans la table enfant passe en première ligne.  
  
## Compilation du code  
 Cet exemple nécessite :  
  
-   Références aux assemblys System, System.Data, System.Drawing, System.Windows.Forms et System.Xml.  
  
 Pour plus d'informations sur la création de cet exemple à partir de la ligne de commande pour [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] ou [!INCLUDE[csprcs](../../../includes/csprcs-md.md)], consultez [Génération à partir de la ligne de commande](../Topic/Building%20from%20the%20Command%20Line%20\(Visual%20Basic\).md) ou [Génération à partir de la ligne de commande avec csc.exe](../../../ocs/csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md).  Vous pouvez également générer cet exemple dans [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] en collant le code dans un nouveau projet.  Consultez également [Comment : compiler et exécuter un exemple complet de code Windows Forms à l'aide de Visual Studio](http://msdn.microsoft.com/library/Bb129228%20\(v=vs.110\)).  
  
## Voir aussi  
 [Comment : s'assurer que plusieurs contrôles liés à la même source de données restent synchronisés](../../../docs/framework/winforms/multiple-controls-bound-to-data-source-synchronized.md)   
 [Composant BindingSource](../../../docs/framework/winforms/controls/bindingsource-component.md)   
 [Liaison de données et Windows Forms](../../../docs/framework/winforms/data-binding-and-windows-forms.md)