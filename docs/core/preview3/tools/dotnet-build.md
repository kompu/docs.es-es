---
title: Comando dotnet-build | SDK de .NET Core
description: El comando dotnet-build compila un proyecto y todas sus dependencias.
keywords: dotnet-build, CLI, comando de CLI, .NET Core
author: mairaw
manager: wpickett
ms.date: 10/13/2016
ms.topic: article
ms.prod: .net-core
ms.technology: .net-core-technologies
ms.devlang: dotnet
ms.assetid: 70285a83-4103-4617-be8b-d0e1e9a4a91d
translationtype: Human Translation
ms.sourcegitcommit: 1a84c694945fe0c77468eb77274ab46618bccae6
ms.openlocfilehash: 04c4d77b31bf4a1bdb425d837c490440e7312e57

---

#<a name="dotnet-build"></a>dotnet-build

## <a name="name"></a>Nombre 
dotnet-build: compila un proyecto y todas sus dependencias 

## <a name="synopsis"></a>Sinopsis

`dotnet build [--help] [--output]  [--framework]  
    [--configuration]  [--runtime] [--version-suffix]
    [--build-profile]  [--no-incremental] [--no-dependencies]
    [<project>]`

## <a name="description"></a>Descripción

El comando `dotnet build` compila varios archivos de origen de un proyecto de origen y sus dependencias en un archivo binario. De forma predeterminada, el archivo binario resultante está en lenguaje intermedio (IL) y tiene una extensión DLL. 
`dotnet build`También quita un archivo `\*.deps` que describe lo que necesita el host para ejecutar la aplicación.  

La compilación requiere la existencia de un archivo de recursos (un archivo que enumere todas las dependencias de la aplicación), lo que significa que tiene que ejecutar [`dotnet restore`](dotnet-restore.md) antes de compilar el código.

Antes de comenzar cualquier compilación, el verbo `build` analiza el proyecto y sus dependencias para realizar las comprobaciones de seguridad incrementales.
Si todas las comprobaciones son correctas, la compilación continúa con la compilación incremental del proyecto y sus dependencias; de lo contrario, retrocede a la compilación no incremental. Mediante una marca de perfil, los usuarios pueden elegir recibir información adicional sobre cómo pueden mejorar los tiempos de compilación.

Para compilar una aplicación ejecutable en lugar de una biblioteca, debe establecer la propiedad `<OutputType>`:

```xml
<PropertyGroup>
    <OutputType>Exe</OutputType>
</PropertyGroup>
```

## <a name="options"></a>Opciones

`-h|--help`

Imprime una corta ayuda para el comando.  

`-o|--output <OUTPUT_DIRECTORY>`

Directorio donde se colocan los archivos binarios compilados. También debe definir `--framework` cuando se especifica esta opción.

`-f|--framework <FRAMEWORK>`

Compila para un marco específico. El marco debe definirse en el [archivo de proyecto](csproj.md).

`-c|--configuration [Debug|Release]`

Define una configuración con la que se va a realizar la compilación.  Si se omite, se adopta el valor predeterminado de `Debug`.

`-r|--runtime [RUNTIME_IDENTIFIER]`

Tiempo de ejecución de destino con el que realizar la compilación. Para obtener una lista de identificadores de tiempo de ejecución (RID) que puede usar, consulte el [catálogo de RID](../../rid-catalog.md). 

`--version-suffix [VERSION_SUFFIX]`

Define qué `*` debe reemplazarse por el campo de versión en el archivo del proyecto. El formato sigue las instrucciones de versión de NuGet. 

`--build-profile`

Imprime las comprobaciones de seguridad incrementales que los usuarios necesitan para que la compilación incremental se active automáticamente.

`--no-incremental`

Marca la compilación como no segura para la compilación incremental. Esto desactiva la compilación incremental y fuerza una recompilación limpia del gráfico de dependencias del proyecto.

`--no-dependencies`

Omite las referencias de proyecto a proyecto y solo compila el proyecto raíz especificado para compilar.

## <a name="examples"></a>Ejemplos

Creación de un proyecto y sus dependencias:

`dotnet build`

Creación de un proyecto y sus dependencias mediante la configuración de lanzamiento:

`dotnet build --configuration Release`

Compilación de un proyecto y sus dependencias para un tiempo de ejecución específico (en este ejemplo, Ubuntu 16.04):

`dotnet build --runtime ubuntu.16.04-x64`



<!--HONumber=Nov16_HO3-->

