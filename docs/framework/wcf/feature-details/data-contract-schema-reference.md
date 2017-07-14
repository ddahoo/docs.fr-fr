---
title: "R&#233;f&#233;rence des sch&#233;mas de contrats de donn&#233;es | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "contrats de données [WCF], référence des schémas"
ms.assetid: 9ebb0ebe-8166-4c93-980a-7c8f1f38f7c0
caps.latest.revision: 24
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 24
---
# R&#233;f&#233;rence des sch&#233;mas de contrats de donn&#233;es
Cette rubrique décrit le sous\-ensemble du schéma XML \(XSD\) utilisé par <xref:System.Runtime.Serialization.DataContractSerializer> pour décrire les types CLR \(Common Language Run\-time\) pour la sérialisation XML.  
  
## Mappages DataContractSerializer  
 `DataContractSerializer` mappe des types CLR à XSD lorsque les métadonnées sont exportées d’un service [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] à l’aide d’un point de terminaison de métadonnées ou à l’aide de l’[Outil Service Model Metadata Tool \(Svcutil.exe\)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md).[!INCLUDE[crdefault](../../../../includes/crdefault-md.md)] [Sérialiseur de contrat de données](../../../../docs/framework/wcf/feature-details/data-contract-serializer.md).  
  
 `DataContractSerializer` mappe également XSD aux types CLR lorsque Svcutil.exe est utilisé pour accéder à Web Services Description Language \(WSDL\) ou aux documents XSD et génère des contrats de données pour les services ou les clients.  
  
 Uniquement les instances de schéma XML conformes aux spécifications déclarées dans ce document peuvent être mappées aux types CLR à l'aide de `DataContractSerializer`.  
  
### Niveaux pris en charge  
 `DataContractSerializer` fournit les niveaux de prise en charge suivants pour une fonctionnalité de schéma XML donnée :  
  
-   **Pris en charge**. Il y a mappage explicite de cette fonctionnalité aux types ou aux attributs CLR \(ou les deux à la fois\) via `DataContractSerializer`.  
  
-   **Ignoré**. La fonctionnalité est autorisée dans les schémas importés par `DataContractSerializer`, mais n'a aucun effet sur la génération du code.  
  
-   **Interdit**.`DataContractSerializer` ne prend pas en charge l'importation d'un schéma à l'aide de la fonctionnalité. Par exemple, Svcutil.exe, lors de l'accès à un WSDL avec un schéma qui utilise une telle fonctionnalité, revient à la place à l'utilisation de <xref:System.Xml.Serialization.XmlSerializer>. La valeur est par défaut.  
  
## Informations générales  
  
