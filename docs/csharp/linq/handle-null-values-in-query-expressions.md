---
title: Controlar valores nulos en expresiones de consulta
description: "Cómo se controlan valores nulos en expresiones de consulta."
keywords: .NET, .NET Core, C#
author: BillWagner
manager: wpickett
ms.author: wiwagn
ms.date: 12/1/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: ac63ae8b-724d-4251-9334-528f4e884ae7
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 14b64bf8d3590f4f7dc3d1b00cb50d0bc421d9bc
ms.lasthandoff: 03/13/2017

---
# <a name="handle-null-values-in-query-expressions"></a>Controlar valores nulos en expresiones de consulta

En este ejemplo se muestra cómo controlar los posibles valores nulos en colecciones de origen. Una colección de objetos como <xref:System.Collections.Generic.IEnumerable%601> puede contener elementos cuyo valor sea [null](../language-reference/keywords/null.md). Si una colección de origen es nula o contiene un elemento cuyo valor es null, y la consulta no controla los valores nulos, se iniciará una <xref:System.NullReferenceException> cuando se ejecute la consulta.  
  
## <a name="example"></a>Ejemplo

 Se pueden codificar de forma defensiva para evitar una excepción de referencia nula, tal y como se muestra en el ejemplo siguiente:  
  
 [!code-cs[csProgGuideLINQ#82](../../../samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_1.cs)]  
  
 En el ejemplo anterior, la cláusula `where` filtra todos los elementos nulos de la secuencia de categorías. Esta técnica es independiente de la comprobación de null en la cláusula join. La expresión condicional con null de este ejemplo funciona porque `Products.CategoryID` es de tipo `int?`, que es una abreviatura de `Nullable<int>`.  
  
## <a name="example"></a>Ejemplo

 En una cláusula join, si solo una de las claves de comparación es un tipo que acepta valores NULL, puede convertir la otra en un tipo que acepta valores NULL en la expresión de consulta. En el ejemplo siguiente, suponga que `EmployeeID` es una columna que contiene valores de tipo `int?`:  
  
 [!code-cs[csProgGuideLINQ#83](../../../samples/snippets/csharp/concepts/linq/how-to-handle-null-values-in-query-expressions_2.cs)]  
  
## <a name="see-also"></a>Vea también  
 <xref:System.Nullable%601>   
 [Expresiones de consulta LINQ](index.md)   
 [Nullable types](../programming-guide/nullable-types/index.md) (Tipos que aceptan valores NULL [Guía de programación de C#])
