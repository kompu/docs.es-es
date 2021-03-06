---
title: "Option Infer instrucción | Documentos de Microsoft"
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.topic: article
f1_keywords:
- vb.OptionInfer
- vb.Infer
dev_langs:
- VB
helpviewer_keywords:
- variables [Visual Basic], declaring
- Option Infer statement
- Infer keyword
- declaring variables, inferred
- inferred variable declaration
ms.assetid: 4ad3e6e9-8f5b-4209-a248-de22ef6e4652
caps.latest.revision: 72
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
ms.openlocfilehash: e6074ad44b94c80f275af562edef2dcd1173c0c3
ms.lasthandoff: 03/13/2017

---
# <a name="option-infer-statement"></a>Option Infer (instrucción)
Permite el uso de la inferencia de tipo de variable local en la declaración de variables.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
Option Infer { On | Off }  
```  
  
## <a name="parts"></a>Elementos  
  
|Término|Definición|  
|---|---|  
|`On`|Opcional. Habilita la inferencia de tipo de variable local.|  
|`Off`|Opcional. Deshabilita la inferencia de tipo de variable local.|  
  
## <a name="remarks"></a>Comentarios  
 Para establecer `Option Infer` en un archivo, escriba `Option Infer On` o `Option Infer Off` en la parte superior del archivo, antes de cualquier otro código fuente. Si el valor establecido para `Option Infer` en un archivo entra en conflicto con el valor establecido en el IDE o en la línea de comandos, el valor del archivo tiene prioridad.  
  
 Al establecer `Option Infer` en `On`, puede declarar variables locales sin especificar explícitamente un tipo de datos. El compilador deduce el tipo de datos de una variable a partir del tipo de su expresión de inicialización.  
  
 En la siguiente ilustración, `Option Infer` está activado. La variable de la declaración `Dim someVar = 2` se declara como un entero mediante la inferencia de tipo.  
  
 ![Vista IntelliSense de la declaración.](../../../visual-basic/language-reference/statements/media/optioninferasinteger.png "optionInferAsInteger")  
IntelliSense cuando Option Infer está activado  
  
 En la siguiente ilustración, `Option Infer` está desactivado. La variable de la declaración `Dim someVar = 2` se declara como un `Object` mediante la inferencia de tipo. En este ejemplo, el **Option Strict** está configurada en **desactivar** en el [página compilación, Diseñador de proyectos (Visual Basic)](https://docs.microsoft.com/visualstudio/ide/reference/compile-page-project-designer-visual-basic).  
  
 ![Vista IntelliSense de la declaración.](../../../visual-basic/language-reference/statements/media/optioninferasobject.png "optionInferAsObject")  
IntelliSense cuando Option Infer está desactivado  
  
> [!NOTE]
>  Cuando una variable se declara como un `Object`, el tipo de tiempo de ejecución puede cambiar mientras se ejecuta el programa. [!INCLUDE[vbprvb](../../../csharp/programming-guide/concepts/linq/includes/vbprvb_md.md)]realiza operaciones llamadas *boxing* y *unboxing* para convertir entre un `Object` y un tipo de valor, lo que ralentiza la ejecución. Para obtener información sobre las conversiones boxing y unboxing, vea la [especificación del lenguaje Visual Basic](../../../visual-basic/reference/language-specification.md).  
  
 La inferencia de tipo se aplica en el nivel de procedimiento, y no se aplica fuera de un procedimiento en una clase, estructura, módulo o interfaz.  
  
 Para obtener más información, consulte [inferencia de tipo Local](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md).  
  
## <a name="when-an-option-infer-statement-is-not-present"></a>Cuando la instrucción Option Infer no está presente  
 Si el código fuente no contiene una `Option Infer` instrucción, el **Option Infer** en el [página compilación, Diseñador de proyectos (Visual Basic)](https://docs.microsoft.com/visualstudio/ide/reference/compile-page-project-designer-visual-basic) se utiliza. Si se usa el compilador de línea de comandos, el [/optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md) se utiliza la opción del compilador.  
  
#### <a name="to-set-option-infer-in-the-ide"></a>Cómo establecer Option Infer en el IDE  
  
1.  En **el Explorador de soluciones**, seleccione un proyecto. En el **proyecto** menú, haga clic en **propiedades**. Para obtener más información, consulte [NIB: administrar propiedades del proyecto con el Diseñador de proyectos](http://msdn.microsoft.com/en-us/983f3c18-832f-4666-afec-74b716ff3e0e).  
  
2.  Haga clic en el **compilar** ficha.  
  
3.  Establezca el valor de la **Option infer** cuadro.  
  
 Cuando se crea un nuevo proyecto, el **Option Infer** en el **compilar** ficha se establece en el **Option Infer** en el **valores predeterminados de VB** cuadro de diálogo. Para tener acceso a la **valores predeterminados de VB** cuadro de diálogo el **herramientas** menú, haga clic en **opciones**. En el **opciones** diálogo cuadro, expanda **proyectos y soluciones**y, a continuación, haga clic en **valores predeterminados de VB**. El valor predeterminado inicial de **valores predeterminados de VB** es `On`.  
  
#### <a name="to-set-option-infer-on-the-command-line"></a>Cómo establecer Option Infer en la línea de comandos  
  
-   Incluir el [/optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md) opción del compilador en el **vbc** comando.  
  
## <a name="default-data-types-and-values"></a>Tipos de datos y valores predeterminados  
 En la tabla siguiente se describen los resultados de diversas combinaciones resultantes de especificar el tipo de datos y el inicializador en una instrucción `Dim`.  
  
|¿Tipo de datos especificado?|¿Inicializador especificado?|Ejemplo|Resultado|  
|---|---|---|---|  
|No|No|`Dim qty`|Si `Option Strict` está desactivado (valor predeterminado), la variable se establece en `Nothing`.<br /><br /> Si `Option Strict` está activado, se produce un error en tiempo de compilación.|  
|No|Sí|`Dim qty = 5`|Si `Option Infer` está activado (valor predeterminado), la variable toma el tipo de datos del inicializador. Consulte [inferencia del tipo Local](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md).<br /><br /> Si `Option Infer` está desactivado y `Option Strict` está desactivado, la variable toma el tipo de datos de `Object`.<br /><br /> Si `Option Infer` está desactivado y `Option Strict` está activado, se produce un error en tiempo de compilación.|  
|Sí|No|`Dim qty As Integer`|La variable se inicializa con el valor predeterminado del tipo de datos. Para obtener más información, consulte [Dim (instrucción)](../../../visual-basic/language-reference/statements/dim-statement.md).|  
|Sí|Sí|`Dim qty  As Integer = 5`|Si el tipo de datos del inicializador no es convertible al tipo de datos especificado, se produce un error en tiempo de compilación.|  
  
## <a name="example"></a>Ejemplo  
 Los ejemplos siguientes muestran cómo la instrucción `Option Infer` habilita la inferencia de tipo local.  
  
 [!code-vb[VbVbalrTypeInference Nº&6;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/option-infer-statement_1.vb)]  
  
## <a name="example"></a>Ejemplo  
 El siguiente ejemplo demuestra que el tipo en tiempo de ejecución puede ser diferente cuando una variable se identifica como un `Object`.  
  
 [!code-vb[VbVbalrTypeInference&#11;](../../../visual-basic/language-reference/statements/codesnippet/VisualBasic/option-infer-statement_2.vb)]  
  
## <a name="see-also"></a>Vea también  
 [Dim (instrucción)](../../../visual-basic/language-reference/statements/dim-statement.md)   
 [Inferencia de tipo local](../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)   
 [Option Compare (instrucción)](../../../visual-basic/language-reference/statements/option-compare-statement.md)   
 [Option Explicit (instrucción)](../../../visual-basic/language-reference/statements/option-explicit-statement.md)   
 [Option Strict (instrucción)](../../../visual-basic/language-reference/statements/option-strict-statement.md)   
 [Cuadro de diálogo de opciones de valores predeterminados, proyectos, Visual Basic](https://docs.microsoft.com/visualstudio/ide/reference/visual-basic-defaults-projects-options-dialog-box)   
 [/optioninfer](../../../visual-basic/reference/command-line-compiler/optioninfer.md)   
 [Conversión boxing y conversión unboxing](../../../csharp/programming-guide/types/boxing-and-unboxing.md)