-   L’espace de noms du schéma est décrit sur la page [XML Schema](http://go.microsoft.com/fwlink/?LinkId=95475) \(Schéma XML\). Le préfixe "xs" est utilisé dans ce document.  
  
-   Les attributs avec un espace de noms qui n'est pas un schéma sont ignorés.  
  
-   Les annotations \(sauf celles décrites dans ce document\) sont ignorées.  
  
### \<xs:schema\> : attributs  
  
|Attribut|DataContract|  
|--------------|------------------|  
|`attributeFormDefault`|Ignoré.|  
|`blockDefault`|Ignoré.|  
|`elementFormDefault`|Doit être qualifié. Tous les éléments doivent être qualifiés pour qu'un schéma soit pris en charge par `DataContractSerializer`. Cela peut être accompli soit en définissant le paramètre xs:schema\/@elementFormDefault sur "qualified", soit en définissant xs:element\/@form sur "qualified" sur chaque déclaration d'élément individuelle.|  
|`finalDefault`|Ignoré.|  
|`Id`|Ignoré.|  
|`targetNamespace`|Pris en charge et mappé à l'espace de noms du contrat de données. Si cet attribut n'est pas spécifié, l'espace de noms vierge est utilisé. Ne peut pas être l'espace de noms  réservé.|  
|`version`|Ignoré.|  
  
### \<xs:schema\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`include`|Prise en charge.`DataContractSerializer` prend en charge xs:include et xs:import. Toutefois, Svcutil.exe restreint le suivi des références `xs:include/@schemaLocation` et `xs:import/@location` lorsque les métadonnées sont chargées à partir d'un fichier local. La liste des fichiers de schéma doit passer par un mécanisme hors bande et non par `include` dans ce cas ; les documents de schéma `include` sont ignorés.|  
|`redefine`|Interdit. L'utilisation de `xs:redefine` est interdite par `DataContractSerializer` pour des raisons de sécurité : `x:redefine` requiert que `schemaLocation` soit suivi. Dans certaines circonstances, Svcutil.exe restreint l'utilisation de `schemaLocation` à l'aide de DataContract.|  
|`import`|Prise en charge.`DataContractSerializer` prend en charge `xs:include` et `xs:import`. Toutefois, Svcutil.exe restreint le suivi des références `xs:include/@schemaLocation` et `xs:import/@location` lorsque les métadonnées sont chargées à partir d'un fichier local. La liste des fichiers de schéma doit passer par un mécanisme hors bande et non par `include` dans ce cas ; les documents de schéma `include` sont ignorés.|  
|`simpleType`|Prise en charge. Consultez la section `xs:simpleType`.|  
|`complexType`|Pris en charge, mappe aux contrats de données. Consultez la section `xs:complexType`.|  
|`group`|Ignoré.`DataContractSerializer` ne prend pas en charge l'utilisation de `xs:group`, `xs:attributeGroup` et `xs:attribute`. Ces déclarations sont ignorées en tant qu'enfants de `xs:schema`, mais ne peuvent pas être référencées à partir de `complexType` ou d'autres constructions prises en charge.|  
|`attributeGroup`|Ignoré.`DataContractSerializer` ne prend pas en charge l'utilisation de `xs:group`, `xs:attributeGroup` et `xs:attribute`. Ces déclarations sont ignorées en tant qu'enfants de `xs:schema`, mais ne peuvent pas être référencées à partir de `complexType` ou d'autres constructions prises en charge.|  
|`element`|Prise en charge. Consultez la déclaration d'élément globale \(GED\).|  
|`attribute`|Ignoré.`DataContractSerializer` ne prend pas en charge l'utilisation de `xs:group`, `xs:attributeGroup` et `xs:attribute`. Ces déclarations sont ignorées en tant qu'enfants de `xs:schema`, mais ne peuvent pas être référencées à partir de `complexType` ou d'autres constructions prises en charge.|  
|`notation`|Ignoré.|  
  
## Types complexes – \<xs:complexType\>  
  
### Informations générales  
 Chaque type complexe \<xs:complexType\> mappe à un contrat de données.  
  
### \<xs:complexType\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`abstract`|Doit être faux \(valeur par défaut\).|  
|`block`|Interdit.|  
|`final`|Ignoré.|  
|`id`|Ignoré.|  
|`mixed`|Doit être faux \(valeur par défaut\).|  
|`name`|Pris en charge et mappé au nom du contrat de données. Si le nom inclut des points, une tentative est faite pour mapper le type à un type interne. Par exemple, un type complexe nommé `A.B` mappe à un type de contrat de données qui est un type interne d'un type portant le nom du contrat de données `A`, mais uniquement si un tel type de contrat de données existe. Plusieurs niveaux d'imbrication sont possibles : par exemple, `A.B.C` peut être un type interne, mais uniquement si à la fois `A` et `A.B` existent.|  
  
### \<xs:complexType\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`simpleContent`|Les extensions sont interdites.<br /><br /> La restriction est autorisée uniquement depuis `anySimpleType`.|  
|`complexContent`|Prise en charge. Consultez « Héritage ».|  
|`group`|Interdit.|  
|`all`|Interdit.|  
|`choice`|Interdit|  
|`sequence`|Pris en charge, mappe aux membres de données d'un contrat de données.|  
|`attribute`|Interdit, même si l'utilisation \= "prohibited" \(avec une exception\). Uniquement les attributs facultatifs de l'espace de noms du schéma de sérialisation standard sont pris en charge. Ils ne mappent pas aux membres de données dans le modèle de programmation du contrat de données. Actuellement, un seul de ces attributs est significatif et est discuté dans la section ISerializable. Tous les autres sont ignorés.|  
|`attributeGroup`|Interdit. Dans la version 1 finale de [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)], `DataContractSerializer` ignore la présence de `attributeGroup` à l'intérieur de `xs:complexType`.|  
|`anyAttribute`|Interdit.|  
|\(vide\)|Mappe à un contrat de données sans membres de données.|  
  
### \<xs:sequence\> dans un type complexe : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`id`|Ignoré.|  
|`maxOccurs`|Doit être 1 \(valeur par défaut\).|  
|`minOccurs`|Doit être 1 \(valeur par défaut\).|  
  
### \<xs:sequence\> dans un type complexe : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`element`|Chaque instance mappe à un membre de données.|  
|`group`|Interdit.|  
|`choice`|Interdit.|  
|`sequence`|Interdit.|  
|`any`|Interdit.|  
|\(vide\)|Mappe à un contrat de données sans membres de données.|  
  
## Éléments – \<xs:element\>  
  
### Informations générales  
 `<xs:element>` peut se produire dans les contextes suivants :  
  
