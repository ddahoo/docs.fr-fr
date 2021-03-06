---
title: "Dans CustomPeerResolverService&#160;: Inscriptions des clients | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 40236953-a916-4236-84a6-928859e1331a
caps.latest.revision: 7
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 7
---
# Dans CustomPeerResolverService&#160;: Inscriptions des clients
Chaque nœud de la maille publie ses informations sur le point de terminaison sur le service de résolution par le biais de la fonction `Register`.  Le service de résolution stocke ces informations comme un enregistrement d'inscription.  Cet enregistrement contient un identificateur unique \(RegistrationID\) et les informations sur le point de terminaison \(PeerNodeAddress\) concernant le nœud.  
  
## Enregistrements périmés et heure d'expiration  
 Dans l'idéal, lorsqu'un nœud quitte la maille, il appelle la fonction `Unregister`, qui entraîne la suppression de l'entrée de l'inscription par le service de résolution.  Quelquefois, les nœuds sont arrêtés ou deviennent inaccessibles avant l'appel de `Unregister`, laissant derrière un enregistrement d'inscription périmé.  
  
 Les enregistrements périmés dans votre service de résolution peuvent entraîner l'échec des connexions.  Si un nœud qui essaie de se connecter à une maille reçoit des informations de connexion périmées du service de résolution, l'établissement de la liaison avec la maille peut prendre plus de temps.  Les enregistrements périmés occupent également de l'espace en mémoire.  Sans un processus de nettoyage efficace, le cache dans lequel sont stockées les inscriptions pourrait finir par être saturé, entraînant alors l'échec du service de résolution.  
  
 Le <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> marque chaque enregistrement avec une heure d'expiration \(DateTime\) et stocke cette information dans le cadre de l'enregistrement.  Le service utilise cette heure d'expiration pour identifier les enregistrements périmés.  Les implémentations personnalisées devraient se comporter de manière semblable.  
  
## RefreshInterval et CleanupInterval  
 La propriété `RefreshInterval` du <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> définit le délai de validité des enregistrements d'inscription longs dans la table de recherche d'inscription du service.  Lorsque le délai défini par cette propriété pour un enregistrement donné a expiré, l'enregistrement en question est périmé et est marqué en vue de sa suppression.  
  
 La propriété `CleanupInterval` du <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> indique au service l'intervalle de recherche et de suppression des enregistrements d'inscription périmés.  La valeur de `CleanupInterval` doit être supérieure ou égale à la valeur `RefreshInterval` définie sur le service.  
  
 Pour implémenter votre propre service de résolution, vous devez écrire une fonction de maintenance qui supprime les enregistrements d'inscription périmés.  Pour ce faire, plusieurs méthodes sont possibles :  
  
-   **Maintenance périodique** : définissez le déclenchement périodique d'une minuterie et parcourez votre magasin de données afin de supprimer les enregistrements obsolètes.  Le <xref:System.ServiceModel.PeerResolvers.CustomPeerResolverService> utilise cette approche.  
  
-   **Suppression passive** : au lieu de rechercher activement les enregistrements périmés à intervalles normaux, vous pouvez identifier et supprimer ces enregistrements pendant que votre service exécute déjà une autre fonction.  Cela peut ralentir potentiellement le temps de réponse aux demandes des clients de programme de résolution, mais il élimine la nécessité de définir une minuterie, et peut être plus efficace si peu de nœuds sont supposés quitter la maille sans appeler `Unregister`.  
  
## RegistrationLifetime et Refresh  
 Lorsqu'un nœud s'inscrit auprès d'un service de résolution, il reçoit de ce dernier un objet <xref:System.ServiceModel.PeerResolvers.RegisterResponseInfo>.  Cet objet a une propriété `RegistrationLifetime` qui indique au nœud combien de temps il reste avant que l'inscription expire et sa suppression par le service de résolution.  Par exemple, si `RegistrationLifetime` est égal à 2 minutes, le nœud doit appeler `Refresh` dans moins de 2 minutes afin de garantir que l'enregistrement reste actualisé et ne soit pas supprimé.  Lorsque le service de résolution reçoit une demande `Refresh`, il examine l'enregistrement et réinitialise l'heure d'expiration.  Refresh retourne un objet <xref:System.ServiceModel.PeerResolvers.RefreshResponseInfo> avec une propriété `RegistrationLifetime`.  
  
## Voir aussi  
 [Programmes de résolution d'homologue](../../../../docs/framework/wcf/feature-details/peer-resolvers.md)