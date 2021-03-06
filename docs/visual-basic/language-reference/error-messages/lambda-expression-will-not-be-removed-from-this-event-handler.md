---
title: "Expresión lambda no se quitará de este controlador de eventos | Documentos de Microsoft"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- bc42326
- vbc42326
dev_langs:
- VB
helpviewer_keywords:
- BC42326
ms.assetid: 63214dc6-0112-4245-8ebf-7c9e8f5a5782
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
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: bdf7ad8f8a116c818e72d67150d72d0c96a4dc3b
ms.lasthandoff: 03/13/2017

---
# <a name="lambda-expression-will-not-be-removed-from-this-event-handler"></a>La expresión lambda no se quitará de este controlador de eventos
La expresión lambda no se quitará de este controlador de eventos. Asigne la expresión lambda a una variable y use la variable para agregar y quitar el evento.  
  
 Cuando se utilizan expresiones lambda con controladores de eventos, es podrán que no vea el comportamiento que espera. El compilador genera un nuevo método para cada definición de la expresión lambda, incluso si son idénticos. Por lo tanto, el código siguiente muestra `False`.  
  
```vb  
Module Module1  
  
    Sub Main()  
        Dim fun1 As ChangeInteger = Function(p As Integer) p + 1  
        Dim fun2 As ChangeInteger = Function(p As Integer) p + 1  
        Console.WriteLine(fun1 = fun2)  
    End Sub  
  
    Delegate Function ChangeInteger(ByVal x As Integer) As Integer  
  
End Module  
```  
  
 Cuando se utilizan expresiones lambda con controladores de eventos, esto puede producir resultados inesperados. En el ejemplo siguiente, la expresión lambda agregada por `AddHandler` no quita el `RemoveHandler` instrucción.  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Sub Main()  
  
        ' The following line adds one listener to the event.  
        AddHandler ProcessInteger, Function(m As Integer) m  
  
        ' The following statement searches the current listeners   
        ' for a match to remove. However, this lambda is not the same  
        ' as the previous one, so nothing is removed.  
        RemoveHandler ProcessInteger, Function(m As Integer) m  
  
    End Sub  
End Module  
```  
  
 De forma predeterminada, este mensaje es una advertencia. Para obtener más información sobre cómo ocultar las advertencias o tratar advertencias como errores, vea [configurar advertencias en Visual Basic](https://docs.microsoft.com/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **Id. de error:** BC42326  
  
## <a name="to-correct-this-error"></a>Para corregir este error  
  
-   Para evitar la advertencia y quitar la expresión lambda, asigne la expresión lambda a una variable y usar la variable de la `AddHandler` y `RemoveHandler` instrucciones, como se muestra en el ejemplo siguiente.  
  
```vb  
Module Module1  
  
    Event ProcessInteger(ByVal x As Integer)  
  
    Dim PrintHandler As ProcessIntegerEventHandler  
  
    Sub Main()  
  
        ' Assign the lambda expression to a variable.  
        PrintHandler = Function(m As Integer) m  
  
        ' Use the variable to add the listener.  
        AddHandler ProcessInteger, PrintHandler  
  
        ' Use the variable again when you want to remove the listener.  
        RemoveHandler ProcessInteger, PrintHandler  
  
    End Sub  
End Module  
```  
  
## <a name="see-also"></a>Vea también  
 [Expresiones lambda](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)   
 [Conversión de delegado flexible](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)   
 [Eventos](../../../visual-basic/programming-guide/language-features/events/index.md)