-   Il peut se produire dans un `<xs:sequence>`, qui décrit un membre de données d'un contrat de données normal \(qui n'est pas une collection\). Dans ce cas, l'attribut `maxOccurs` doit avoir la valeur 1. \(Une valeur de 0 n'est pas autorisée\).  
  
-   Il peut se produire dans un `<xs:sequence>`, qui décrit un membre de données d'un contrat de données de collection. Dans ce cas, l'attribut `maxOccurs` doit être supérieur à 1 ou « unbounded ».  
  
-   Il peut se produire dans un `<xs:schema>` en tant que déclaration d'élément globale \(GED\).  
  
### \<xs:element\> avec maxOccurs\=1 dans un \<xs:sequence\> \(membres de données\)  
  
|Attribut|Schéma|  
|--------------|------------|  
|`ref`|Interdit.|  
|`name`|Pris en charge, mappe au nom du membre de données.|  
|`type`|Pris en charge, mappe au type du membre de données. Pour plus d'informations, consultez le mappage de type\/primitif. Si non spécifié \(et l'élément ne contient pas de type anonyme\), `xs:anyType` est pris en compte.|  
|`block`|Ignoré.|  
|`default`|Interdit.|  
|`fixed`|Interdit.|  
|`form`|Doit être qualifié. Cet attribut peut également être défini via `elementFormDefault` sur `xs:schema`.|  
|`id`|Ignoré.|  
|`maxOccurs`|1|  
|`minOccurs`|Mappe à la propriété <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> d'un membre de données \(`IsRequired` a la valeur true lorsque `minOccurs` a la valeur 1\).|  
|`nillable`|Affecte le mappage de type. Consultez le mappage de type\/primitif.|  
  
### \<xs:element\> avec maxOccurs\>1 dans un \<xs:sequence\> \(collections\)  
  
-   Mappe à un <xref:System.Runtime.Serialization.CollectionDataContractAttribute>.  
  
-   Dans les types collection, un seul xs:element est autorisé dans un xs:sequence.  
  
 Les collections peuvent appartenir aux types suivants :  
  
-   Collections normales \(par exemple, tableaux\).  
  
