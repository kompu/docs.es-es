---
title: switch (palabra clave) (Referencia de C#) | Microsoft Docs
ms.date: 2017-03-07
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- switch_CSharpKeyword
- switch
- case
- case_CSharpKeyword
dev_langs:
- CSharp
helpviewer_keywords:
- switch statement [C#]
- switch keyword [C#]
- case statement [C#]
- default keyword [C#]
ms.assetid: 44bae8b8-8841-4d85-826b-8a94277daecb
caps.latest.revision: 47
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: b6f8b110e087093bd47573a1a4a05752be91e743
ms.lasthandoff: 03/13/2017

---
# <a name="switch-c-reference"></a>switch (Referencia de C#)
`switch` es una instrucción de selección que elige una sola *sección switch* para ejecutarla desde una lista de candidatos en función de una coincidencia de patrones con la *expresión de coincidencia*. 
  
 [!code-cs[switch#1](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch1.cs#1)]  

La instrucción `switch` se suele usar como alternativa a un constructor [if-else](if-else.md) si una sola expresión se prueba con tres o más condiciones. Por ejemplo, la siguiente instrucción `switch` determina si una variable de tipo `Color` tiene uno de tres valores: 

[!code-cs[switch#3](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch3.cs#1)] 

Es equivalente al siguiente ejemplo que usa un constructor `if`-`else`. 

[!code-cs[switch#3a](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch3a.cs#1)] 

## <a name="the-match-expression"></a>Expresión de coincidencia

La expresión de coincidencia proporciona el valor que debe coincidir con los patrones de las etiquetas `case`. Su sintaxis es:

```csharp
   switch (expr)
```

En C# 6, la expresión de coincidencia debe ser una expresión que devuelva un valor de los siguientes tipos:

- Un [carácter](char.md).
- Una [cadena](string.md).
- Un [booleano](bool.md). 
- Un valor entero, como [int](int.md) o [long](long.md).
- Un valor [enum](enum.md).

A partir de C# 7, la expresión de coincidencia puede ser cualquier expresión que no sea nula.
 
## <a name="the-switch-section"></a>Sección switch
 
 Una instrucción `switch` incluye una o más secciones switch. Cada sección switch contiene una o más *etiquetas case* seguidas de una o más instrucciones. En el ejemplo siguiente se muestra una instrucción `switch` simple con tres secciones switch. Cada sección switch tiene una etiqueta case, por ejemplo, `case 1:`, y dos instrucciones.
 
  Una instrucción `switch` puede incluir cualquier número de secciones switch y cada sección puede tener una o más etiquetas case, como se muestra en el ejemplo siguiente. Pero dos etiquetas case no pueden contener la misma expresión.  

 [!code-cs[switch#2](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch2.cs#1)]  

 Solo se ejecuta una sección switch en una instrucción switch. C# no permite que la ejecución continúe de una sección switch a la siguiente. Por este motivo, el código siguiente genera un error del compilador, CS0163: "El control no puede pasar explícitamente de una etiqueta case (<case label>) a otra".   

```csharp  
switch (caseSwitch)  
{  
    // The following switch section causes an error.  
    case 1:  
        Console.WriteLine("Case 1...");  
        // Add a break or other jump statement here.  
    case 2:  
        Console.WriteLine("... and/or Case 2");  
        break;  
}  
```  
Este requisito se suele cumplir al salir explícitamente de la sección switch mediante una instrucción [break](break.md), [goto](goto.md) o [return](return.md). Pero el código siguiente también es válido, porque garantiza que el control del programa no puede pasar explícitamente a la sección switch `default`.
  
 [!code-cs[switch#4](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch4.cs#1)]    
  
 La ejecución de la lista de instrucciones en la sección switch con una etiqueta case que coincide con la expresión de coincidencia comienza con la primera instrucción y continúa a lo largo de la lista de instrucciones, normalmente hasta que se alcanza una instrucción de salto, como `break`, `goto case`, `return` o `throw`. En este punto, el control se transfiere fuera de la instrucción `switch` o a otra etiqueta case.  

## <a name="case-labels"></a>Etiquetas case

 Cada etiqueta case especifica un patrón que se compara con la expresión de coincidencia (la variable `caseSwitch` en los ejemplos anteriores). Si coinciden, el control se transfiere a la sección switch que contiene la **primera** etiqueta case coincidente. Si ningún patrón de la etiqueta case coincide con la expresión de coincidencia, el control se transfiere a la sección con la etiqueta case `default`, si la hubiera. Si no hay ninguna etiqueta case `default`, no se ejecuta ninguna instrucción de ninguna sección switch y el control se transfiere fuera de la instrucción `switch`.

 Para más información sobre la instrucción `switch` y la coincidencia de patrones, vea la sección [Coincidencia de patrones con la instrucción `switch`](#pattern).

 Dado que C# 6 solo admite el patrón constante y no permite la repetición de valores constantes, las etiquetas case definen valores mutuamente exclusivos y solo un patrón puede coincidir con la expresión de coincidencia. Por este motivo, el orden en que aparezcan las instrucciones `case` no tiene importancia.

 Pero en C# 7, dado que se admiten otros patrones, las etiquetas case no necesitan definir valores mutuamente exclusivos y varios patrones pueden coincidir con la expresión de coincidencia. Puesto que solo se ejecutan las instrucciones de la sección switch que contiene el primer patrón coincidente, el orden en que aparecen las instrucciones `case` sí es importante. Si C# detecta una sección switch cuya instrucción o instrucciones case son equivalentes a o son subconjuntos de instrucciones anteriores, genera un error del compilador, CS8120: "El caso del modificador ya se ha gestionado en un caso anterior". 

 En el ejemplo siguiente se muestra una instrucción `switch` que usa una variedad de patrones que no son mutuamente excluyentes. Si mueve la sección switch `case 0:` de modo que ya no sea la primera sección de la instrucción `switch`, C# genera un error del compilador debido a que un entero cuyo valor es cero es un subconjunto de todos los enteros, que es el patrón definido por la instrucción `case int val`.

 [!code-cs[switch#5](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch5.cs#1)]    

Puede corregir este problema y eliminar la advertencia del compilador de alguna de estas dos formas:

- Si cambia el orden de las secciones switch. 
 
- Si usa una </a name="when">cláusula when</a> en la etiqueta `case`.
 
## <a name="the-default-case"></a>Etiqueta case `default`

La etiqueta case `default` especifica la sección switch que se va a ejecutar si la expresión de coincidencia no coincide con ninguna otra etiqueta `case`. Si no hay ninguna etiqueta case `default` y la expresión de coincidencia no coincide con ninguna otra etiqueta `case`, el flujo del programa pasa a la instrucción `switch`.

La etiqueta case `default` puede aparecer en cualquier orden en la instrucción `switch`. Independientemente de su orden en el código fuente, siempre se evalúa en último lugar, después de que se hayan evaluado las demás etiquetas `case`.

## <a name="a-namepattern--pattern-matching-with-the-switch-statement"></a><a name="pattern" /> Coincidencia de patrones con la instrucción `switch`
  
Cada instrucción `case` define un patrón que, si coincide con la expresión de coincidencia, provoca la ejecución de su sección switch contenedora. Todas las versiones de C# admiten el patrón de constante. Los demás patrones se admiten a partir de C# 7. 
  
### <a name="constant-pattern"></a>Patrón de constante 

El patrón de constante comprueba si la expresión de coincidencia es igual a una constante especificada. Su sintaxis es:

```csharp
   case constant:
```

donde *constant* es el valor que se va a comprobar. *constant* puede ser cualquiera de las expresiones de constante siguientes: 

- Un literal [booleano](bool.md), ya sea `true` o `false`.
- Cualquier constante entera, como [int](int.md), [long](long.md) o [byte](byte.md). 
- El nombre de una variable `const` declarada.
- Una constante de enumeración.
- Un literal de [carácter](char.md).
- Un literal de [cadena](string.md).

La expresión de constante se evalúa de la siguiente forma:

- Si *expr* y *constant* son tipos enteros, el operador de igualdad de C# determina si la expresión devuelve `true` (es decir, si `expr == constant`).

- De lo contrario, el valor de la expresión se determina mediante una llamada al método estático [Object.Equals(expr, constant)](xref:System.Object.Equals(System.Object,System.Object)).  

En el ejemplo siguiente se usa el patrón de constante para determinar si una fecha determinada es un fin de semana, el primer día, el último día o la mitad de la semana laboral. Evalúa la propiedad [DateTime.DayOfWeek](xref:System.DateTime.DayOfWeek) del día actual con los miembros de la enumeración @System.DayOfWeek. 

[!code-cs[switch#7](../../../../samples/snippets/csharp/language-reference/keywords/switch/const-pattern.cs#1)]

En el ejemplo siguiente se usa el patrón de constante para controlar la entrada del usuario en una aplicación de consola que simula una cafetera automática.
  
 [!code-cs[switch#6](../../../../samples/snippets/csharp/language-reference/keywords/switch/switch6.cs)]  

### <a name="type-pattern"></a>Patrón de tipo

El patrón de tipo habilita la conversión y la evaluación de tipo concisas. Cuando se usa con la instrucción `switch` para realizar la coincidencia de patrones, comprueba si una expresión se puede convertir en un tipo especificado y, en caso afirmativo, la convierte en una variable de ese tipo. Su sintaxis es:

```csharp
   case type varname 
```
donde *type* es el nombre del tipo al que se va a convertir el resultado de *expr* y *varname* es el objeto al que se va a convertir el resultado de *expr* si hay coincidencia. 

La expresión `case` es `true` si se cumple alguna de las siguientes condiciones:

- *expr* es una instancia del mismo tipo que *type*.

- *expr* es una instancia de un tipo que deriva de *type*. En otras palabras, el resultado de *expr* puede ser convertido en una instancia de *type*.

- *expr* tiene un tipo en tiempo de compilación que es una clase base de *type* y *expr* tiene un tipo en tiempo de ejecución que es *type* o se deriva de *type*. El *tipo en tiempo de compilación* de una variable es el tipo de la variable tal como se define en su declaración de tipos. El *tipo en tiempo de ejecución* de una variable es el tipo de la instancia que se asigna a esa variable.

- *type* es una instancia de un tipo que implementa la interfaz *type*.

Si la expresión case es true, *varname* se asigna definitivamente y tiene ámbito local únicamente dentro de la sección switch.

Tenga en cuenta que `null` no coincide con un tipo. Para que `null` coincida, use la siguiente etiqueta `case`:

```csharp
case null:
```
 
En el ejemplo siguiente se usa el patrón de tipo para proporcionar información sobre los distintos tipos de colección.

[!code-cs[switch#5](../../../../samples/snippets/csharp/language-reference/keywords/switch/type-pattern.cs#1)]

Sin coincidencia de patrones, este código podría escribirse del modo siguiente. El uso de la coincidencia de patrones de tipo genera código más compacto y legible al eliminar la necesidad de comprobar si el resultado de una conversión es `null` o de realizar conversiones repetidas.  

[!code-cs[switch#6](../../../../samples/snippets/csharp/language-reference/keywords/switch/type-pattern2.cs#1)]

## <a name="the-case-statement-and-the-when-clause"></a>Instrucción `case` y cláusula `when`

A partir de C# 7, dado que las instrucciones case no necesitan ser mutuamente excluyentes, puede agregar una cláusula `when` para especificar una condición adicional que deba cumplirse para que la instrucción case se evalúe como true. La cláusula `when` puede ser cualquier expresión que devuelva un valor booleano. Uno de los usos más comunes de la cláusula `when` es evitar que una sección switch se ejecute cuando el valor de una expresión de coincidencia sea `null`. 

 En el ejemplo siguiente se define una clase base `Shape`, una clase `Rectangle` que deriva de `Shape` y una clase `Square` que deriva de `Rectangle`. Usa la cláusula `when` para asegurarse de que `ShowShapeInfo` trate a un objeto `Rectangle` al que se han asignado las mismas longitudes y anchos como si fuera `Square` aunque de él no se hayan creado instancias como de un objeto `Square`. El método no intenta mostrar información sobre un objeto que es `null` ni sobre una forma cuya área es cero. 

[!code-cs[switch#8](../../../../samples/snippets/csharp/language-reference/keywords/switch/when-clause.cs#1)]
  
Tenga en cuenta que la cláusula `when` del ejemplo que intenta comprobar si un objeto `Shape` es `null` no se ejecuta. El patrón de tipo correcto para comprobar `null` es `case null:`.

## <a name="c-language-specification"></a>Especificación del lenguaje C#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>Vea también  

 [Referencia de C#](../index.md)   
 [Guía de programación de C#](../../programming-guide/index.md)   
 [Palabras clave de C#](index.md)   
 [if-else](if-else.md)   
 [Coincidencia de patrones](../../pattern-matching.md)   
 

 
