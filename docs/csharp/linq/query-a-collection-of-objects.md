---
title: "Consultar una colección de objetos"
description: "Cómo se consultan colecciones."
keywords: .NET, .NET Core, C#
author: BillWagner
manager: wpickett
ms.author: wiwagn
ms.date: 11/30/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 87a76f8a-0b58-4791-90ea-2fe0a30416c9
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 6694d28355b638c1fe55df0b44700f2dd8d839de
ms.lasthandoff: 03/13/2017

---
# <a name="query-a-collection-of-objects"></a>Consultar una colección de objetos
En este ejemplo se muestra cómo realizar una consulta simple en una lista de objetos `Student`. Cada objeto `Student` contiene información básica sobre el alumno y una lista que representa las puntuaciones del alumno en cuatro exámenes.  
  
 Esta aplicación sirve de marco en muchos otros ejemplos de esta sección que usan el mismo origen de datos `students`.  
  
## <a name="example"></a>Ejemplo  
 La consulta siguiente devuelve los alumnos que reciben una puntuación de 90 o más en su primer examen.  
  
 [!code-cs[csProgGuideLINQ#15](../../../samples/snippets/csharp/concepts/linq/how-to-query-a-collection-of-objects_1.cs)]  
  
 Esta consulta es deliberadamente simple para permitirle experimentar. Por ejemplo, puede probar otras condiciones en la cláusula `where` o usar una cláusula `orderby` para ordenar los resultados.  
  

## <a name="see-also"></a>Vea también  
 [Expresiones de consulta LINQ](index.md)