-   Collections de dictionnaires \(mappage d'une valeur à une autre; par exemple, un <xref:System.Collections.Hashtable>\).  
  
-   La seule différence entre un dictionnaire et un tableau pour un type de paire clé\/valeur réside dans le modèle de programmation généré. Il existe un mécanisme d'annotation de schéma qui peut être utilisé pour indiquer qu'un type donné est une collection de dictionnaires.  
  
 Les règles appliquées aux attributs `ref`, `block`, `default`, `fixed`, `form` et `id` sont les mêmes que pour le type qui n'est pas une collection. Les autres attributs sont inclus dans le tableau suivant.  
  
|Attribut|Schéma|  
|--------------|------------|  
|`name`|Pris en charge, mappe à la propriété <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> dans l'attribut `CollectionDataContractAttribute`.|  
|`type`|Pris en charge, mappe au type stocké dans la collection.|  
|`maxOccurs`|Supérieur à 1 ou "unbounded". Le schéma DC doit utiliser "unbounded".|  
|`minOccurs`|Ignoré.|  
|`nillable`|Affecte le mappage de type. Cet attribut est ignoré pour les collections de dictionnaires.|  
  
### \<xs:element\> dans un \<xs:schema\> \(déclaration d'élément globale\)  
  
-   Une déclaration d'élément globale \(GED\) qui a les mêmes nom et espace de noms qu'un type dans le schéma, ou qui définit un type anonyme à l'intérieur d'elle\-même, est considérée comme associée au type.  
  
-   Exportation de schéma : les GED associées sont générées pour chaque type généré, simple et complexe.  
  
-   Désérialisation\/sérialisation : les GED associées sont utilisées comme éléments racine pour le type.  
  
-   Importation de schéma : les GED associées ne sont pas requises et sont ignorées si elles suivent les règles suivantes \(à moins qu'elles définissent des types\).  
  
|Attribut|Schéma|  
|--------------|------------|  
|`abstract`|Doit être faux pour les GED associées.|  
|`block`|Interdit dans les GED associées.|  
|`default`|Interdit dans les GED associées.|  
|`final`|Doit être faux pour les GED associées.|  
|`fixed`|Interdit dans les GED associées.|  
|`id`|Ignoré.|  
|`name`|Prise en charge. Consultez la définition des GED associées.|  
|`nillable`|Doit être vrai pour les GED associées.|  
|`substitutionGroup`|Interdit dans les GED associées.|  
|`type`|Pris en charge, doit correspondre au type associé pour les GED associées \(à moins que l'élément contienne un type anonyme\).|  
  
### \<xs:element\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`simpleType`|Pris en charge.\*|  
|`complexType`|Pris en charge.\*|  
|`unique`|Ignoré.|  
|`key`|Ignoré.|  
|`keyref`|Ignoré.|  
|\(vide\)|Prise en charge.|  
  
 \* L'utilisation des mappages `simpleType` et `complexType,` pour les types anonymes est la même que pour les types non anonymes, à l'exception qu'il n'y a pas de contrats de données anonymes, ainsi, un contrat de données nommé est créé, avec un nom généré dérivé du nom d'élément. Les règles pour les types anonymes sont les suivantes :  
  
-   Détail de l'implémentation [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)] : si le nom `xs:element` ne contient pas de points, le type anonyme mappe à un type interne du type externe de contrat de données. Si le nom contient des points, le type de contrat de données résultant est indépendant \(n'est pas un type interne\).  
  
-   Le nom de contrat de données généré du type interne est le nom de contrat de données du type externe suivi par un point, le nom de l'élément et la chaîne "Type".  
  
-   Si un contrat de données avec un tel nom existe déjà, le nom est rendu unique en ajoutant "1", "2", "3" et ainsi de suite jusqu'à ce qu'un nom unique soit créé.  
  
## Types simples \- \<xs:simpleType\>  
  
### \<xs:simpleType\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`final`|Ignoré.|  
|`id`|Ignoré.|  
|`name`|Pris en charge, mappe au nom du contrat de données.|  
  
### \<xs:simpleType\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`restriction`|Prise en charge. Mappe aux contrats de données d'énumération. Cet attribut est ignoré s'il ne correspond pas au modèle d'énumération. Consultez la section des restrictions `xs:simpleType`.|  
|`list`|Prise en charge. Mappe aux contrats de données d'énumération d'indicateur. Consultez la section répertoriant `xs:simpleType`.|  
|`union`|Interdit.|  
  
### \<xs: restriction\>  
  
-   Les restrictions de type complexe sont prises en charge uniquement pour la base \= "`xs:anyType`".  
  
-   Les restrictions de type simple de `xs:string` qui n'ont pas de facettes de restriction autres que `xs:enumeration` sont mappées aux contrats de données d'énumération.  
  
-   Toutes les autres restrictions de type simple sont mappées aux types qu'elles restreignent. Par exemple, une restriction de `xs:int` mappe à un entier, tout comme `xs:int` lui\-même.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] le mappage de type primitif, consultez le Mappage de type\/primitif.  
  
### \<xs:restriction\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`base`|Doit être un type simple pris en charge ou `xs:anyType`.|  
|`id`|Ignoré.|  
  
### \<xs:restriction\> pour tous les autres cas : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`simpleType`|Si présent, doit être dérivé d'un type primitif pris en charge.|  
|`minExclusive`|Ignoré.|  
|`minInclusive`|Ignoré.|  
|`maxExclusive`|Ignoré.|  
|`maxInclusive`|Ignoré.|  
|`totalDigits`|Ignoré.|  
|`fractionDigits`|Ignoré.|  
|`length`|Ignoré.|  
|`minLength`|Ignoré.|  
|`maxLength`|Ignoré.|  
|`enumeration`|Ignoré.|  
|`whiteSpace`|Ignoré.|  
|`pattern`|Ignoré.|  
|\(vide\)|Prise en charge.|  
  
## Énumération  
  
### \<xs:restriction\> pour les énumérations : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`base`|Si présent, doit être `xs:string`.|  
|`id`|Ignoré.|  
  
### \<xs:restriction\> pour les énumérations : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`simpleType`|Si présent, doit être une restriction d'énumération prise en charge par le contrat de données \(cette section\).|  
|`minExclusive`|Ignoré.|  
|`minInclusive`|Ignoré.|  
|`maxExclusive`|Ignoré.|  
|`maxInclusive`|Ignoré.|  
|`totalDigits`|Ignoré.|  
|`fractionDigits`|Ignoré.|  
|`length`|Interdit.|  
|`minLength`|Interdit.|  
|`maxLength`|Interdit.|  
|`enumeration`|Prise en charge. Le "id" d'énumération est ignoré, et "value" mappe au nom de la valeur dans le contrat de données d'énumération.|  
|`whiteSpace`|Interdit.|  
|`pattern`|Interdit.|  
|\(vide\)|Pris en charge, mappe au type énumération vide.|  
  
 Le code suivant montre une classe d'énumération C\#.  
  
```  
public enum MyEnum  
{  
   first = 3,  
   second = 4,  
   third =5  
```  
  
 }  
  
 Cette classe mappe au schéma suivant par `DataContractSerializer`. Si les valeurs d'énumération démarrent à partir de 1, les blocs `xs:annotation` ne sont pas générés.  
  
```  
<xs:simpleType name="MyEnum">  
<xs:restriction base="xs:string">  
 <xs:enumeration value="first">  
  <xs:annotation>  
   <xs:appinfo>  
    <EnumerationValue   
     xmlns="http://schemas.microsoft.com/2003/10/Serialization/">  
     3  
    </EnumerationValue>  
   </xs:appinfo>  
  </xs:annotation>  
 </xs:enumeration>  
 <xs:enumeration value="second">  
  <xs:annotation>  
   <xs:appinfo>  
    <EnumerationValue   
     xmlns="http://schemas.microsoft.com/2003/10/Serialization/">  
     4  
    </EnumerationValue>  
   </xs:appinfo>  
  </xs:annotation>  
 </xs:enumeration>   
</xs:restriction>  
</xs:simpleType>  
```  
  
### \<xs:list\>  
 `DataContractSerializer` mappe des types énumération marqués avec `System.FlagsAttribute` à `xs:list` dérivé de `xs:string`. Aucune autre variation `xs:list` n'est prise en charge.  
  
### \<xs:list\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`itemType`|Interdit.|  
|`id`|Ignoré.|  
  
### \<xs:list\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`simpleType`|Doit être une restriction de `xs:string` utilisant la facette `xs:enumeration`.|  
  
 Si la valeur d'énumération ne suit pas une progression de puissance de 2 \(valeur par défaut pour les indicateurs\), la valeur est stockée dans l'élément `xs:annotation/xs:appInfo/ser:EnumerationValue`.  
  
 Par exemple, le code suivant marque un type énumération.  
  
```  
[Flags]  
public enum AuthFlags  
{    
  AuthAnonymous = 1,  
  AuthBasic = 2,  
  AuthNTLM = 4,  
  AuthMD5 = 16,  
  AuthWindowsLiveID = 64,  
}  
```  
  
 Ce type mappe au schéma suivant.  
  
```  
<xs:simpleType name="AuthFlags">  
    <xs:list>  
      <xs:simpleType>  
        <xs:restriction base="xs:string">  
          <xs:enumeration value="AuthAnonymous" />  
          <xs:enumeration value="AuthBasic" />  
          <xs:enumeration value="AuthNTLM" />  
          <xs:enumeration value="AuthMD5">  
            <xs:annotation>  
              <xs:appinfo>  
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Se  
rialization/">16</EnumerationValue>  
              </xs:appinfo>  
            </xs:annotation>  
          </xs:enumeration>  
          <xs:enumeration value="AuthWindowsLiveID">  
            <xs:annotation>  
              <xs:appinfo>  
                <EnumerationValue xmlns="http://schemas.microsoft.com/2003/10/Se  
rialization/">64</EnumerationValue>  
              </xs:appinfo>  
            </xs:annotation>  
          </xs:enumeration>  
        </xs:restriction>  
      </xs:simpleType>  
    </xs:list>  
  </xs:simpleType>  
```  
  
## Héritage  
  
### Règles Générales  
 Un contrat de données peut hériter d'un autre contrat de données. De tels contrats de données mappent à une base et sont dérivés des types d'extension à l'aide de la construction de schéma XML `<xs:extension>`.  
  
 Un contrat de données ne peut pas hériter d'un contrat de données de collection.  
  
 Le code ci\-dessous montre un exemple de contrat de données.  
  
```  
[DataContract]  
public class Person  
{  
  [DataMember]  
  public string Name;  
}  
[DataContract]  
public class Employee : Person   
{      
  [DataMember]  
  public int ID;  
}  
```  
  
 Ce contrat de données mappe à la déclaration de type de schéma XML suivante.  
  
```  
<xs:complexType name="Employee">  
 <xs:complexContent mixed="false">  
  <xs:extension base="tns:Person">  
   <xs:sequence>  
    <xs:element minOccurs="0" name="ID" type="xs:int"/>  
   </xs:sequence>  
  </xs:extension>  
 </xs:complexContent>  
</xs:complexType>  
<xs:complexType name="Person">  
 <xs:sequence>  
  <xs:element minOccurs="0" name="Name"   
    nillable="true" type="xs:string"/>  
 </xs:sequence>  
</xs:complexType>  
```  
  
### \<xs:complexContent\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`id`|Ignoré.|  
|`mixed`|Doit être faux.|  
  
### \<xs:complexContent\> : contenu  
  
|Sommaire|Schéma|  
|--------------|------------|  
|`restriction`|Interdit, sauf lorsque la base \= "`xs:anyType`". Ce qui précède équivaut à placer directement le contenu de `xs:restriction` sous le conteneur de `xs:complexContent`.|  
|`extension`|Prise en charge. Mappe à l'héritage de contrat de données.|  
  
### \<xs:extension\> dans \<xs:complexContent\> : attributs  
  
|Attribut|Schéma|  
|--------------|------------|  
|`id`|Ignoré.|  
|`base`|Prise en charge. Mappe au type de contrat de données de base de qui ce type hérite.|  
  
### \<xs:extension\> dans \<xs:complexContent\> : contenu  
 Les règles sont les mêmes que pour le contenu `<xs:complexType>`.  
  
 Si un `<xs:sequence>` est fourni, ses éléments membres mappent aux membres de données supplémentaires qui sont présents dans le contrat de données dérivé.  
  
 Si un type dérivé contient un élément portant le même nom qu'un élément dans un type de base, la déclaration d'élément en double mappe à un membre de données avec un nom généré pour être unique. Des entiers positifs sont ajoutés au nom du membre de données \("member1", "member2" et ainsi de suite\) jusqu'à ce qu'un nom unique soit trouvé. Inversement :  
  
-   Si un contrat de données dérivé a un membre de données portant le même nom et type qu'un membre de données dans un contrat de données de base, `DataContractSerializer` génère cet élément correspondant dans le type dérivé.  
  
-   Si un contrat de données dérivé a un membre de données portant le même nom qu'un membre de données dans un contrat de données de base mais ayant un type différent, `DataContractSerializer` importe un schéma avec un élément de type `xs:anyType` à la fois dans le type de base et dans les déclarations de type dérivé. Le nom du type d'origine est conservé dans `xs:annotations/xs:appInfo/ser:ActualType/@Name`.  
  
 Les deux variations peuvent aboutir à schéma ayant un modèle de contenu ambigu, qui dépend de l'ordre des membres de données respectifs.  
  
## Mappage de type\/primitif  
 `DataContractSerializer` utilise le mappage suivant pour les types primitifs de schéma XML.  
  
|Type XSD|Type .NET|  
|--------------|---------------|  
|`anyType`|<xref:System.Object>.|  
|`anySimpleType`|<xref:System.String>.|  
|`duration`|<xref:System.TimeSpan>.|  
|`dateTime`|<xref:System.DateTime>.|  
|`dateTimeOffset`|Objets <xref:System.DateTime> et <xref:System.TimeSpan> pour l'offset. Consultez Sérialisation DateTimeOffset ci\-dessous.|  
|`time`|<xref:System.String>.|  
|`date`|<xref:System.String>.|  
|`gYearMonth`|<xref:System.String>.|  
|`gYear`|<xref:System.String>.|  
|`gMonthDay`|<xref:System.String>.|  
|`gDay`|<xref:System.String>.|  
|`gMonth`|<xref:System.String>.|  
|`boolean`|<xref:System.Boolean>|  
|`base64Binary`|Tableau <xref:System.Byte>.|  
|`hexBinary`|<xref:System.String>.|  
|`float`|<xref:System.Single>.|  
|`double`|<xref:System.Double>.|  
|`anyURI`|<xref:System.Uri>.|  
|`QName`|<xref:System.Xml.XmlQualifiedName>.|  
|`string`|<xref:System.String>.|  
|`normalizedString`|<xref:System.String>.|  
|`token`|<xref:System.String>.|  
|`language`|<xref:System.String>.|  
|`Name`|<xref:System.String>.|  
|`NCName`|<xref:System.String>.|  
|`ID`|<xref:System.String>.|  
|`IDREF`|<xref:System.String>.|  
|`IDREFS`|<xref:System.String>.|  
|`ENTITY`|<xref:System.String>.|  
|`ENTITIES`|<xref:System.String>.|  
|`NMTOKEN`|<xref:System.String>.|  
|`NMTOKENS`|<xref:System.String>.|  
|`decimal`|<xref:System.Decimal>.|  
|`integer`|<xref:System.Int64>.|  
|`nonPositiveInteger`|<xref:System.Int64>.|  
|`negativeInteger`|<xref:System.Int64>.|  
|`long`|<xref:System.Int64>.|  
|`int`|<xref:System.Int32>.|  
|`short`|<xref:System.Int16>.|  
|`Byte`|<xref:System.SByte>.|  
|`nonNegativeInteger`|<xref:System.Int64>.|  
|`unsignedLong`|<xref:System.UInt64>.|  
|`unsignedInt`|<xref:System.UInt32>.|  
|`unsignedShort`|<xref:System.UInt16>.|  
|`unsignedByte`|<xref:System.Byte>.|  
|`positiveInteger`|<xref:System.Int64>.|  
  
## Mappage de types ISerializable  
 Dans le [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] version 1.0, `ISerializable` a été introduit comme mécanisme général pour sérialiser des objets pour la persistance ou le transfert de données. Il existe de nombreux types [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] qui implémentent `ISerializable` et qui peuvent être passés entre les applications.`DataContractSerializer` fournit naturellement le support pour les classes `ISerializable`.`DataContractSerializer` mappe des types de schéma d'implémentation `ISerializable` qui diffèrent uniquement par le QName \(nom qualifié\) du type et sont des collections de propriétés. Par exemple, `DataContractSerializer` mappe <xref:System.Exception> au type XSD suivant dans l'espace de noms http:\/\/schemas.datacontract.org\/2004\/07\/System.  
  
```  
<xs:complexType name="Exception">  
 <xs:sequence>  
  <xs:any minOccurs="0" maxOccurs="unbounded"   
      namespace="##local" processContents="skip"/>  
 </xs:sequence>  
 <xs:attribute ref="ser:FactoryType"/>  
</xs:complexType>  
```  
  
 L'attribut facultatif `ser:FactoryType` déclaré dans le schéma de sérialisation du contrat de données référence une classe de fabrique qui peut désérialiser le type. La classe de fabrique doit faire partie de la collection de types connus de l'instance `DataContractSerializer` qui est utilisée.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] les types connus, consultez [Types connus de contrats de données](../../../../docs/framework/wcf/feature-details/data-contract-known-types.md).  
  
