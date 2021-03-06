---
title: /subsystemversion (Visual Basic) | Documentos de Microsoft
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
helpviewer_keywords:
- /subsystemversion compiler option [Visual Basic]
- -subsystemversion compiler option [Visual Basic]
- subsystemversion compiler option [Visual Basic]
ms.assetid: 08be22b2-f447-4cd3-8203-120b1b920b54
caps.latest.revision: 9
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: bc9ea6a844fae7f98315e5d3557fdf306f467dd5
ms.lasthandoff: 03/13/2017

---
# <a name="subsystemversion-visual-basic"></a>/subsystemversion (Visual Basic)
Especifica la versión mínima del subsistema en el que puede ejecutar el archivo ejecutable generado, que determinan las versiones de Windows en el que puede ejecutar el archivo ejecutable. Normalmente, esta opción garantiza que el archivo ejecutable puede aprovechar las características de seguridad determinada que no están disponibles en versiones anteriores de Windows.  
  
> [!NOTE]
>  Para especificar que el subsistema de sí mismo, use la [/target](../../../csharp/language-reference/compiler-options/target-compiler-option.md) opción del compilador.  
  
## <a name="syntax"></a>Sintaxis  
  
```vb  
/subsystemversion:major.minor  
```  
  
#### <a name="parameters"></a>Parámetros  
 `major.minor`  
 La versión mínima necesaria del subsistema, como se expresa en una notación de puntos para las versiones principales y secundarias. Por ejemplo, puede especificar que una aplicación no se puede ejecutar en un sistema operativo que sea anterior a Windows 7 si establece el valor de esta opción en 6.01, como se describe en la tabla más adelante en este tema. Debe especificar los valores de `major` y `minor` como enteros.  
  
 Líder en ceros el `minor` versión no cambia la versión, pero hacer los ceros finales. Por ejemplo, 6.1 y 6.01 hacen referencia a la misma versión, pero 6.10 hace referencia a una versión diferente. Se recomienda que expresa la versión secundaria como dos dígitos para evitar confusiones.  
  
## <a name="remarks"></a>Comentarios  
 En la tabla siguiente se enumera las versiones de subsistema comunes de Windows.  
  
|Versión de Windows|Versión del subsistema|  
|---------------------|-----------------------|  
|Windows 2000|5.00|  
|Windows XP|5.01|  
|Windows Server 2003|5.02|  
|Windows Vista|6.00|  
|Windows 7|6.01|  
|Windows Server 2008|6.01|  
|[!INCLUDE[win8](../../../csharp/language-reference/compiler-options/includes/win8_md.md)]|6.02|  
  
## <a name="default-values"></a>Valores predeterminados  
 El valor predeterminado de la **/subsystemversion** opción del compilador depende de las condiciones de la lista siguiente:  
  
-   El valor predeterminado es 6.02 si se establece ninguna opción del compilador en la lista siguiente:  
  
    -   [/ target: appcontainerexe](../../../visual-basic/reference/command-line-compiler/target.md)  
  
    -   [/ target: winmdobj](../../../visual-basic/reference/command-line-compiler/target.md)  
  
    -   [/Platform:ARM](../../../visual-basic/reference/command-line-compiler/platform.md)  
  
-   El valor predeterminado es 6.00 si utiliza MSBuild, escribimos [!INCLUDE[net_v45](../../../csharp/language-reference/compiler-options/includes/net_v45_md.md)], y no ha configurado ninguna de las opciones del compilador que se especificaron anteriormente en esta lista.  
  
-   El valor predeterminado es 4.00 si ninguna de las anteriores condiciones es true.  
  
## <a name="setting-this-option"></a>Establecer esta opción  
 Para establecer el **/subsystemversion** opción del compilador en Visual Studio, debe abrir el archivo .vbproj y especifique un valor para el `SubsystemVersion` propiedad en el XML de MSBuild. No se puede establecer esta opción en el IDE de Visual Studio. Para obtener más información, vea "Valores predeterminados" anteriormente en este tema o [Common MSBuild Project Properties](https://docs.microsoft.com/visualstudio/msbuild/common-msbuild-project-properties).  
  

  
## <a name="see-also"></a>Vea también  
[Compilador de línea de comandos de Visual Basic](../../../visual-basic/reference/command-line-compiler/index.md)

[Propiedades de MSBuild](https://docs.microsoft.com/visualstudio/msbuild/msbuild-properties)

