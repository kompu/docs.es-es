---
title: "Tipos básicos | Guía de C#"
description: "Obtenga información sobre los tipos principales (valores numéricos, cadenas y objeto) en todos los programas de C#"
keywords: .NET, .NET Core, C#
author: BillWagner
ms.author: wiwagn
ms.date: 10/10/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-csharp
ms.devlang: csharp
ms.assetid: 95c686ba-ae4f-440e-8e94-0dbd6e04d11f
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 57c5524a18535778db337500b0a7e9013ce93519
ms.lasthandoff: 03/13/2017

---

# <a name="types-variables-and-values"></a>Tipos, variables y valores  
C# es un lenguaje fuertemente tipado. Todas las variables y constantes tienen un tipo, al igual que todas las expresiones que se evalúan como un valor. Cada una de las firmas de método especifica un tipo para cada parámetro de entrada y para el valor devuelto. La biblioteca de clases .NET Framework define un conjunto de tipos numéricos integrados, así como tipos más complejos que representan una amplia variedad de construcciones lógicas, como el sistema de archivos, conexiones de red, colecciones y matrices de objetos, y fechas. Los programas de C# típicos usan tipos de la biblioteca de clases, así como tipos definidos por el usuario que modelan los conceptos que son específicos del dominio del problema del programa.  
  
Entre la información almacenada en un tipo se puede incluir lo siguiente:  
  
-   El espacio de almacenamiento que requiere una variable del tipo.  
  
-   Los valores máximo y mínimo que puede representar.  
  
-   Los miembros (métodos, campos, eventos, etc.) que contiene.  
  
-   El tipo base del que hereda.  
  
-   La ubicación donde se asignará la memoria para variables en tiempo de ejecución.  
  
-   Los tipos de operaciones permitidas.  
  
El compilador usa información de tipo para garantizar que todas las operaciones que se realizan en el código cuentan *con seguridad de tipos*. Por ejemplo, si declara una variable de tipo [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx), el compilador le permite usar la variable en operaciones de suma y resta. Si intenta realizar esas mismas operaciones en una variable de tipo [bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx), el compilador genera un error, como se muestra en el siguiente ejemplo:  
  
[!code-csharp[Seguridad de tipos](../../samples/snippets/csharp/concepts/basic-types/type-safety.cs)]  
  
> [!NOTE]  
>  Los desarrolladores de C y C++ deben tener en cuenta que, en C#, [bool](https://msdn.microsoft.com/library/c8f5xwh7.aspx) no se puede convertir en [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx).  
  
El compilador inserta la información de tipo en el archivo ejecutable como metadatos. Common Language Runtime (CLR) usa esos metadatos en tiempo de ejecución para garantizar aún más la seguridad de tipos cuando asigna y reclama memoria.  