## Schéma de sérialisation DataContract  
 Plusieurs schémas exportés par `DataContractSerializer`, utilisent des types, des éléments et des attributs d'un espace de noms spécial de la sérialisation du contrat de données :  
  
 http:\/\/schemas.microsoft.com\/2003\/10\/Serialization  
  
 Voici une déclaration de schéma de sérialisation du contrat de données complète.  
  
```  
<xs:schema attributeFormDefault="qualified"          
   elementFormDefault="qualified"        
   targetNamespace =   
    "http://schemas.microsoft.com/2003/10/Serialization/"   
   xmlns:xs="http://www.w3.org/2001/XMLSchema"        
   xmlns:tns="http://schemas.microsoft.com/2003/10/Serialization/">  
  
 <!-- Top-level elements for primitive types. -->  
 <xs:element name="anyType" nillable="true" type="xs:anyType"/>  
 <xs:element name="anyURI" nillable="true" type="xs:anyURI"/>  
 <xs:element name="base64Binary"   
       nillable="true" type="xs:base64Binary"/>  
 <xs:element name="boolean" nillable="true" type="xs:boolean"/>  
 <xs:element name="byte" nillable="true" type="xs:byte"/>  
 <xs:element name="dateTime" nillable="true" type="xs:dateTime"/>  
 <xs:element name="decimal" nillable="true" type="xs:decimal"/>  
 <xs:element name="double" nillable="true" type="xs:double"/>  
 <xs:element name="float" nillable="true" type="xs:float"/>  
 <xs:element name="int" nillable="true" type="xs:int"/>  
 <xs:element name="long" nillable="true" type="xs:long"/>  
 <xs:element name="QName" nillable="true" type="xs:QName"/>  
 <xs:element name="short" nillable="true" type="xs:short"/>  
 <xs:element name="string" nillable="true" type="xs:string"/>  
 <xs:element name="unsignedByte"   
       nillable="true" type="xs:unsignedByte"/>  
 <xs:element name="unsignedInt"   
       nillable="true" type="xs:unsignedInt"/>  
 <xs:element name="unsignedLong"   
       nillable="true" type="xs:unsignedLong"/>  
 <xs:element name="unsignedShort"   
       nillable="true" type="xs:unsignedShort"/>  
  
 <!-- Primitive types introduced for certain .NET simple types. -->  
 <xs:element name="char" nillable="true" type="tns:char"/>  
 <xs:simpleType name="char">  
  <xs:restriction base="xs:int"/>  
 </xs:simpleType>  
  
 <!-- xs:duration is restricted to an ordered value space,   
    to map to System.TimeSpan -->  
 <xs:element name="duration" nillable="true" type="tns:duration"/>  
 <xs:simpleType name="duration">  
  <xs:restriction base="xs:duration">  
   <xs:pattern   
     value="\-?P(\d*D)?(T(\d*H)?(\d*M)?(\d*(\.\d*)?S)?)?"/>  
   <xs:minInclusive value="-P10675199DT2H48M5.4775808S"/>  
   <xs:maxInclusive value="P10675199DT2H48M5.4775807S"/>  
  </xs:restriction>  
 </xs:simpleType>  
  
 <xs:element name="guid" nillable="true" type="tns:guid"/>  
 <xs:simpleType name="guid">  
  <xs:restriction base="xs:string">  
   <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>  
  </xs:restriction>  
 </xs:simpleType>  
  
 <!-- This is used for schemas exported from ISerializable type. -->  
 <xs:attribute name="FactoryType" type="xs:QName"/>  
</xs:schema>  
```  
  
 Il convient de remarquer les points suivants :  
  
