---
title: "Tipos de colección ordenados | Microsoft Docs"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- SortedDictionary collection type
- SortedList class, grouping data in collections
- grouping data in collections, SortedList collection type
- SortedList collection type
- collections [.NET Framework], SortedList collection type
ms.assetid: 3db965b2-36a6-4b12-b76e-7f074ff7275a
caps.latest.revision: 16
author: mairaw
ms.author: mairaw
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 2ac1552dba8756d033ee02651142476c4a15a485
ms.lasthandoff: 04/18/2017

---
# <a name="sorted-collection-types"></a>Tipos de colecciones ordenadas
La clase <xref:System.Collections.SortedList?displayProperty=fullName>, la clase genérica <xref:System.Collections.Generic.SortedList%602?displayProperty=fullName> y la clase genérica <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=fullName> son similares a la clase <xref:System.Collections.Hashtable> y a la clase genérica <xref:System.Collections.Generic.Dictionary%602> porque implementan la interfaz <xref:System.Collections.IDictionary>, pero mantienen sus elementos en el criterio de ordenación por clave y no tienen la característica de inserción y recuperación O(1) de las tablas hash. Las tres clases tienen varias características en común:  
  
-   Las tres clases implementan la interfaz <xref:System.Collections.IDictionary?displayProperty=fullName>. Las dos clases genéricas también implementan la interfaz genérica <xref:System.Collections.Generic.IDictionary%602?displayProperty=fullName>.  
  
-   Cada elemento es un par de clave y valor para propósitos de enumeración.  
  
    > [!NOTE]
    >  La clase no genérica <xref:System.Collections.SortedList> devuelve objetos <xref:System.Collections.DictionaryEntry> cuando se enumeran, aunque los dos tipos genéricos devuelven objetos <xref:System.Collections.Generic.KeyValuePair%602>.  
  
-   Los elementos se ordenan de acuerdo con una implementación <xref:System.Collections.IComparer?displayProperty=fullName> (para <xref:System.Collections.SortedList> no genérico) o una implementación <xref:System.Collections.Generic.IComparer%601?displayProperty=fullName> (para las dos clases genéricas).  
  
-   Cada clase proporciona propiedades que devuelven colecciones que contienen solo las claves o solo los valores.  
  
 En la tabla siguiente se enumeran algunas de las diferencias entre las dos clases de lista ordenada y la clase <xref:System.Collections.Generic.SortedDictionary%602>.  
  
|Clase no genérica <xref:System.Collections.SortedList> y clase genérica <xref:System.Collections.Generic.SortedList%602>|Clase genérica <xref:System.Collections.Generic.SortedDictionary%602>|  
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|Las propiedades que devuelven claves y valores se indizan, lo que permite una recuperación indizada eficaz.|Sin recuperación indizada.|  
|La recuperación es O(log `n`).|La recuperación es O(log `n`).|  
|La inserción y eliminación son generalmente O(`n`); pero la inserción es O(1) para los datos que ya están en el criterio de ordenación, de manera que cada elemento se agrega al final de la lista. (Se supone que no es necesario cambiar de tamaño).|La inserción y eliminación son O(log `n`).|  
|Utiliza menos memoria que <xref:System.Collections.Generic.SortedDictionary%602>.|Utiliza más memoria que la clase no genérica <xref:System.Collections.SortedList> y la clase genérica <xref:System.Collections.Generic.SortedList%602>.|  
  
 Para listas ordenadas o diccionarios que deben ser accesibles simultáneamente desde varios subprocesos, se puede agregar una lógica de ordenación a una clase que deriva de <xref:System.Collections.Concurrent.ConcurrentDictionary%602>.  
  
> [!NOTE]
>  Para los valores que contienen sus propias claves (por ejemplo, registros de empleados que contienen un número de id. de empleado), puede derivar de la clase genérica <xref:System.Collections.ObjectModel.KeyedCollection%602> para crear una colección con clave que tenga algunas características de lista y algunas características de diccionario.  
  
 A partir de [!INCLUDE[net_v40_long](../../../includes/net-v40-long-md.md)], la clase <xref:System.Collections.Generic.SortedSet%601> proporciona un árbol que mantiene los datos ordenados después de las inserciones, eliminaciones y búsquedas. Esta clase y la clase <xref:System.Collections.Generic.HashSet%601> implementan la interfaz <xref:System.Collections.Generic.ISet%601>.  
  
## <a name="see-also"></a>Vea también  
 <xref:System.Collections.IDictionary?displayProperty=fullName>   
 <xref:System.Collections.Generic.IDictionary%602?displayProperty=fullName>   
 <xref:System.Collections.Concurrent.ConcurrentDictionary%602>   
 [Tipos de colección utilizados normalmente](../../../docs/standard/collections/commonly-used-collection-types.md)
