---
title: "&lt;&lt;= Operador (Referencia de C#) | Microsoft Docs"
ms.date: "2015-07-20"
ms.prod: ".net"
ms.technology: 
  - "devlang-csharp"
ms.topic: "article"
f1_keywords: 
  - "<<=_CSharpKeyword"
dev_langs: 
  - "CSharp"
helpviewer_keywords: 
  - "<<= (operador de asignación y desplazamiento a la izquierda) [C#]"
  - "operador de asignación y desplazamiento a la izquierda (<<=) [C#]"
ms.assetid: 3bc99c78-1edb-4827-86fc-bce6c3048871
caps.latest.revision: 16
author: "BillWagner"
ms.author: "wiwagn"
caps.handback.revision: 16
---
# &lt;&lt;= Operador (Referencia de C#)
El operador de asignación y desplazamiento a la izquierda.  
  
## Comentarios  
 Una expresión de la forma  
  
```  
x <<= y  
```  
  
 se evalúa como  
  
```  
x = x << y  
```  
  
 salvo que `x` sólo se evalúa una vez.  El [operador \<\<](../../../csharp/language-reference/operators/left-shift-operator.md) desplaza `x` a la izquierda el número de bits especificado por `y`.  
  
 El operador `<<=` no se puede sobrecargar directamente, pero los tipos definidos por el usuario sí pueden sobrecargar el [operador \<\<](../../../csharp/language-reference/operators/left-shift-operator.md) \(vea [operador](../../../csharp/language-reference/keywords/operator.md)\).  
  
## Ejemplo  
 [!code-cs[csRefOperators#12](../../../csharp/language-reference/operators/codesnippet/CSharp/left-shift-assignment-operator_1.cs)]  
  
## Vea también  
 [Referencia de C\#](../../../csharp/language-reference/index.md)   
 [Guía de programación de C\#](../../../csharp/programming-guide/index.md)   
 [operadores de C\#](../../../csharp/language-reference/operators/index.md)