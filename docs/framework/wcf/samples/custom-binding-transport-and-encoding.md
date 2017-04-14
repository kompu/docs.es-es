---
title: "Transporte y codificaci&#243;n de enlace personalizado | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6c0b353d-79ee-4e61-b348-be49ad0e9a16
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# Transporte y codificaci&#243;n de enlace personalizado
Un enlace personalizado se define mediante una lista ordenada de elementos de enlace discretos.Este ejemplo muestra cómo configurar un enlace personalizado con varios elementos de codificación de mensajes y transporte.  
  
> [!NOTE]
>  El procedimiento de instalación y las instrucciones de compilación de este ejemplo se encuentran al final de este tema.  
  
 Este ejemplo está basado en [Autohospedaje](../../../../docs/framework/wcf/samples/self-host.md) y se ha modificado para configurar tres extremos para admitir los transportes HTTP, TCP y NamedPipe con enlaces personalizados.La configuración del cliente se modificó de igual forma y el código de cliente cambió para comunicarse con cada uno de los tres extremos.  
  
 El ejemplo muestra cómo configurar un enlace personalizado que admite un transporte determinado y la codificación de mensajes.Esto se logra configurando un transporte y una codificación de mensajes para el elemento `binding`.La clasificación de elementos de enlace es importante para definir un enlace personalizado, porque cada uno representa un nivel en la pila de canales \(consulte [Enlaces personalizados](../../../../docs/framework/wcf/extending/custom-bindings.md)\).Este ejemplo configura tres enlaces personalizados: un transporte HTTP con codificación de texto, un transporte TCP con codificación de texto y un transporte NamedPipe con una codificación binaria.  
  
 La configuración de servicio define los enlaces personalizados de la siguiente forma:  
  
```  
<bindings>  
    <customBinding>  
        <binding name="HttpBinding" >  
            <textMessageEncoding   
                messageVersion="Soap12Addressing10"/>  
            <httpTransport />  
        </binding>  
        <binding name="TcpBinding" >  
            <textMessageEncoding />  
            <tcpTransport />  
        </binding>  
        <binding name="NamedPipeBinding" >  
            <binaryMessageEncoding />  
            <namedPipeTransport />  
        </binding>  
    </customBinding>  
</bindings>  
  
```  
  
 Al ejecutar el ejemplo, las solicitudes de operación y las respuestas se muestran en la ventana de la consola del cliente y del servicio.El cliente se comunica con cada uno de los tres extremos, teniendo acceso primero a HTTP, después a TCP y finalmente a NamedPipe.Presione ENTRAR en cada ventana de la consola para cerrar el servicio y el cliente.  
  
 El enlace `namedPipeTransport` no admite las operaciones de equipo a equipo.Sólo se utiliza para la comunicación en el mismo equipo.Por consiguiente, al ejecutar el ejemplo en un escenario con varios equipos, marque como comentarios las líneas siguientes en el archivo de código de cliente:  
  
```csharp  
CalculatorClient client = new CalculatorClient("default");  
Console.WriteLine("Communicate with named pipe endpoint.");  
// Call operations.  
DoCalculations(client);  
//Closing the client gracefully closes the connection and cleans up resources  
client.Close();  
```  
  
```vb  
Dim client As New CalculatorClient("default")  
Console.WriteLine("Communicate with named pipe endpoint.")  
' call operations  
DoCalculations(client)  
'Closing the client gracefully closes the connection and cleans up resources  
client.Close()  
```  
  
> [!NOTE]
>  Si usa Svcutil.exe para recompilar la configuración de este ejemplo, asegúrese de que modifica el nombre del extremo en la configuración del cliente para que coincida con el código de cliente.  
  
### Para configurar, compilar y ejecutar el ejemplo  
  
1.  Asegúrese de realizar los [Procedimiento de instalación única para los ejemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md).  
  
2.  Para compilar el código C\#, C\+\+ o Visual Basic .NET Edition de la solución, siga las instrucciones de [Compilación de los ejemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
3.  Para ejecutar el ejemplo en una configuración con un único equipo o con varios, siga las instrucciones de [Ejecución de los ejemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/running-the-samples.md).  
  
> [!IMPORTANT]
>  Puede que los ejemplos ya estén instalados en su equipo.Compruebe el siguiente directorio \(valor predeterminado\) antes de continuar.  
>   
>  `<>InstallDrive:\WF_WCF_Samples`  
>   
>  Si no existe este directorio, vaya a la página de [ejemplos de Windows Communication Foundation \(WCF\) y Windows Workflow Foundation \(WF\) Samples para .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) para descargar todos los ejemplos de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)] y [!INCLUDE[wf1](../../../../includes/wf1-md.md)].Este ejemplo se encuentra en el siguiente directorio.  
>   
>  `<unidadDeInstalación>:\WF_WCF_Samples\WCF\Basic\Binding\Custom\Transport`  
  
## Vea también