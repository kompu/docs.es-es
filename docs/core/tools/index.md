---
title: "Herramientas de la interfaz de línea de comandos (CLI) de .NET Core | Microsoft Docs"
description: "Introducción a las herramientas y características de la interfaz de la línea de comandos (CLI)."
keywords: CLI, herramientas de la CLI, .NET, .NET Core
author: blackdwarf
ms.author: mairaw
ms.date: 03/20/2017
ms.topic: article
ms.prod: .net-core
ms.technology: dotnet-cli
ms.devlang: dotnet
ms.assetid: 7c5eee9f-d873-4224-8f5f-ed83df329a59
translationtype: Human Translation
ms.sourcegitcommit: d97a1501ad25b683cbb5d7fbd8bd1b137f7f4046
ms.openlocfilehash: 978dd62d655d0168b5a9c1c9732bc69ca9b256eb
ms.lasthandoff: 04/10/2017

---

# <a name="net-core-command-line-interface-cli-tools"></a>Herramientas de la interfaz de la línea de comandos (CLI) de .NET Core

La interfaz de la línea de comandos (CLI) de .NET Core es una nueva cadena de herramientas multiplataforma para el desarrollo de aplicaciones. NET. La CLI es una base sobre la que pueden descansar herramientas de nivel superior, como los entornos de desarrollo integrados (IDE), los editores y los orquestadores de compilación.

## <a name="installation"></a>Instalación

Puede usar los instaladores nativos o los scripts de shell de instalación:

