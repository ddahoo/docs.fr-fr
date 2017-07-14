---
title: "Liaison des donn&#233;es aux contr&#244;les (WCF Data Services) | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-oob"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "applications clientes, Services de données WCF"
  - "liaison de données, Services de données WCF"
  - "Services de données WCF, bibliothèque cliente"
ms.assetid: b32e1d49-c214-4cb1-867e-88fbb3d08c8d
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# Liaison des donn&#233;es aux contr&#244;les (WCF Data Services)
Avec [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)], vous pouvez lier des contrôles tels que les contrôles `ComboBox` et `ListView` à une instance de la classe <xref:System.Data.Services.Client.DataServiceCollection%601>. Cette collection, qui hérite de la classe <xref:System.Collections.ObjectModel.ObservableCollection%601>, contient les données d'un flux [!INCLUDE[ssODataFull](../../../../includes/ssodatafull-md.md)].  Cette classe représente une collecte de données dynamique qui fournit des notifications lorsque des éléments sont ajoutés ou supprimés.  Lorsque vous utilisez une instance de <xref:System.Data.Services.Client.DataServiceCollection%601> pour la liaison de données, les bibliothèques clientes [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] gèrent ces événements pour vérifier que les objets suivis par <xref:System.Data.Services.Client.DataServiceContext> restent synchronisés avec les données dans l'élément d'interface utilisateur relié.  
  
 La classe <xref:System.Data.Services.Client.DataServiceCollection%601> implémente \(indirectement\) l'interface <xref:System.Collections.Specialized.INotifyCollectionChanged> pour alerter le contexte lorsque des objets sont ajoutés ou supprimés de la collection.  Les objets de type de service de données utilisés avec <xref:System.Data.Services.Client.DataServiceCollection%601> doivent également implémenter l'interface <xref:System.ComponentModel.INotifyPropertyChanged> pour alerter <xref:System.Data.Services.Client.DataServiceCollection%601> lorsque les propriétés d'objets dans la collection de liaisons ont changé.  
  
> [!NOTE]
>  Lorsque vous utilisez la boîte de dialogue **Ajouter une référence de service** ou l'outil [DataSvcUtil.exe](../../../../docs/framework/data/wcf/wcf-data-service-client-utility-datasvcutil-exe.md) avec l'option `/dataservicecollection` pour générer les classes de service de données client, les classes de données générées implémentent l'interface <xref:System.ComponentModel.INotifyPropertyChanged>.  Pour plus d'informations, consultez [Procédure : générer manuellement des classes de service de données client](../../../../docs/framework/data/wcf/how-to-manually-generate-client-data-service-classes-wcf-data-services.md).  
  
