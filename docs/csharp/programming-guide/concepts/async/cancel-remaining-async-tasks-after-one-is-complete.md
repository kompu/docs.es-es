---
title: "Cancelar las tareas asincrónicas restantes cuando se completa una (C#) | Microsoft Docs"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: d3cebc74-c392-497b-b1e6-62a262eabe05
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 3434dc4b13295101970fd4aadb69d56ddbca7142
ms.lasthandoff: 03/13/2017

---
# <a name="cancel-remaining-async-tasks-after-one-is-complete-c"></a>Cancelar las tareas asincrónicas restantes cuando se completa una (C#)
Mediante el método <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=fullName> junto con un <xref:System.Threading.CancellationToken>, se pueden cancelar todas las tareas restantes cuando se completa una tarea. El método `WhenAny` toma un argumento que es una colección de tareas. El método inicia todas las tareas y devuelve una sola tarea. La tarea se completa cuando se complete cualquier tarea de la colección.  
  
 En este ejemplo se muestra cómo usar un token de cancelación junto con `WhenAny` para retener la primera tarea para finalizar de la colección de tareas y cancelar las tareas restantes. Cada tarea descarga el contenido de un sitio web. En el ejemplo se muestra la longitud del contenido de la primera descarga completa y se cancelan las otras descargas.  
  
> [!NOTE]
>  Para ejecutar los ejemplos, debe tener Visual Studio 2012 o posterior, y .NET Framework 4.5 o posterior, instalado en el equipo.  
  
## <a name="downloading-the-example"></a>Descargar el ejemplo  
 Puede descargar los proyectos completos de Windows Presentation Foundation (WPF) de [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación) y después seguir estos pasos.  
  
1.  Descomprima el archivo descargado y, a continuación, inicie Visual Studio.  
  
2.  En la barra de menús, elija **Archivo**, **Abrir**, **Proyecto o solución**.  
  
3.  En el cuadro de diálogo **Abrir proyecto**, abra la carpeta que contiene el código de ejemplo que descomprimió y, a después, abra el archivo de solución (.sln) para AsyncFineTuningCS.  
  
4.  En el **Explorador de soluciones**, abra el menú contextual del proyecto **CancelAfterOneTask** y, después, elija **Establecer como proyecto de inicio**.  
  
5.  Pulse la tecla F5 para ejecutar el proyecto.  
  
     Presione las teclas Ctrl+F5 para ejecutar el proyecto sin depurarlo.  
  
6.  Ejecute el programa varias veces para comprobar que finalizan primero descargas diferentes.  
  
 Si no quiere descargar el proyecto, puede revisar el archivo MainWindow.xaml.cs al final de este tema.  
  
