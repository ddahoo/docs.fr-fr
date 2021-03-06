---
title: "&lt;sendMessageChannelCache&gt; | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "reference"
ms.assetid: 241e428e-5030-4b13-8a0a-69f05288d3d9
caps.latest.revision: 3
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 3
---
# &lt;sendMessageChannelCache&gt;
Comportement de service qui permet de personnaliser les niveaux de partage du cache, les paramètres du cache de la fabrique de canaux et les paramètres du cache de canaux, pour les flux de travail qui envoient des messages aux points de terminaison de service par le biais d'activités de messagerie Send.  
  
## Syntaxe  
  
```  
  
<behaviors>  
  <serviceBehaviors>  
    <behavior name=String">  
       <sendMessageChannelCache allowUnsafeCaching="Boolean" >          
           <channelSettings idleTimeout="TimeSpan" leaseTimeout="TimeSpan" maxItemsInCache="Integer" />  
           <factorySettings idleTimeout="TimeSpan" leaseTimeout="TimeSpan" maxItemsInCache="Integer" />  
       </sendMessageChannelCache>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
  
```  
  
## Attributs et éléments  
 Les sections suivantes décrivent des attributs, des éléments enfants et des éléments parents.  
  
### Attributs  
  
|Attribut|Description|  
|--------------|-----------------|  
|allowUnsafeCaching|Valeur booléenne qui indique s'il faut activer la mise en cache.  Si le service de flux de travail comporte des liaisons ou des comportements personnalisés, la mise en cache pourrait être non sécurisée et donc désactivée par défaut.  Cependant, si vous voulez activer la mise en cache, affectez à cette propriété la valeur **true**.|  
  
### Éléments enfants  
  
|Élément|Description|  
|-------------|-----------------|  
|[\<channelSettings\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/channelsettings.md)|Spécifie les paramètres du cache de canaux.|  
|[\<factorySettings\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/factorysettings.md)|Spécifie les paramètres du cache de la fabrique de canaux.|  
  
### Éléments parents  
  
|Élément|Description|  
|-------------|-----------------|  
|[\<behavior\> de \<serviceBehaviors\>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/behavior-of-servicebehaviors-of-workflow.md)|Spécifie un élément de comportement.|  
  
## Notes  
 Ce comportement de service est destiné aux flux de travail qui envoient des messages aux points de terminaison de service.  Ces flux de travail sont généralement des flux de travail clients, mais peuvent également être des services de flux de travail hébergés dans un <xref:System.ServiceModel.WorkflowServiceHost>.  
  
 Par défaut, dans un flux de travail hébergé par un <xref:System.ServiceModel.WorkflowServiceHost>, le cache utilisé par les activités de messagerie <xref:System.ServiceModel.Activities.Send> est partagé entre toutes les instances de flux de travail dans <xref:System.ServiceModel.WorkflowServiceHost> \(mise en cache au niveau de l'hôte\).  Pour un flux de travail client non hébergé par un objet  <xref:System.ServiceModel.WorkflowServiceHost>, le cache est uniquement disponible pour l'instance de flux de travail \(mise en cache au niveau de l'instance\).  La mise en cache est désactivée par défaut pour toute activité d'envoi dans votre flux de travail qui a des points de terminaison définis dans la configuration.  
  
 [!INCLUDE[crabout](../../../../../includes/crabout-md.md)] la modification des niveaux de partage de cache par défaut et des paramètres de cache pour la fabrique de canaux et le cache de canaux, consultez [Modification des niveaux de partage du cache pour les activités d'envoi](../../../../../docs/framework/wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md).  
  
## Exemple  
 Dans un service de flux de travail hébergé, vous pouvez spécifier les paramètres du cache de la fabrique et du cache de canaux dans le fichier de configuration de l'application.  Pour cela, ajoutez un comportement de service qui contient les paramètres des caches de la fabrique et de canaux et ajoutez\-le à votre service.  L'exemple suivant affiche le contenu d'un fichier de configuration qui contient le comportement de service **MyChannelCacheBehavior**  avec les paramètres des caches de la fabrique et de canaux personnalisés.  Ce comportement de service est ajouté au service via l'attribut **behaviorConfiguarion** .  
  
```  
  
<configuration>    
  <system.serviceModel>  
    <!-- List of other config sections here -->   
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="MyChannelCacheBehavior">  
          <sendMessageChannelCache allowUnsafeCaching ="false" >  
            <!-- Control only the host level settings -->   
            <factorySettings maxItemsInCache = "8" idleTimeout = "00:05:00" leaseTimeout="10:00:00" />  
            <channelSettings maxItemsInCache = "32" idleTimeout = "00:05:00" leaseTimeout="00:06:00" />  
          </sendMessageChannelCache>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    <services>  
      <service name="MyService" behaviorConfiguration="MyChannelCacheBehavior" />  
    </services>  
  </system.serviceModel>  
</configuration>  
  
```  
  
## Voir aussi  
 <xref:System.ServiceModel.Activities.SendMessageChannelCache>   
 <xref:System.ServiceModel.Activities.Configuration.SendMessageChannelCacheElement>   
 <xref:System.ServiceModel.Activities.Send>   
 [Modification des niveaux de partage du cache pour les activités d'envoi](../../../../../docs/framework/wcf/feature-details/changing-the-cache-sharing-levels-for-send-activities.md)