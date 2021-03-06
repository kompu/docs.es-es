---
title: goto (Referencia de C#) | Microsoft Docs
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- goto_CSharpKeyword
- goto
dev_langs:
- CSharp
helpviewer_keywords:
- goto keyword [C#]
ms.assetid: 2c03c9c1-8119-44ef-b740-fb3d287a42fe
caps.latest.revision: 22
author: BillWagner
ms.author: wiwagn
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
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 700376096d969052a9f4633f565f9d9fd4dfec46
ms.lasthandoff: 03/13/2017

---
# <a name="goto-c-reference"></a>goto (Referencia de C#)
La instrucción `goto` transfiere el control del programa directamente a una instrucción con etiqueta.  
  
 Un uso común de `goto` consiste en transferir el control a una etiqueta de switch case específica o a la etiqueta predeterminada en una instrucción `switch`.  
  
 La instrucción `goto` también es útil para salir de bucles demasiado anidados.  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se muestra cómo usar `goto` en una instrucción [switch](../../../csharp/language-reference/keywords/switch.md).  
  
 [!code-cs[csrefKeywordsJump#4](../../../csharp/language-reference/keywords/codesnippet/CSharp/goto_1.cs)]  
  
## <a name="example"></a>Ejemplo  
 En el ejemplo siguiente se muestra cómo usar `goto` para salir de bucles anidados.  
  
 [!code-cs[csrefKeywordsJump#5](../../../csharp/language-reference/keywords/codesnippet/CSharp/goto_2.cs)]  
  
## <a name="c-language-specification"></a>Especificación del lenguaje C#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>Vea también  
 [Referencia de C#](../../../csharp/language-reference/index.md)   
 [Guía de programación de C#](../../../csharp/programming-guide/index.md)   
 [Palabras clave de C#](../../../csharp/language-reference/keywords/index.md)   
 [goto (instrucción)](https://docs.microsoft.com/cpp/cpp/goto-statement-cpp)   
 [Instrucciones de salto](../../../csharp/language-reference/keywords/jump-statements.md)
