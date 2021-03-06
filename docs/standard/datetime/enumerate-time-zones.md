---
title: "Cómo: Enumerar zonas horarias presentes en un equipo"
description: "Cómo enumerar zonas horarias presentes en un equipo"
keywords: .NET, .NET Core
author: stevehoag
ms.author: shoag
ms.date: 08/15/2016
ms.topic: article
ms.prod: .net
ms.technology: dotnet-standard
ms.devlang: dotnet
ms.assetid: c5ae4a6c-1790-4355-b5b1-879aaf956129
translationtype: Human Translation
ms.sourcegitcommit: 90fe68f7f3c4b46502b5d3770b1a2d57c6af748a
ms.openlocfilehash: f30ba2a483ff7e5867417969946c2774175d5e3d
ms.lasthandoff: 03/02/2017

---

# <a name="how-to-enumerate-time-zones-present-on-a-computer"></a>Cómo: Enumerar zonas horarias presentes en un equipo

Para trabajar correctamente con una zona horaria designada, es necesario que la información sobre esa zona horaria esté a disposición del sistema. Por ejemplo, el sistema operativo Windows almacena esta información en el Registro. Pero aunque el número total de zonas horarias que existe en el mundo es elevado, el Registro contiene información sobre solo un subconjunto de ellas. Además, el propio Registro es una estructura dinámica cuyo contenido está sujeto a cambios intencionados y accidentales. Como resultado, una aplicación no siempre puede suponer que una zona horaria determinada esté definida y disponible en un sistema. El primer paso para muchas aplicaciones que usan aplicaciones de información de zona horaria es determinar si las zonas horarias necesarias están disponibles en el sistema local o proporcionar al usuario una lista de zonas horarias entre las que seleccionar. Esto exige que una aplicación enumere las zonas horarias definidas en un sistema local. 

## <a name="to-enumerate-the-time-zones-present-on-the-local-system"></a>Para enumerar las zonas horarias presentes en el sistema local

1. Llame al método [TimeZoneInfo.GetSystemTimeZones](xref:System.TimeZoneInfo.GetSystemTimeZones). El método devuelve una colección genérica [ReadOnlyCollection&lt;T&gt;](xref:System.Collections.ObjectModel.ReadOnlyCollection%601) de objetos [TimeZoneInfo](xref:System.TimeZoneInfo). Las entradas de la colección se ordenan por su propiedad [DisplayName](xref:System.TimeZoneInfo.DisplayName). Por ejemplo:

    ```csharp
    ReadOnlyCollection<TimeZoneInfo> tzCollection;
    tzCollection = TimeZoneInfo.GetSystemTimeZones();
    ```

    ```vb
    Dim tzCollection As ReadOnlyCollection(Of TimeZoneInfo) = TimeZoneInfo.GetSystemTimeZones
    ```

2. Enumere los objetos individuales [TimeZoneInfo](xref:System.TimeZoneInfo) de la colección con un bucle `foreach` (en C#) o un bucle `For Each…Next` (en Visual Basic) y realice cualquier procesamiento necesario en cada objeto. Por ejemplo, el código siguiente enumera la colección [ReadOnlyCollection&lt;T&gt;](xref:System.Collections.ObjectModel.ReadOnlyCollection%601) de objetos [TimeZoneInfo](xref:System.TimeZoneInfo) devuelta en el paso 1 y enumera el nombre para mostrar de cada zona horaria en la consola.

    ```csharp
    foreach (TimeZoneInfo timeZone in tzCollection)
    Console.WriteLine("   {0}: {1}", timeZone.Id, timeZone.DisplayName);
    ```

    ```vb
    For Each timeZone As TimeZoneInfo In tzCollection
        Console.WriteLine("   {0}: {1}", timeZone.Id, timeZone.DisplayName)
    Next
    ```

## <a name="see-also"></a>Vea también

[Fechas, horas y zonas horarias](index.md)

[Buscar las zonas horarias definidas en un sistema local](finding-the-time-zones-on-local-system.md)


