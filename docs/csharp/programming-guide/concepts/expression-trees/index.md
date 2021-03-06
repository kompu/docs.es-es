---
title: "Árboles de expresión (C#) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 7d0ac21a-6d90-4e2e-8903-528cb78615b7
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: 7e33ed084c560470a486ebbb25035a59ddc18565
ms.openlocfilehash: 87195c8936aba485919d6c717fcbfaa1b282bddc
ms.lasthandoff: 03/31/2017

---
# <a name="expression-trees-c"></a>Árboles de expresión (C#)
Los árboles de expresión representan el código en una estructura de datos en forma de árbol donde cada nodo es una expresión, por ejemplo, una llamada a método o una operación binaria como `x < y`.  
  
 El código representado en árboles de expresión se puede compilar y ejecutar. Esto permite realizar cambios dinámicos en el código ejecutable, ejecutar consultas LINQ en varias bases de datos y crear consultas dinámicas. Para obtener más información sobre los árboles de expresión en LINQ, consulte [How to: Use Expression Trees to Build Dynamic Queries (C#)](../../../../csharp/programming-guide/concepts/expression-trees/how-to-use-expression-trees-to-build-dynamic-queries.md) (Usar árboles de expresión para generar consultas dinámicas en C#).  
  
 Los árboles de expresión también se usan en Dynamic Language Runtime (DLR) para proporcionar interoperabilidad entre los lenguajes dinámicos y .NET Framework y, asimismo, para permitir que los programadores de compiladores emitan árboles de expresión en lugar de Lenguaje intermedio de Microsoft (MSIL). Para obtener más información sobre el entorno DLR, consulte [Dynamic Language Runtime Overview](https://msdn.microsoft.com/library/dd233052) (Información general sobre Dynamic Language Runtime).  
  
 Para crear un árbol de expresión en función de una expresión lambda anónima, puede usar el compilador de Visual Basic o de C#. También puede hacerlo manualmente con el espacio de nombres <xref:System.Linq.Expressions>.  
  
## <a name="creating-expression-trees-from-lambda-expressions"></a>Crear árboles de expresión a partir de expresiones lambda  
 Cuando una expresión lambda se asigna a una variable de tipo <xref:System.Linq.Expressions.Expression%601>, el compilador emite código para crear un árbol de expresión que represente la expresión lambda.  
  
 El compilador de C# puede generar árboles de expresión solo a partir de lambdas de expresión (o de lambdas de una sola línea). No pueden analizar lambdas de instrucción (o lambdas de varias líneas). Para obtener más información sobre las expresiones lambda en C#, consulte [Expresiones lambda](../../../../csharp/programming-guide/statements-expressions-operators/lambda-expressions.md).  
  
 Los siguientes ejemplos de código muestran cómo crear un árbol de expresión que represente la expresión lambda `num => num < 5` con el compilador de C#.  
  
```csharp  
Expression<Func<int, bool>> lambda = num => num < 5;  
```  
  
## <a name="creating-expression-trees-by-using-the-api"></a>Crear árboles de expresión mediante la API  
 Para crear un árbol de expresión con la API, use la clase <xref:System.Linq.Expressions.Expression>. Esta clase contiene métodos de fábrica estáticos que crean nodos de árbol de expresión de tipos específicos, como, por ejemplo, <xref:System.Linq.Expressions.ParameterExpression>, que representa una variable o un parámetro, o <xref:System.Linq.Expressions.MethodCallExpression>, que representa una llamada de método. <xref:System.Linq.Expressions.ParameterExpression>, <xref:System.Linq.Expressions.MethodCallExpression> y el resto de tipos específicos de las expresiones también están definidos en el espacio de nombres <xref:System.Linq.Expressions>. Dichos tipos derivan del tipo abstracto <xref:System.Linq.Expressions.Expression>.  
  
 En el siguiente ejemplo de código se muestra cómo crear un árbol de expresión que represente la expresión lambda `num => num < 5` mediante la API.  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Manually build the expression tree for   
// the lambda expression num => num < 5.  
ParameterExpression numParam = Expression.Parameter(typeof(int), "num");  
ConstantExpression five = Expression.Constant(5, typeof(int));  
BinaryExpression numLessThanFive = Expression.LessThan(numParam, five);  
Expression<Func<int, bool>> lambda1 =  
    Expression.Lambda<Func<int, bool>>(  
        numLessThanFive,  
        new ParameterExpression[] { numParam });  
```  
  
 En .NET Framework 4 y versiones posteriores, la API de árboles de expresión admite también asignaciones y expresiones de flujo de control como bucles, bloques condicionales y bloques `try-catch`. Con la API, se pueden crear árboles de expresión más complejos que los que pueden crear el compilador de C# a partir de expresiones lambda. En el siguiente ejemplo se indica cómo crear un árbol de expresión que calcula el factorial de un número.  
  
```csharp  
// Creating a parameter expression.  
ParameterExpression value = Expression.Parameter(typeof(int), "value");  
  
// Creating an expression to hold a local variable.   
ParameterExpression result = Expression.Parameter(typeof(int), "result");  
  
// Creating a label to jump to from a loop.  
LabelTarget label = Expression.Label(typeof(int));  
  
// Creating a method body.  
BlockExpression block = Expression.Block(  
    // Adding a local variable.  
    new[] { result },  
    // Assigning a constant to a local variable: result = 1  
    Expression.Assign(result, Expression.Constant(1)),  
    // Adding a loop.  
        Expression.Loop(  
    // Adding a conditional block into the loop.  
           Expression.IfThenElse(  
    // Condition: value > 1  
               Expression.GreaterThan(value, Expression.Constant(1)),  
    // If true: result *= value --  
               Expression.MultiplyAssign(result,  
                   Expression.PostDecrementAssign(value)),  
    // If false, exit the loop and go to the label.  
               Expression.Break(label, result)  
           ),  
    // Label to jump to.  
       label  
    )  
);  
  
// Compile and execute an expression tree.  
int factorial = Expression.Lambda<Func<int, int>>(block, value).Compile()(5);  
  
Console.WriteLine(factorial);  
// Prints 120.  
```

Para obtener más información, consulte [Generating Dynamic Methods with Expression Trees in Visual Studio 2010](http://go.microsoft.com/fwlink/p/?LinkId=169513) (Generar métodos dinámicos con árboles de expresión en Visual Studio 2010), que también se aplica a las últimas versiones de Visual Studio.
  
## <a name="parsing-expression-trees"></a>Analizar árboles de expresión  
 En el siguiente ejemplo de código se muestra cómo la expresión del árbol que representa la expresión lambda `num => num < 5` se puede descomponer en partes.  
  
```csharp  
// Add the following using directive to your code file:  
// using System.Linq.Expressions;  
  
// Create an expression tree.  
Expression<Func<int, bool>> exprTree = num => num < 5;  
  
// Decompose the expression tree.  
ParameterExpression param = (ParameterExpression)exprTree.Parameters[0];  
BinaryExpression operation = (BinaryExpression)exprTree.Body;  
ParameterExpression left = (ParameterExpression)operation.Left;  
ConstantExpression right = (ConstantExpression)operation.Right;  
  
Console.WriteLine("Decomposed expression: {0} => {1} {2} {3}",  
                  param.Name, left.Name, operation.NodeType, right.Value);  
  
// This code produces the following output:  
  
// Decomposed expression: num => num LessThan 5  
```  
  
## <a name="immutability-of-expression-trees"></a>Inmutabilidad de los árboles de expresión  
 Los árboles de expresión deben ser inmutables. Esto significa que, si desea modificar un árbol de expresión, deberá construir un nuevo árbol de expresión copiando el ya existente y reemplazando los nodos que hay en él. Puede usar un visitante de árbol de expresión para recorrer el árbol de expresión existente. Para obtener más información, consulte [How to: Modify Expression Trees (C#)](../../../../csharp/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md) (Cómo modificar árboles de expresión en C#).  
  
## <a name="compiling-expression-trees"></a>Compilar árboles de expresión  
 El tipo <xref:System.Linq.Expressions.Expression%601> proporciona el método <xref:System.Linq.Expressions.Expression%601.Compile%2A> para compilar el código representado mediante un árbol de expresión en un delegado ejecutable.  
  
 En el siguiente ejemplo de código se indica cómo compilar un árbol de expresión y ejecutar el código resultante.  
  
```csharp  
// Creating an expression tree.  
Expression<Func<int, bool>> expr = num => num < 5;  
  
// Compiling the expression tree into a delegate.  
Func<int, bool> result = expr.Compile();  
  
// Invoking the delegate and writing the result to the console.  
Console.WriteLine(result(4));  
  
// Prints True.  
  
// You can also use simplified syntax  
// to compile and run an expression tree.  
// The following line can replace two previous statements.  
Console.WriteLine(expr.Compile()(4));  
  
// Also prints True.  
```  
  
 Para obtener más información, consulte [How to: Execute Expression Trees (C#)](../../../../csharp/programming-guide/concepts/expression-trees/how-to-execute-expression-trees.md) (Cómo ejecutar árboles de expresión en C#).  
  
## <a name="see-also"></a>Vea también  
 <xref:System.Linq.Expressions>   
 [How to: Execute Expression Trees (C#)](../../../../csharp/programming-guide/concepts/expression-trees/how-to-execute-expression-trees.md)  (Ejecutar árboles de expresión en C#)  
 [How to: Modify Expression Trees (C#)](../../../../csharp/programming-guide/concepts/expression-trees/how-to-modify-expression-trees.md)  (Modificar árboles de expresión en C#)  
 [Expresiones lambda](../../../../csharp/programming-guide/statements-expressions-operators/lambda-expressions.md)   
 [Dynamic Language Runtime Overview](https://msdn.microsoft.com/library/dd233052)  (Información general sobre Dynamic Language Runtime)  
 [Programming Concepts (C#)](../../../../csharp/programming-guide/concepts/index.md) (Conceptos de programación (C#))