-   `ser:char` est introduit pour représenter des caractères Unicode de type <xref:System.Char>.  
  
-   Le `valuespace` de `xs:duration` est réduit à un jeu ordonné afin qu'il puisse être mappé à un <xref:System.TimeSpan>.  
  
-   `FactoryType` est utilisé dans les schémas exportés des types dérivés de <xref:System.Runtime.Serialization.ISerializable>.  
  
## Importation de schémas qui ne sont pas DataContract  
 `DataContractSerializer` dispose de l'option `ImportXmlTypes` pour autoriser l'importation des schémas non conformes au profil XSD `DataContractSerializer` \(consultez la propriété <xref:System.Runtime.Serialization.XsdDataContractImporter.Options%2A>\). Affecter à cette option la valeur `true` active l'acceptation des types de schémas non conformes et les mappe à l'implémentation suivante, <xref:System.Xml.Serialization.IXmlSerializable> qui encapsule un tableau de <xref:System.Xml.XmlNode> \(seul le nom de la classe diffère\).  
  
```  
[GeneratedCodeAttribute("System.Runtime.Serialization", "3.0.0.0")]  
[System.Xml.Serialization.XmlSchemaProviderAttribute("ExportSchema")]  
[System.Xml.Serialization.XmlRootAttribute(IsNullable=false)]  
public partial class Person : object, IXmlSerializable  
{    
  private XmlNode[] nodesField;    
  private static XmlQualifiedName typeName =   
new XmlQualifiedName("Person","http://Microsoft.ServiceModel.Samples");    
  public XmlNode[] Nodes  
  {  
    get {return this.nodesField;}  
    set {this.nodesField = value;}  
  }    
  public void ReadXml(XmlReader reader)  
  {  
    this.nodesField = XmlSerializableServices.ReadNodes(reader);  
  }    
  public void WriteXml(XmlWriter writer)  
  {  
    XmlSerializableServices.WriteNodes(writer, this.Nodes);  
  }    
  public System.Xml.Schema.XmlSchema GetSchema()  
  {  
    return null;  
  }    
  public static XmlQualifiedName ExportSchema(XmlSchemaSet schemas)  
  {  
    XmlSerializableServices.AddDefaultSchema(schemas, typeName);  
    return typeName;  
  }  
}  
```  
  