## Création de la collection de liaisons  
 Créez une nouvelle instance de la classe <xref:System.Data.Services.Client.DataServiceCollection%601> en appelant l'une des méthodes de constructeur de classe avec une instance <xref:System.Data.Services.Client.DataServiceContext> fournie et éventuellement une requête <xref:System.Data.Services.Client.DataServiceQuery%601> ou LINQ qui, lorsqu'elle est exécutée, retourne une instance <xref:System.Collections.Generic.IEnumerable%601>.  Cette instance <xref:System.Collections.Generic.IEnumerable%601> fournit la source d'objets pour la collection de liaisons, matérialisés à partir d'un flux [!INCLUDE[ssODataShort](../../../../includes/ssodatashort-md.md)].  Pour plus d'informations, consultez [Matérialisation d'objets](../../../../docs/framework/data/wcf/object-materialization-wcf-data-services.md).  Par défaut, les modifications apportées aux objets liés et aux éléments insérés dans la collection sont suivies automatiquement par <xref:System.Data.Services.Client.DataServiceContext>.  Si vous devez suivre ces modifications manuellement, appelez l'une des méthodes de constructeur qui prend un paramètre `trackingMode` et spécifiez une valeur <xref:System.Data.Services.Client.TrackingMode>.  
  
 L'exemple suivant montre comment créer une instance de <xref:System.Data.Services.Client.DataServiceCollection%601> à partir d'un <xref:System.Data.Services.Client.DataServiceContext> fourni et d'une <xref:System.Data.Services.Client.DataServiceQuery%601> qui retourne tous les clients avec les ordres associés :  
  
 [!code-csharp[Astoria Northwind Client#CustomersOrders2Binding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorders2.cs#customersorders2binding)]
 [!code-vb[Astoria Northwind Client#CustomersOrders2Binding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorders2.vb#customersorders2binding)]  
  
## Liaison de données aux éléments Windows Presentation Foundation  
 Étant donné que la classe <xref:System.Data.Services.Client.DataServiceCollection%601> hérite de la classe <xref:System.Collections.ObjectModel.ObservableCollection%601>, vous pouvez lier des objets à un élément ou un contrôle dans une application WPF \(Windows Presentation Foundation\) comme vous le feriez lorsque vous utilisez la classe <xref:System.Collections.ObjectModel.ObservableCollection%601> pour la liaison.  Pour plus d'informations, consultez [Liaison de données \(Windows Presentation Foundation\)](../../../../docs/framework/wpf/data/data-binding-wpf.md).  Une façon de lier des données du service des données aux contrôles WPF est de définir la propriété `DataContext` de l'élément sur l'instance de la classe <xref:System.Data.Services.Client.DataServiceCollection%601> qui contient le résultat de la requête.  Dans ce cas, utilisez la propriété <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A> pour définir la source de l'objet pour le contrôle.  Utilisez la propriété <xref:System.Windows.Controls.ItemsControl.DisplayMemberPath%2A> pour spécifier quelle propriété de l'objet lié afficher.  Si vous liez un élément à un objet connexe retourné par une propriété de navigation, incluez le chemin d'accès dans la liaison définie pour la propriété <xref:System.Windows.Controls.ItemsControl.ItemsSource%2A>.  Ce chemin d'accès est relatif à l'objet racine défini par la propriété <xref:System.Windows.FrameworkElement.DataContext%2A> du contrôle parent.  L'exemple suivant définit la propriété <xref:System.Windows.FrameworkElement.DataContext%2A> d'un élément <xref:System.Windows.Controls.StackPanel> pour qu'elle lie le contrôle parent à une <xref:System.Data.Services.Client.DataServiceCollection%601> d'objets de client :  
  
 [!code-csharp[Astoria Northwind Client#MasterDetailBinding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderscustom.xaml.cs#masterdetailbinding)]
 [!code-csharp[Astoria Northwind Client#MasterDetailBinding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderswpf.xaml.cs#masterdetailbinding)]
 [!code-vb[Astoria Northwind Client#MasterDetailBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderscustom.xaml.vb#masterdetailbinding)]
 [!code-vb[Astoria Northwind Client#MasterDetailBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf.xaml.vb#masterdetailbinding)]
 [!code-vb[Astoria Northwind Client#MasterDetailBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderscustom2.xaml.vb#masterdetailbinding)]  
  
 L'exemple suivant montre la définition XAML de la liaison des contrôles enfants <xref:System.Windows.Controls.DataGrid> et <xref:System.Windows.Controls.ComboBox> :  
  
 [!code-xml[Astoria Northwind Client#MasterDetailXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf.xaml#masterdetailxaml)]  
  
 Pour plus d'informations, consultez [Procédure : lier des données aux éléments Windows Presentation Foundation](../../../../docs/framework/data/wcf/bind-data-to-wpf-elements-wcf-data-services.md).  
  
 Lorsqu'une entité participe à une relation un\-à\-plusieurs ou plusieurs\-à\-plusieurs, la propriété de navigation de la relation retourne une collection d'objets connexes.  Lorsque vous utilisez la boîte de dialogue **Ajouter une référence de service** ou l'outil DataSvcUtil.exe pour générer les classes de service de données client, la propriété de navigation retourne une instance de <xref:System.Data.Services.Client.DataServiceCollection%601>.  Cette opération vous permet de lier des objets connexes à un contrôle, et de prendre en charge des scénarios WPF courants, tels que le modèle de liaison maître\/détail pour les entités connexes.  Dans l'exemple de code XAML précédent, le code XAML lie le <xref:System.Data.Services.Client.DataServiceCollection%601> principal à l'élément de données racine.  Puis l'ordre <xref:System.Windows.Controls.DataGrid> est lié aux Orders que <xref:System.Data.Services.Client.DataServiceCollection%601> a retourné de l'objet Customers sélectionné qui à son tour est lié à l'élément des données racine de <xref:System.Windows.Window>.  
  
## Liaison de données aux contrôles Windows Forms  
 Pour lier des objets à un contrôle Windows Form, définissez la propriété `DataSource` du contrôle sur l'instance de la classe <xref:System.Data.Services.Client.DataServiceCollection%601> qui contient le résultat de la requête.  
  
> [!NOTE]
>  La liaison de données n'est prise en charge que pour le contrôle à l'écoute des événements de modification en implémentant les interfaces <xref:System.Collections.Specialized.INotifyCollectionChanged> et <xref:System.ComponentModel.INotifyPropertyChanged>.  Lorsqu'un contrôle ne prend pas en charge ce type de notification des modifications, les modifications apportées à l'objet <xref:System.Data.Services.Client.DataServiceCollection%601> sous\-jacent ne sont pas répercutées sur le contrôle dépendant.  
  
 L'exemple suivant lie un objet <xref:System.Data.Services.Client.DataServiceCollection%601> à un contrôle <xref:System.Windows.Forms.ComboBox> :  
  
 [!code-csharp[Astoria Northwind Client#CustomersOrdersDataBindingSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorders.cs#customersordersdatabindingspecific)]
 [!code-vb[Astoria Northwind Client#CustomersOrdersDataBindingSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorders.vb#customersordersdatabindingspecific)]  
  
 Lorsque vous utilisez la boîte de dialogue **Ajouter une référence de service** pour générer les classes de service de données client, une source de données projet est également créée qui est basée sur le <xref:System.Data.Services.Client.DataServiceContext> généré.  Avec cette source de données, vous pouvez créer des éléments d'interface utilisateur ou des contrôles qui affichent des données issues du service de données en faisant simplement glisser des éléments de la fenêtre **Sources de données** sur le concepteur.  Ces éléments deviennent des éléments dans l'interface utilisateur de l'application qui sont liés à la source de données.  Pour plus d'informations, consultez [Procédure : lier des données à l'aide d'une source de données projet](../../../../docs/framework/data/wcf/how-to-bind-data-using-a-project-data-source-wcf-data-services.md).  
  
## Liaison de données paginées  
 Un service de données peut être configuré pour limiter la quantité de données interrogées qui sont retournées dans un message de réponse unique.  Pour plus d'informations, consultez [Configuration du service de données](../../../../docs/framework/data/wcf/configuring-the-data-service-wcf-data-services.md).  Lorsque le service de données pagine des données de réponse, chaque réponse contient un lien utilisé pour retourner la page suivante de résultats.  Pour plus d'informations, consultez [Chargement de contenu différé](../../../../docs/framework/data/wcf/loading-deferred-content-wcf-data-services.md).  Dans ce cas, vous devez charger explicitement des pages en appelant la méthode <xref:System.Data.Services.Client.DataServiceCollection%601.Load%2A> sur <xref:System.Data.Services.Client.DataServiceCollection%601> en passant l'URI obtenu de la propriété <xref:System.Data.Services.Client.DataServiceQueryContinuation.NextLinkUri%2A>, comme dans l'exemple suivant :  
  
 [!code-csharp[Astoria Northwind Client#BindPagedDataSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderswpf3.xaml.cs#bindpageddataspecific)]
 [!code-vb[Astoria Northwind Client#BindPagedDataSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderswpf3.xaml.vb#bindpageddataspecific)]  
  
 Les objets connexes sont chargés de façon semblable.  Pour plus d'informations, consultez [Procédure : lier des données aux éléments Windows Presentation Foundation](../../../../docs/framework/data/wcf/bind-data-to-wpf-elements-wcf-data-services.md).  
  
## Personnalisation des comportements de liaison de données  
 La classe <xref:System.Data.Services.Client.DataServiceCollection%601> vous permet d'intercepter les événements déclenchés lorsque des modifications sont apportées à la collection, telles que l'ajout ou la suppression d'un objet, et lorsque les modifications sont apportées aux propriétés d'objet dans une collection.  Vous pouvez modifier les événements de liaison de données pour remplacer le comportement par défaut qui inclut les contraintes suivantes :  
  
-   Aucune validation n'est effectuée dans les délégués.  
  
-   L'ajout d'une entité ajoute automatiquement des entités connexes.  
  
-   La suppression d'une entité ne supprime pas les entités connexes.  
  
 Lorsque vous créez une nouvelle instance de <xref:System.Data.Services.Client.DataServiceCollection%601>, vous avez l'option de spécifier les paramètres suivants qui définissent des délégués aux méthodes qui gèrent les événements déclenchés lorsque les objets liés sont modifiés :  
  
-   `entityChanged` \- méthode appelée lors de la modification de la propriété d'un objet lié.  Ce délégué <xref:System.Func%602> accepte un objet <xref:System.Data.Services.Client.EntityChangedParams> et retourne une valeur booléenne qui indique si le comportement par défaut, appeler <xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A> sur le <xref:System.Data.Services.Client.DataServiceContext>, doit encore se produire.  
  
-   `entityCollectionChanged`\- méthode appelée lorsqu'un objet est ajouté ou supprimé de la collection de liaisons.  Ce délégué <xref:System.Func%602> accepte un objet <xref:System.Data.Services.Client.EntityCollectionChangedParams> et retourne une valeur booléenne qui indique si le comportement par défaut, appeler <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A> pour une action <xref:System.Collections.Specialized.NotifyCollectionChangedAction> ou <xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> pour une action <xref:System.Collections.Specialized.NotifyCollectionChangedAction> sur <xref:System.Data.Services.Client.DataServiceContext>, doit encore se produire.  
  
> [!NOTE]
>  [!INCLUDE[ssAstoria](../../../../includes/ssastoria-md.md)] n'effectue aucune validation des comportements personnalisés que vous implémentez dans ces délégués.  
  
 Dans l'exemple suivant, l'action <xref:System.Collections.Specialized.NotifyCollectionChangedAction> est personnalisée pour appeler la méthode <xref:System.Data.Services.Client.DataServiceContext.DeleteLink%2A> et <xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> pour supprimer des entités `Orders_Details` qui appartiennent à une entité `Orders` supprimée.  Cette action personnalisée est effectuée parce que les entités dépendantes ne sont pas supprimées automatiquement lorsque l'entité parente est supprimée.  
  
 [!code-csharp[Astoria Northwind Client#CustomersOrdersDeleteRelated](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria northwind client/cs/customerorderscustom.xaml.cs#customersordersdeleterelated)]
 [!code-vb[Astoria Northwind Client#CustomersOrdersDeleteRelated](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderscustom.xaml.vb#customersordersdeleterelated)]
 [!code-vb[Astoria Northwind Client#CustomersOrdersDeleteRelated](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria northwind client/vb/customerorderscustom2.xaml.vb#customersordersdeleterelated)]  
  
 Pour plus d'informations, consultez [Procédure : personnaliser des comportements de liaison de données](../../../../docs/framework/data/wcf/how-to-customize-data-binding-behaviors-wcf-data-services.md).  
  
 Le comportement par défaut lorsqu'un objet est supprimé d'une <xref:System.Data.Services.Client.DataServiceCollection%601> à l'aide de la méthode <xref:System.Collections.ObjectModel.Collection%601.Remove%2A> est que l'objet est également marqué comme supprimé dans <xref:System.Data.Services.Client.DataServiceContext>.  Pour modifier ce comportement, vous pouvez spécifier un délégué à une méthode dans le paramètre `entityCollectionChanged` qui est appelé lorsque l'événement <xref:System.Collections.Specialized.INotifyCollectionChanged.CollectionChanged> se produit.  
  
## Liaison de données avec les classes de données de client personnalisées  
 Pour pouvoir charger des objets dans <xref:System.Data.Services.Client.DataServiceCollection%601>, les objets eux\-mêmes doivent implémenter l'interface <xref:System.ComponentModel.INotifyPropertyChanged>.  Les classes clientes du service de données générées lorsque vous utilisez la boîte de dialogue **Ajouter une référence de service** ou l'outil [DataSvcUtil.exe](../../../../docs/framework/data/wcf/wcf-data-service-client-utility-datasvcutil-exe.md) implémentent cette interface.  Si vous fournissez vos propres classes de données clientes, vous devez utiliser un autre type de collection pour la liaison de données.  Lorsque les objets changent, vous devez gérer des événements dans les contrôles liés aux données pour appeler les méthodes suivantes de la classe <xref:System.Data.Services.Client.DataServiceContext> :  
  
-   <xref:System.Data.Services.Client.DataServiceContext.AddObject%2A>\- lorsqu'un nouvel objet est ajouté à la collection.  
  
-   <xref:System.Data.Services.Client.DataServiceContext.DeleteObject%2A> \- lorsqu'un objet est supprimé de la collection.  
  
-   <xref:System.Data.Services.Client.DataServiceContext.UpdateObject%2A>\- lorsqu'une propriété est modifiée sur un objet dans la collection.  
  
-   <xref:System.Data.Services.Client.DataServiceContext.AddLink%2A>\- lorsqu'un objet est ajouté à une collection d'objets connexes.  
  
-   <xref:System.Data.Services.Client.DataServiceContext.SetLink%2A>\- lorsqu'un objet est ajouté à une collection d'objets connexes.  
  
 Pour plus d'informations, consultez [Mise à jour du service de données](../../../../docs/framework/data/wcf/updating-the-data-service-wcf-data-services.md).  
  
## Voir aussi  
 [Procédure : générer manuellement des classes de service de données client](../../../../docs/framework/data/wcf/how-to-manually-generate-client-data-service-classes-wcf-data-services.md)   
 [Procédure : ajouter une référence de service de données](../../../../docs/framework/data/wcf/how-to-add-a-data-service-reference-wcf-data-services.md)