## <a name="specifying-types-in-variable-declarations"></a>Especificar tipos en declaraciones de variable  
Cuando declare una variable o constante en un programa, debe especificar su tipo o usar la palabra clave [var](https://msdn.microsoft.com/library/bb383973.aspx) para que el compilador infiera el tipo. En el ejemplo siguiente se muestran algunas declaraciones de variable que usan tanto tipos numéricos integrados como tipos complejos definidos por el usuario:  
  
[!code-csharp[Declaración de variable](../../samples/snippets/csharp/concepts/basic-types/variable-declaration.cs)]  
  
Los tipos de parámetros de método y valores devueltos se especifican en la firma del método. En la siguiente firma se muestra un método que requiere una variable [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) como argumento de entrada y devuelve una cadena:  
  
[!code-csharp[Firma del método](../../samples/snippets/csharp/concepts/basic-types/method-signature.cs)]  
  
Tras declarar una variable, no se puede volver a declarar con un nuevo tipo y no se le puede asignar un valor que no sea compatible con su tipo declarado. Por ejemplo, no puede declarar un valor [int](https://msdn.microsoft.com/library/5kzh1b5w.aspx) y, luego, asignarle un valor booleano de [true](https://msdn.microsoft.com/library/06d3w013.aspx). En cambio, los valores se pueden convertir en otros tipos, por ejemplo, cuando se asignan a variables nuevas o se pasan como argumentos de método. El compilador realiza automáticamente una *conversión de tipo* que no da lugar a una pérdida de datos. Una conversión que pueda dar lugar a la pérdida de datos requiere un valor *cast* en el código fuente. 

Para obtener más información, consulte [Conversiones de tipos](https://msdn.microsoft.com/library/ms173105.aspx).
 
## <a name="built-in-types"></a>Tipos integrados
C# proporciona un conjunto estándar de tipos numéricos integrados para representar números enteros, valores de punto flotante, expresiones booleanas, caracteres de texto, valores decimales y otros tipos de datos. También hay tipos **string** y **object** integrados. Están disponibles para su uso en cualquier programa de C#. Para obtener más información sobre los tipos integrados, consulte [Tablas de referencia para tipos](https://msdn.microsoft.com/library/1dhd7f2x.aspx).  
  
## <a name="custom-types"></a>Tipos personalizados  
Las construcciones [struct](https://msdn.microsoft.com/library/ah19swz4.aspx), [class](https://msdn.microsoft.com/library/0b0thckt.aspx), [interface](https://msdn.microsoft.com/library/87d83y5b.aspx) y [enum](https://msdn.microsoft.com/library/sbbt4032.aspx) se usan para crear sus propios tipos personalizados. La biblioteca de clases .NET Framework es en sí misma una colección de tipos personalizados proporcionados por Microsoft que puede usar en sus propias aplicaciones. De forma predeterminada, los tipos usados con más frecuencia en la biblioteca de clases están disponibles en cualquier programa de C#. Otros están disponibles solo cuando agrega explícitamente una referencia de proyecto al ensamblado en el que se definen. Una vez que el compilador tenga una referencia al ensamblado, puede declarar variables (y constantes) de los tipos declarados en dicho ensamblado en el código fuente. Para obtener más información, consulte [.NET Framework class library](https://msdn.microsoft.com/library/gg145045(v=vs.110).aspx) (Biblioteca de clases .NET Framework).  
  
## <a name="generic-types"></a>Tipos genéricos  
Los tipos se pueden declarar con uno o varios *parámetros de tipo* que actúan como un marcador de posición para el tipo real (el *tipo concreto*) que proporcionará el código de cliente cuando cree una instancia del tipo. Estos tipos se denominan *tipos genéricos*. Por ejemplo, el tipo de .NET Framework @System.Collections.Generic.List%601 tiene un parámetro de tipo al que, por convención, se denomina *T*. Cuando crea una instancia del tipo, especifica el tipo de los objetos que contendrá la lista, por ejemplo, la cadena:  
  
[!code-csharp[Tipos genéricos](../../samples/snippets/csharp/concepts/basic-types/generic-type.cs)] 
  
El uso del parámetro de tipo permite volver a usar la misma clase para incluir cualquier tipo de elemento, sin necesidad de convertir cada elemento en [object](https://msdn.microsoft.com/library/9kkx3h3c.aspx). Las clases de colección genéricas se denominan *colecciones fuertemente tipadas* porque el compilador conoce el tipo específico de los elementos de la colección y puede generar un error en tiempo de compilación si, por ejemplo, intenta agregar un valor entero al objeto `strings` del ejemplo anterior. Para obtener más información, consulte [Genéricos](programming-guide/generics/index.md). 

## <a name="implicit-types-anonymous-types-and-tuple-types"></a>Tipos implícitos, tipos anónimos y tipos de tupla  
Como se ha mencionado anteriormente, puede asignar implícitamente un tipo a una variable local (pero no miembros de clase) mediante la palabra clave [var](https://msdn.microsoft.com/library/bb383973.aspx). La variable sigue recibiendo un tipo en tiempo de compilación, pero este lo proporciona el compilador. Para obtener más información, consulte [Variables locales con asignación implícita de tipos](https://msdn.microsoft.com/library/bb384061.aspx).  
  
En algunos casos, resulta conveniente crear un tipo con nombre para conjuntos sencillos de valores relacionados que no quiera almacenar ni pasar fuera de los límites del método. Puede crear *tipos anónimos* para este fin. Para obtener más información, consulte [Anonymous Types](https://msdn.microsoft.com/library/bb397696.aspx) (Tipos anónimos [Guía de programación de C#]).

Es habitual querer devolver más de un valor de un método. Puede crear *tipos de tupla* que devuelven varios valores en una única llamada de método. Para obtener más información, consulte [Tuples](tuples.md) (Tuplas).

## <a name="the-common-type-system"></a>Common Type System  
Es importante entender dos aspectos fundamentales sobre el sistema de tipos en .NET Framework:  
  
-   Es compatible con el principio de herencia. Los tipos pueden derivarse de otros tipos, denominados *tipos base*. El tipo derivado hereda (con algunas restricciones), los métodos, las propiedades y otros miembros del tipo base. A su vez, el tipo base puede derivarse de algún otro tipo, en cuyo caso el tipo derivado hereda los miembros de ambos tipos base en su jerarquía de herencia. Todos los tipos, incluidos los tipos numéricos integrados como @System.Int32 (palabra clave de C#: `int`), se derivan en última instancia de un único tipo base, que es @System.Object (palabra clave de C#: `object`). Esta jerarquía de tipos unificada se denomina [Common Type System](../standard/common-type-system.md) (CTS). Para obtener más información sobre la herencia en C#, consulte [Herencia](https://msdn.microsoft.com/library/ms173149.aspx).  
  
-   En CTS, cada tipo se define como un *tipo de valor* o un *tipo de referencia*. Esto incluye todos los tipos personalizados de la biblioteca de clases .NET Framework y también sus propios tipos definidos por el usuario. Los tipos que se definen mediante el uso de la palabra clave [struct](https://msdn.microsoft.com/library/ah19swz4.aspx) son tipos de valor; todos los tipos numéricos integrados son **structs**. Para obtener más información sobre los tipos de valor, consulte [Structs](structs.md). Los tipos que se definen mediante el uso de la palabra clave [class](https://msdn.microsoft.com/library/0b0thckt.aspx) son tipos de referencia. Para obtener más información sobre los tipos de referencia, consulte [Classes](classes.md) (Clases). Los tipos de referencia y los tipos de valor tienen distintas reglas de tiempo de compilación y distintos comportamientos de tiempo de ejecución.
 
  
## <a name="see-also"></a>Vea también
[Estructuras](structs.md)

[Clases](classes.md)