* Los instaladores nativos se usan principalmente en máquinas del desarrollador y usan el mecanismo de instalación nativo de cada plataforma compatible, por ejemplo, los paquetes DEB en Ubuntu o los paquetes MSI en Windows. Estos instaladores instalan y configuran el entorno para su uso inmediato por el desarrollador, pero requieren privilegios administrativos en la máquina. Puede ver las instrucciones de instalación en la [Guía de instalación de .NET Core](https://aka.ms/dotnetcoregs).
* Los scripts de shell se usan principalmente para configurar servidores de compilación o cuando desee instalar las herramientas sin privilegios administrativos. Los scripts de instalación no instalan los requisitos previos en la máquina, se deben instalar manualmente. Para más información, consulte el [tema de referencia sobre los scripts de instalación](dotnet-install-script.md). Para más información sobre cómo configurar la CLI en el servidor de compilación de integración continua (CI), consulte [Uso de .NET Core SDK y herramientas de integración continua (CI)](using-ci-with-cli.md).

De forma predeterminada, la CLI se instala en paralelo (SxS), para que varias versiones de las herramientas de la CLI puedan coexistir en una única máquina. En la sección [Controlador](#driver) se explica más detalladamente cómo determinar qué versión usar en una máquina en la que hay varias versiones instaladas.

## <a name="cli-commands"></a>Comandos de la CLI

De forma predeterminada, se instalan los siguientes comandos:

### <a name="basic-commands"></a>Comandos básicos

* [new](dotnet-new.md)
* [restore](dotnet-restore.md)
* [build](dotnet-build.md)
* [publish](dotnet-publish.md)
* [run](dotnet-run.md)
* [test](dotnet-test.md)
* [vstest](dotnet-vstest.md)
* [pack](dotnet-pack.md)
* [migrate](dotnet-migrate.md)
* [clean](dotnet-clean.md)
* [sln](dotnet-sln.md)

### <a name="project-modification-commands"></a>Comandos de modificación del proyecto

* [add package](dotnet-add-package.md)
* [add reference](dotnet-add-reference.md)
* [remove package](dotnet-remove-package.md)
* [remove reference](dotnet-remove-reference.md)
* [list reference](dotnet-list-reference.md)

### <a name="advanced-commands"></a>Comandos avanzados

* [nuget delete](dotnet-nuget-delete.md)
* [nuget locals](dotnet-nuget-locals.md)
* [nuget push](dotnet-nuget-push.md)
* [msbuild](dotnet-msbuild.md)
* [dotnet install script](dotnet-install-script.md)

La CLI adopta un modelo de extensibilidad que le permite especificar herramientas adicionales para sus proyectos. Para más información, consulte el tema [Modelo de extensibilidad de la CLI de .NET Core](extensibility.md).

## <a name="command-structure"></a>Estructura de comandos

La estructura de comandos de la CLI consta del [controlador ("dotnet")](#driver), [el comando (o "verbo")](#command-verb) y posiblemente de los [argumentos de comandos](#arguments) y otras [opciones](#options). Este patrón se puede ver en la mayoría de las operaciones de la CLI, como la creación de una nueva aplicación de consola y su ejecución desde la línea de comandos, como muestran los siguientes comandos cuando se ejecutan desde un directorio denominado *my_app*:

```console
dotnet new console
dotnet restore
dotnet build --output /build_output
dotnet /build_output/my_app.dll
```

### <a name="driver"></a>Controlador

El controlador se denomina [dotnet](dotnet.md) y tiene dos responsabilidades, ejecutar una [aplicación dependiente del marco](../deploying/index.md) o ejecutar un comando. La única vez que se usa `dotnet` sin un comando es cuando se usa para iniciar una aplicación.

Para ejecutar una aplicación dependiente del marco, especifique la aplicación después del controlador, por ejemplo, `dotnet /path/to/my_app.dll`. Cuando ejecute el comando desde la carpeta donde reside la DLL de la aplicación, simplemente ejecute `dotnet my_app.dll`.

Cuando se proporciona un comando para el controlador, `dotnet.exe` inicia el proceso de ejecución del comando de la CLI. En primer lugar, el controlador determina la versión de las herramientas que quiere usar. Si no se especifica la versión en las opciones de comando, el controlador usa la versión más reciente que esté disponible. Para especificar una versión distinta de la última versión instalada, use la opción `--fx-version <VERSION>` (consulte la referencia del [comando dotnet)](dotnet.md). Una vez determinada la versión del SDK, el controlador ejecuta el comando.

### <a name="command-verb"></a>Comando ("verbo")

El comando (o "verbo") es simplemente un comando que realiza una acción. Por ejemplo, `dotnet build` compila el código. `dotnet publish` publica el código. Los comandos se implementan como una aplicación de consola usando una convención `dotnet-{verb}`. 

### <a name="arguments"></a>Argumentos

Los argumentos que se pasan en la línea de comandos son los argumentos para el comando invocado. Por ejemplo, cuando ejecuta `dotnet publish my_app.csproj`, el argumento `my_app.csproj` indica el proyecto que se publicará y se pasará al comando `publish`.

### <a name="options"></a>Opciones

Las opciones que se pasan en la línea de comandos son las opciones para el comando que se invoca. Por ejemplo, cuando ejecuta `dotnet publish --output /build_output`, la opción `--output` y su valor se pasan al comando `publish`. 

## <a name="migration-from-projectjson"></a>Migración desde project.json

Si usó herramientas de la versión preliminar 2 para producir proyectos basados en *project.json*, consulte el tema sobre [dotnet migrate](dotnet-migrate.md) para más información sobre cómo migrar su proyecto a MSBuild/*.csproj* para usarlo con herramientas de versión. Para los proyectos de .NET Core creados antes del lanzamiento de las herramientas de versión preliminar 2, actualice manualmente el proyecto siguiendo las instrucciones que se indican en [Migración de DNX a CLI de .NET Core (project.json)](../migration/from-dnx.md) y luego use `dotnet migrate` o actualice directamente los proyectos.

## <a name="additional-resources"></a>Recursos adicionales

* [dotnet/repositorio de GitHub de la CLI](https://github.com/dotnet/cli/)
* [.NET Core installation guide](https://aka.ms/dotnetcoregs) (Guía de instalación de .NET Core)