## <a name="building-the-example"></a>Compilar el ejemplo  
 El ejemplo de este tema amplía el proyecto que se desarrolló en [Cancel an Async Task or a List of Tasks (C#)](../../../../csharp/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md) (Cancelar una tarea asincrónica o una lista de tareas [C#]) para cancelar una lista de tareas. El ejemplo usa la misma interfaz de usuario, aunque el botón **Cancelar** no se usa explícitamente.  
  
 Para generar su propio ejemplo, paso a paso, siga las instrucciones de la sección "Descargar el ejemplo", pero elija **CancelAListOfTasks** como el **Proyecto de inicio**. Agregue los cambios de este tema a ese proyecto.  
  
 En el archivo MainWindow.xaml.cs del proyecto **CancelAListOfTasks**, inicie la transición moviendo los pasos de procesamiento para cada sitio web desde el bucle en `AccessTheWebAsync` al siguiente método asincrónico.  
  
```cs  
/ ***Bundle the processing steps for a website into one async method.  
async Task<int> ProcessURLAsync(string url, HttpClient client, CancellationToken ct)  
{  
    // GetAsync returns a Task<HttpResponseMessage>.   
    HttpResponseMessage response = await client.GetAsync(url, ct);  
  
    // Retrieve the website contents from the HttpResponseMessage.  
    byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
    return urlContents.Length;  
}  
```  
  
 En `AccessTheWebAsync`, este ejemplo usa una consulta, el método <xref:System.Linq.Enumerable.ToArray%2A> y el método `WhenAny` para crear e iniciar una matriz de tareas. La aplicación de `WhenAny` a la matriz devuelve una única tarea que, cuando se espera, se evalúa como la primera tarea que llega a la finalización de la matriz de tareas.  
  
 Realice los siguientes cambios en `AccessTheWebAsync`. Los asteriscos marcan los cambios en el archivo de código.  
  
1.  Convierta en comentario o elimine el bucle.  
  
2.  Cree una consulta que, cuando se ejecute, genere una colección de tareas genéricas. Cada llamada a `ProcessURLAsync` devuelve una <xref:System.Threading.Tasks.Task%601> donde `TResult` es un entero.  
  
<CodeContentPlaceHolder>1</CodeContentPlaceHolder>  
3.  Llame a `ToArray` para ejecutar la consulta e iniciar las tareas. La aplicación del método `WhenAny` en el paso siguiente ejecutaría la consulta e iniciaría las tareas sin usar `ToArray`, pero es posible que otros métodos no lo hagan. La práctica más segura es forzar explícitamente la ejecución de la consulta.  
  
<CodeContentPlaceHolder>2</CodeContentPlaceHolder>  
4.  Llame a `WhenAny` en la colección de tareas. `WhenAny` devuelve una `Task(Of Task(Of Integer))` o `Task<Task<int>>`.  Es decir, `WhenAny` devuelve una tarea que se evalúa como una única `Task(Of Integer)` o `Task<int>` cuando se espera. Esa única tarea es la primera tarea de la colección en finalizar. La tarea que finalizó primero se asigna a `firstFinishedTask`. El tipo de `firstFinishedTask` es <xref:System.Threading.Tasks.Task%601>, donde `TResult` es un entero, ya que es el tipo de valor devuelto de `ProcessURLAsync`.  
  
    ```cs  
    // ***Call WhenAny and then await the result. The task that finishes   
    // first is assigned to firstFinishedTask.  
    Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);  
    ```  
  
5.  En este ejemplo, solo le interesa la tarea que finaliza primero. Por tanto, use <xref:System.Threading.CancellationTokenSource.Cancel%2A?displayProperty=fullName> para cancelar las tareas restantes.  
  
    ```cs  
    // ***Cancel the rest of the downloads. You just want the first one.  
    cts.Cancel();  
    ```  
  
6.  Por último, espere a `firstFinishedTask` para recuperar la longitud del contenido descargado.  
  
    ```cs  
    var length = await firstFinishedTask;  
    resultsTextBox.Text += String.Format("\r\nLength of the downloaded website:  {0}\r\n", length);  
    ```  
  
 Ejecute el programa varias veces para comprobar que finalizan primero descargas diferentes.  
  
## <a name="complete-example"></a>Ejemplo completo  
 El código siguiente es el archivo MainWindow.xaml.cs completo para el ejemplo. Los asteriscos marcan los elementos que se agregaron para este ejemplo.  
  
 Observe que tiene que agregar una referencia para <xref:System.Net.Http>.  
  
 Puede descargar el proyecto de [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación).  
  
```cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http.  
using System.Net.Http;  
  
// Add the following using directive.  
using System.Threading;  
  
namespace CancelAfterOneTask  
{  
    public partial class MainWindow : Window  
    {  
        // Declare a System.Threading.CancellationTokenSource.  
        CancellationTokenSource cts;  
  
        public MainWindow()  
        {  
            InitializeComponent();  
        }  
  
        private async void startButton_Click(object sender, RoutedEventArgs e)  
        {  
            // Instantiate the CancellationTokenSource.  
            cts = new CancellationTokenSource();  
  
            resultsTextBox.Clear();  
  
            try  
            {  
                await AccessTheWebAsync(cts.Token);  
                resultsTextBox.Text += "\r\nDownload complete.";  
            }  
            catch (OperationCanceledException)  
            {  
                resultsTextBox.Text += "\r\nDownload canceled.";  
            }  
            catch (Exception)  
            {  
                resultsTextBox.Text += "\r\nDownload failed.";  
            }  
  
            // Set the CancellationTokenSource to null when the download is complete.  
            cts = null;  
        }  
  
        // You can still include a Cancel button if you want to.  
        private void cancelButton_Click(object sender, RoutedEventArgs e)  
        {  
            if (cts != null)  
            {  
                cts.Cancel();  
            }  
        }  
  
        // Provide a parameter for the CancellationToken.  
        async Task AccessTheWebAsync(CancellationToken ct)  
        {  
            HttpClient client = new HttpClient();  
  
            // Call SetUpURLList to make a list of web addresses.  
            List<string> urlList = SetUpURLList();  
  
            // ***Comment out or delete the loop.  
            //foreach (var url in urlList)  
            //{  
            //    // GetAsync returns a Task<HttpResponseMessage>.   
            //    // Argument ct carries the message if the Cancel button is chosen.   
            //    // ***Note that the Cancel button can cancel all remaining downloads.  
            //    HttpResponseMessage response = await client.GetAsync(url, ct);  
  
            //    // Retrieve the website contents from the HttpResponseMessage.  
            //    byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
            //    resultsTextBox.Text +=  
            //        String.Format("\r\nLength of the downloaded string: {0}.\r\n", urlContents.Length);  
            //}  
  
            // ***Create a query that, when executed, returns a collection of tasks.  
            IEnumerable<Task<int>> downloadTasksQuery =  
                from url in urlList select ProcessURLAsync(url, client, ct);  
  
            // ***Use ToArray to execute the query and start the download tasks.   
            Task<int>[] downloadTasks = downloadTasksQuery.ToArray();  
  
            // ***Call WhenAny and then await the result. The task that finishes   
            // first is assigned to firstFinishedTask.  
            Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);  
  
            // ***Cancel the rest of the downloads. You just want the first one.  
            cts.Cancel();  
  
            // ***Await the first completed task and display the results.   
            // Run the program several times to demonstrate that different  
            // websites can finish first.  
            var length = await firstFinishedTask;  
            resultsTextBox.Text += String.Format("\r\nLength of the downloaded website:  {0}\r\n", length);  
        }  
  
        // ***Bundle the processing steps for a website into one async method.  
        async Task<int> ProcessURLAsync(string url, HttpClient client, CancellationToken ct)  
        {  
            // GetAsync returns a Task<HttpResponseMessage>.   
            HttpResponseMessage response = await client.GetAsync(url, ct);  
  
            // Retrieve the website contents from the HttpResponseMessage.  
            byte[] urlContents = await response.Content.ReadAsByteArrayAsync();  
  
            return urlContents.Length;  
        }  
  
        // Add a method that creates a list of web addresses.  
        private List<string> SetUpURLList()  
        {  
            List<string> urls = new List<string>   
            {   
                "http://msdn.microsoft.com",  
                "http://msdn.microsoft.com/library/hh290138.aspx",  
                "http://msdn.microsoft.com/library/hh290140.aspx",  
                "http://msdn.microsoft.com/library/dd470362.aspx",  
                "http://msdn.microsoft.com/library/aa578028.aspx",  
                "http://msdn.microsoft.com/library/ms404677.aspx",  
                "http://msdn.microsoft.com/library/ff730837.aspx"  
            };  
            return urls;  
        }  
    }  
    // Sample output:  
  
    // Length of the downloaded website:  158856  
  
    // Download complete.  
}  
```  
  
## <a name="see-also"></a>Vea también  
 <xref:System.Threading.Tasks.Task.WhenAny%2A>   
 [Ajustar una aplicación asincrónica (C#)](../../../../csharp/programming-guide/concepts/async/fine-tuning-your-async-application.md)   
 [Programación asincrónica con async y await (C#)](../../../../csharp/programming-guide/concepts/async/index.md)   
 [Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/?LinkId=255046) (Ejemplo asincrónico: Ajustar la aplicación)
