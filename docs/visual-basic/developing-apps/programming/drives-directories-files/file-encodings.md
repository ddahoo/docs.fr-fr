---
title: Encodages de fichiers (Visual Basic) | Microsoft Docs
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
dev_langs:
- VB
helpviewer_keywords:
- character encodings
- files, encoding
- Unicode, file encoding
- file encoding
ms.assetid: ea2c5f5f-bbb1-4150-9928-b9951fa6bc57
caps.latest.revision: 8
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
ms.translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: d59312951429780990e9cc048e3ad0671b81d8ae
ms.contentlocale: fr-fr
ms.lasthandoff: 04/12/2017

---
# <a name="file-encodings-visual-basic"></a>Encodages de fichiers (Visual Basic)
Les encodages de fichiers, également appelés codages de caractères, spécifient comment représenter les caractères lors du traitement du texte. Un encodage peut être préférable à un autre en fonction des caractères de langue qu’il peut ou non gérer, bien qu’Unicode soit généralement privilégié.  
  
 Lors de la lecture ou de l’écriture dans des fichiers, des incohérences d’encodage de fichiers peuvent provoquer des exceptions ou des résultats incorrects.  
  
## <a name="types-of-encodings"></a>Types d’encodages  
 Unicode est l’encodage à utiliser par défaut quand vous travaillez avec des fichiers. Unicode est une norme internationale d’encodage de caractères qui utilise des valeurs de code de 16 bits pour représenter tous les caractères utilisés dans l’informatique moderne, notamment les symboles techniques et les caractères spéciaux utilisés dans l’édition.  
  
 Les anciennes normes d’encodage de caractères se composaient de jeux de caractères traditionnels, tels que le jeu de caractères ANSI Windows qui utilise des valeurs de code de 8 bits ou des combinaisons de valeurs de 8 bits, pour représenter les caractères utilisés dans une langue ou une région géographique spécifique.  
  
## <a name="encoding-class"></a>Classe d’encodage  
 La classe <xref:System.Text.Encoding> représente un encodage de caractères. Ce tableau liste et décrit les types d’encodages disponibles.  
  
|Nom|Description|
|---|---|    
|<xref:System.Text.ASCIIEncoding>|Représente un encodage de caractères ASCII de caractères Unicode.|  
|<xref:System.Text.UnicodeEncoding>|Représente un encodage UTF-16 de caractères Unicode.|  
|<xref:System.Text.UTF32Encoding>|Représente un encodage UTF-32 de caractères Unicode.|  
|<xref:System.Text.UTF7Encoding>|Représente un encodage UTF-7 de caractères Unicode.|  
|<xref:System.Text.UTF8Encoding>|Représente un encodage UTF-8 de caractères Unicode.|  
  
## <a name="see-also"></a>Voir aussi  
 [Lecture à partir de fichiers](../../../../visual-basic/developing-apps/programming/drives-directories-files/reading-from-files.md)   
 [Écriture dans des fichiers](../../../../visual-basic/developing-apps/programming/drives-directories-files/writing-to-files.md)
