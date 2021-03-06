---
title: "My.Request (Objeto) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.suite: ""
ms.technology: 
  - "devlang-visual-basic"
ms.topic: "article"
f1_keywords: 
  - "My.MyWebExtension.Request"
  - "My.Request"
dev_langs: 
  - "VB"
helpviewer_keywords: 
  - "My.Request (objeto)"
ms.assetid: 93d5f0e2-6b60-4a2c-8652-d90216f6ad10
caps.latest.revision: 9
author: "stevehoag"
ms.author: "shoag"
caps.handback.revision: 9
---
# My.Request (Objeto)
[!INCLUDE[vs2017banner](../../../visual-basic/developing-apps/includes/vs2017banner.md)]

Obtiene el objeto <xref:System.Web.HttpRequest> para la página solicitada.  
  
## Comentarios  
 El objeto `My.Request` contiene información sobre la solicitud HTTP actual.  
  
 El objeto `My.Request` sólo está disponible para las aplicaciones ASP.NET.  
  
## Ejemplo  
 El ejemplo siguiente obtiene la colección de encabezados del objeto `My.Request` y utiliza el objeto `My.Response` para escribirlo en la página ASP.NET.  
  
 [!code-vb[VbVbalrMyWeb#1](../../../visual-basic/language-reference/objects/codesnippet/VisualBasic/my-request-object_1.aspx)]  
  
## Vea también  
 <xref:System.Web.HttpRequest>   
 [My.Response \(Objeto\)](../../../visual-basic/language-reference/objects/my-response-object.md)