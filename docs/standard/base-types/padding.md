---
title: Cadenas de relleno
description: Cadenas de relleno
keywords: .NET, .NET Core
author: stevehoag
ms.author: shoag
ms.date: 07/26/2016
ms.topic: article
ms.prod: .net
ms.technology: dotnet-standard
ms.devlang: dotnet
ms.assetid: 1c8b3b44-d370-49e1-90b5-64ac81c02ae91c8b3b44-d370-49e1-90b5-64ac81c02ae9
translationtype: Human Translation
ms.sourcegitcommit: 90fe68f7f3c4b46502b5d3770b1a2d57c6af748a
ms.openlocfilehash: bc3cc9028b232cc2ba6ca3130c4bdb261c4a0a42
ms.lasthandoff: 03/02/2017

---

# <a name="padding-strings"></a>Cadenas de relleno

Use uno de los siguientes métodos [System.String](xref:System.String) para crear una cadena que conste de una cadena original rellenada con caracteres iniciales o finales hasta una longitud total especificada. El carácter de relleno puede ser un espacio o un carácter especificado y, por tanto, parece que está alineado a la derecha o a la izquierda.

Nombre del método | Uso
----------- | ---
[String.PadLeft](xref:System.String.PadLeft(System.Int32)) | Rellena una cadena con caracteres iniciales hasta una longitud total especificada.
[String.PadRight](xref:System.String.PadRight(System.Int32)) | Rellena una cadena con caracteres finales hasta una longitud total especificada.

## <a name="padleft"></a>PadLeft

El método [String.PadLeft](xref:System.String.PadLeft(System.Int32)) crea una cadena mediante la concatenación de suficientes caracteres de relleno iniciales con una cadena original para alcanzar la longitud total especificada. El método [String.PadLeft(Int32)](xref:System.String.PadLeft(System.Int32)) usa el espacio en blanco como carácter de relleno y el método [String.PadLeft(Int32, Char)](xref:System.String.PadLeft(System.Int32,System.Char)) le permite especificar su propio carácter de relleno.

En el ejemplo de código siguiente se usa el método [PadLeft(Int32, Char)](xref:System.String.PadLeft(System.Int32,System.Char)) para crear una cadena con una longitud de veinte caracteres. En el ejemplo se muestra "`--------Hello World!`" en la consola.

```csharp
string MyString = "Hello World!";
Console.WriteLine(MyString.PadLeft(20, '-'));
```

```vb
Dim MyString As String = "Hello World!"
Console.WriteLine(MyString.PadLeft(20, "-"c))
```

## <a name="padright"></a>PadRight

El método [String.PadRight](xref:System.String.PadRight(System.Int32)) crea una cadena mediante la concatenación de suficientes caracteres de relleno finales con una cadena original para alcanzar la longitud total especificada. El método [String.PadRight(Int32)](xref:System.String.PadRight(System.Int32)) usa el espacio en blanco como carácter de relleno y el método [String.PadRight(Int32, Char)](xref:System.String.PadRight(System.Int32,System.Char)) le permite especificar su propio carácter de relleno.

En el ejemplo de código siguiente se usa el método [PadRight(Int32, Char)](xref:System.String.PadRight(System.Int32,System.Char)) para crear una cadena con una longitud de veinte caracteres. En el ejemplo se muestra "`Hello World!--------`" en la consola.

```csharp
string MyString = "Hello World!";
Console.WriteLine(MyString.PadRight(20, '-'));
```

```vb
Dim MyString As String = "Hello World!"
Console.WriteLine(MyString.PadRight(20, "-"c))
```

## <a name="see-also"></a>Vea también

[Operaciones básicas de cadenas](basic-string-operations.md)