## Sérialisation DateTimeOffset  
 <xref:System.DateTimeOffset> n'est pas traité comme un type primitif. À la place, il est sérialisé comme un élément complexe comportant deux parties. La première partie représente la date et l'heure et la deuxième partie représente l'offset de la date et de l'heure. Un exemple d'une valeur DateTimeOffset sérialisée est illustré dans le code suivant.  
  
```  
<OffSet xmlns:a="http://schemas.datacontract.org/2004/07/System">  
  <DateTime i:type="b:dateTime"   
    xmlns:b="http://www.w3.org/2001/XMLSchema">2008-08-28T08:00:00    
  </DateTime>   
  <OffsetMinutes i:type="b:short"   
   xmlns:b="http://www.w3.org/2001/XMLSchema">-480  
   </OffsetMinutes>   
</OffSet>  
```  
  
 Le schéma se présente comme suit :  
  
```  
<xs:schema targetNamespace="http://schemas.datacontract.org/2004/07/System">  
   <xs:complexType name="DateTimeOffset">  
      <xs:sequence minOccurs="1" maxOccurs="1">  
         <xs:element name="DateTime" type="xs:dateTime"  
         minOccurs="1" maxOccurs="1" />  
         <xs:elementname="OffsetMinutes" type="xs:short"  
         minOccurs="1" maxOccurs="1" />  
      </xs:sequence>  
   </xs:complexType>  
</xs:schema>  
```  
  
## Voir aussi  
 <xref:System.Runtime.Serialization.DataContractSerializer>   
 <xref:System.Runtime.Serialization.DataContractAttribute>   
 <xref:System.Runtime.Serialization.DataMemberAttribute>   
 <xref:System.Runtime.Serialization.XsdDataContractImporter>   
 [Utilisation de contrats de données](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)