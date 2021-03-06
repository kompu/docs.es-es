---
title: "Introducción a .NET Core en macOS con Visual Studio para Mac | Microsoft Docs"
description: "Este tema le guía en la creación de una aplicación de consola sencilla con Visual Studio para Mac y .NET Core."
keywords: .NET, .NET Core, macOS, Mac
author: guardrex
ms.author: mairaw
ms.date: 03/16/2017
ms.topic: article
ms.prod: .net-core
ms.devlang: dotnet
ms.assetid: 8902e849-dd17-42c0-8264-cc7ae3927a0c
translationtype: Human Translation
ms.sourcegitcommit: ff143583ba62fc1d82561e739a75107e50ebee88
ms.openlocfilehash: ed8787d72647f18544bde8ed721cf37f31fbb5fe
ms.lasthandoff: 03/20/2017

---

# <a name="getting-started-with-net-core-on-macos-using-visual-studio-for-mac"></a>Introducción a .NET Core en macOS con Visual Studio para Mac

Visual Studio para Mac proporciona un entorno de desarrollo integrado (IDE) completo para el desarrollo de aplicaciones .NET Core. Este tema le guía en la creación de una aplicación de consola sencilla con Visual Studio para Mac y .NET Core.

> [!NOTE]
> Visual Studio para Mac es un software de versión preliminar. Como con todas las versiones preliminares de productos de Microsoft, sus comentarios son muy valiosos. Hay dos maneras de proporcionar comentarios al equipo de desarrollo de Visual Studio para Mac:
> * En Visual Studio para Mac, seleccione **Ayuda > Notificar un problema** en el menú o **Notificar un problema** desde la pantalla de bienvenida, que abre una ventana para presentar un informe de errores.
> * Para hacer una sugerencia, seleccione **Ayuda > Aportar una sugerencia** en el menú o **Aportar una sugerencia** desde la pantalla de bienvenida, que le lleva a la [página web de User Voice de Visual Studio para Mac](https://visualstudio.uservoice.com/forums/563332-visual-studio-for-mac).

## <a name="prerequisites"></a>Requisitos previos

[.NET Core y OpenSSL](https://www.microsoft.com/net/core#macos)

Para más información sobre los requisitos previos, consulte los [requisitos previos para .NET Core en Mac](../../core/macos-prerequisites.md).

## <a name="getting-started"></a>Introducción

Si ya ha instalado los requisitos previos y Visual Studio para Mac, omita esta sección y proceda con [Creación de un proyecto](#creating-a-project). Siga estos pasos para instalar los requisitos previos y Visual Studio para Mac:

1. Descargue e instale [.NET Core y OpenSSL](https://www.microsoft.com/net/core#macos).

1. Descargue el [instalador de Visual Studio para Mac](https://www.visualstudio.com/vs/visual-studio-mac/). Ejecute el instalador. Lea y acepte el contrato de licencia. Durante la instalación, se les proporciona la oportunidad de instalar Xamarin, una tecnología de desarrollo de aplicaciones móviles multiplataforma. La instalación de Xamarin y sus componentes relacionados es opcional para el desarrollo de .NET Core. Para ver un tutorial del proceso de instalación de Visual Studio para Mac, consulte [Introducing Visual Studio for Mac](https://developer.xamarin.com/guides/cross-platform/visual-studio-mac/) (Introducción a Visual Studio para Mac). Una vez completada la instalación, inicie el IDE de Visual Studio para Mac.

## <a name="creating-a-project"></a>Creación de un proyecto

1. Seleccione **Nuevo proyecto** en la pantalla de bienvenida.

   ![Botón Nuevo proyecto en la pantalla de bienvenida de Visual Studio para Mac](./media/using-on-mac-vs/vsmac1.png)

1. En el cuadro de diálogo **Nuevo proyecto**, seleccione **Aplicación** en el nodo **.NET Core**. Seleccione la plantilla **Aplicación de consola** y haga clic en **Siguiente**.

   ![Lista de plantillas de Nuevo proyecto](./media/using-on-mac-vs/vsmac2.png)

1. Escriba "HelloWorld" en **Nombre de proyecto**. Seleccione **Crear**.

   ![Configuración del cuadro de diálogo de nueva aplicación de consola](./media/using-on-mac-vs/vsmac3.png)

1. Espere mientras se restauran las dependencias del proyecto. El proyecto tiene un único archivo de C#, *Program.cs*, que contiene una clase `Program` con un método `Main`. La instrucción `Console.WriteLine` genera la salida "¡Hola a todos!" en la consola cuando se ejecuta la aplicación.

   ![Ventana principal con el archivo Program.cs abierto](./media/using-on-mac-vs/vsmac4.png)

## <a name="run-the-application"></a>Ejecutar la aplicación

Ejecute la aplicación en modo de depuración mediante <kbd>F5</kbd> o en modo de versión mediante <kbd>CTRL</kbd>+<kbd>F5</kbd>.

![El panel de salida de la aplicación muestra ¡Hola a todos!](./media/using-on-mac-vs/vsmac5.png)

## <a name="next-step"></a>Paso siguiente

El tema [Creación de una solución completa de .NET Core en macOS con Visual Studio para Mac](using-on-mac-vs-full-solution.md) le muestra cómo crear una solución completa de .NET Core que incluye una biblioteca reutilizable y pruebas unitarias.

