---
title: "Transporte: transacciones personalizadas sobre ejemplo UDP | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 6cebf975-41bd-443e-9540-fd2463c3eb23
caps.latest.revision: 21
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 21
---
# Transporte: transacciones personalizadas sobre ejemplo UDP
Este ejemplo se basa en el ejemplo [Transporte: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) en los [Extensibilidad de transporte](../../../../docs/framework/wcf/samples/transport-extensibility.md) de [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)]. Extiende el ejemplo de transporte UDP para admitir el flujo de transacciones personalizado y muestra el uso de la propiedad <xref:System.ServiceModel.Channels.TransactionMessageProperty>.  
  
## Cambios de código en el ejemplo de transporte de UDP  
 Para mostrar el flujo de transacciones, el ejemplo cambia el contrato de servicio para `ICalculatorContract` con objeto de exigir un ámbito de transacción para `CalculatorService.Add()`.El ejemplo también agrega un parámetro `System.Guid` adicional al contrato de la operación `Add`.Este parámetro se utiliza para pasar el identificador de la transacción del cliente al servicio.  
  
```  
class CalculatorService : IDatagramContract, ICalculatorContract  
{  
    [OperationBehavior(TransactionScopeRequired=true)]  
    public int Add(int x, int y, Guid clientTransactionId)  
    {  
        if(Transaction.Current.TransactionInformation.DistributedIdentifier == clientTransactionId)  
    {  
        Console.WriteLine("The client transaction has flowed to the service");  
    }  
    else  
    {  
     Console.WriteLine("The client transaction has NOT flowed to the service");  
    }  
  
    Console.WriteLine("   adding {0} + {1}", x, y);              
    return (x + y);  
    }  
  
    [...]  
}  
```  
  
 El ejemplo [Transporte: UDP](../../../../docs/framework/wcf/samples/transport-udp.md) usa los paquetes UDP para los mensajes entre un cliente y un servicio.[Transport: Custom Transport Sample](../../../../docs/framework/wcf/samples/transport-custom-transactions-over-udp-sample.md) utiliza el mismo mecanismo para transportar mensajes, pero cuando fluye una transacción, se inserta en el paquete UDP junto con el mensaje codificado.  
  
```  
byte[] txmsgBuffer =                TransactionMessageBuffer.WriteTransactionMessageBuffer(txPropToken, messageBuffer);  
  
int bytesSent = this.socket.SendTo(txmsgBuffer, 0, txmsgBuffer.Length, SocketFlags.None, this.remoteEndPoint);  
```  
  
 `TransactionMessageBuffer.WriteTransactionMessageBuffer` es un método auxiliar que contiene la nueva funcionalidad para combinar el token de propagación para la transacción actual con la entidad del mensaje y lo coloca en un búfer.  
  
 Para el transporte de flujo de transacción personalizado, la implementación del cliente debe conocer qué operaciones de servicio requieren flujo de transacción y pasar esta información a [!INCLUDE[indigo2](../../../../includes/indigo2-md.md)].Debería haber también un mecanismo para transmitir la transacción del usuario en el nivel de transporte.Este ejemplo utiliza "inspectores de mensaje[!INCLUDE[indigo2](../../../../includes/indigo2-md.md)]" para obtener esta información.El inspector de mensaje de cliente implementado aquí, que se denomina `TransactionFlowInspector`, realiza las tareas siguientes:  
  
-   Determina si se debe hacer fluir una transacción o no para una acción de mensaje determinada \(esto tiene lugar en `IsTxFlowRequiredForThisOperation()`\).  
  
-   Asocia la transacción de ambiente actual con el mensaje utilizando `TransactionFlowProperty`, si es necesario hacer fluir una transacción \(esto se hace en `BeforeSendRequest()`\).  
  
```  
public class TransactionFlowInspector : IClientMessageInspector  
{  
   void IClientMessageInspector.AfterReceiveReply(ref           System.ServiceModel.Channels.Message reply, object correlationState)  
  {  
  }  
  
   public object BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
   {  
       // obtain the tx propagation token  
       byte[] propToken = null;             
       if (Transaction.Current != null && IsTxFlowRequiredForThisOperation(request.Headers.Action))  
       {  
           try  
           {  
               propToken = TransactionInterop.GetTransmitterPropagationToken(Transaction.Current);  
           }  
           catch (TransactionException e)  
           {  
              throw new CommunicationException("TransactionInterop.GetTransmitterPropagationToken failed.", e);  
           }  
       }  
  
      // set the propToken on the message in a TransactionFlowProperty  
       TransactionFlowProperty.Set(propToken, request);  
  
       return null;              
    }  
  }  
  
  static bool IsTxFlowRequiredForThisOperation(String action)  
 {  
       // In general, this should contain logic to identify which operations (actions)      require transaction flow.  
      [...]  
 }  
}  
  
```  
  
 El `TransactionFlowInspector` se pasa al marco utilizando un comportamiento personalizado: `TransactionFlowBehavior`.  
  
```  
public class TransactionFlowBehavior : IEndpointBehavior  
{  
       public void AddBindingParameters(ServiceEndpoint endpoint,            System.ServiceModel.Channels.BindingParameterCollection bindingParameters)  
      {  
      }  
  
       public void ApplyClientBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
      {  
            TransactionFlowInspector inspector = new TransactionFlowInspector();  
            clientRuntime.MessageInspectors.Add(inspector);  
      }  
  
      public void ApplyDispatchBehavior(ServiceEndpoint endpoint, System.ServiceModel.Dispatcher.EndpointDispatcher endpointDispatcher)  
     {  
     }  
  
      public void Validate(ServiceEndpoint endpoint)  
      {  
      }  
}  
```  
  
 Con el mecanismo anterior en su lugar, el código de usuario crea `TransactionScope` antes de llamar a la operación de servicio.El inspector del mensaje garantiza que la transacción se pasa al transporte en caso de que sea necesaria para que fluya a la operación de servicio.  
  
```  
CalculatorContractClient calculatorClient = new CalculatorContractClient("SampleProfileUdpBinding_ICalculatorContract");  
calculatorClient.Endpoint.Behaviors.Add(new TransactionFlowBehavior());               
  
try  
{  
       for (int i = 0; i < 5; ++i)  
      {  
              // call the 'Add' service operation under a transaction scope  
             using (TransactionScope ts = new TransactionScope())  
             {  
        [...]  
        Console.WriteLine(calculatorClient.Add(i, i * 2));  
         }  
      }               
       calculatorClient.Close();  
}  
catch (TimeoutException)  
{  
    calculatorClient.Abort();  
}  
catch (CommunicationException)  
{  
    calculatorClient.Abort();  
}  
catch (Exception)  
{  
    calculatorClient.Abort();  
    throw;  
}  
```  
  
 Al recibir un paquete UDP del cliente, el servicio lo deserializa para extraer el mensaje y posiblemente una transacción.  
  
```  
count = listenSocket.EndReceiveFrom(result, ref dummy);  
  
// read the transaction and message                       TransactionMessageBuffer.ReadTransactionMessageBuffer(buffer, count, out transaction, out msg);  
```  
  
 `TransactionMessageBuffer.ReadTransactionMessageBuffer()` es el método auxiliar que invierte el proceso de serialización realizado por `TransactionMessageBuffer.WriteTransactionMessageBuffer()`.  
  
 Si se fluye una transacción, se anexa al mensaje en `TransactionMessageProperty`.  
  
```  
message = MessageEncoderFactory.Encoder.ReadMessage(msg, bufferManager);  
  
if (transaction != null)  
{  
       TransactionMessageProperty.Set(transaction, message);  
}  
```  
  
 Esto garantiza que el distribuidor escoja la transacción en el momento de la distribución y lo utilice al llamar a la operación de servicio dirigida por el mensaje.  
  
#### Para configurar, compilar y ejecutar el ejemplo  
  
1.  Para compilar la solución, siga las instrucciones de [Compilación de los ejemplos de Windows Communication Foundation](../../../../docs/framework/wcf/samples/building-the-samples.md).  
  
2.  El ejemplo actual se debería ejecutar de igual forma que el ejemplo [Transporte: UDP](../../../../docs/framework/wcf/samples/transport-udp.md).Para ejecutarlo, inicie el servicio con UdpTestService.exe.Si está ejecutando [!INCLUDE[windowsver](../../../../includes/windowsver-md.md)], debe iniciar el servicio con privilegios elevados.Para ello, haga clic con el botón secundario en UdpTestService.exe en el [!INCLUDE[fileExplorer](../../../../includes/fileexplorer-md.md)] y haga clic en **Ejecutar como administrador**.  
  
3.  Esto produce el siguiente resultado.  
  
    ```  
    Testing Udp From Code.  
    Service is started from code...  
    Press <ENTER> to terminate the service and start service from config...  
    ```  
  
4.  En este momento puede iniciar el cliente ejecutando UdpTestClient.exe.El resultado producido por el cliente es el siguiente.  
  
    ```  
    0  
    3  
    6  
    9  
    12  
    Press <ENTER> to complete test.  
    ```  
  
5.  El resultado del servicio es como sigue.  
  
    ```  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    Hello, world!  
    The client transaction has flowed to the service  
       adding 0 + 0  
    The client transaction has flowed to the service  
       adding 1 + 2  
    The client transaction has flowed to the service  
       adding 2 + 4  
    The client transaction has flowed to the service  
       adding 3 + 6  
    The client transaction has flowed to the service  
       adding 4 + 8  
    ```  
  
6.  La aplicación de servicio muestra `The client transaction has flowed to the service` del mensaje si puede coincidir con el identificador de la transacción enviado por el cliente, en el parámetro `clientTransactionId` de la operación `CalculatorService.Add()`, con el identificador de la transacción del servicio.Se obtiene una coincidencia solo si la transacción del cliente ha fluido hacia el servicio.  
  
7.  Para ejecutar la aplicación cliente en los extremos publicados utilizando la configuración, presione Entrar en la ventana de la aplicación de servicio y después ejecute de nuevo el cliente de pruebas.Debería ver el siguiente resultado en el servicio.  
  
    ```  
    Testing Udp From Config.  
    Service is started from config...  
    Press <ENTER> to terminate the service and exit...  
    ```  
  
8.  Ejecutar el cliente con el servicio genera ahora un resultado similar al anterior.  
  
9. Para regenerar el código de cliente y la configuración utilizando Svcutil.exe, inicie la aplicación de servicio y, a continuación, ejecute el comando Svcutil.exe siguiente desde el directorio raíz del ejemplo.  
  
    ```  
    svcutil http://localhost:8000/udpsample/ /reference:UdpTranport\bin\UdpTransport.dll /svcutilConfig:svcutil.exe.config  
    ```  
  
10. Observe que Svcutil.exe no genera la configuración de la extensión de enlace `sampleProfileUdpBinding`; debe agregarla manualmente.  
  
    ```  
    <configuration>  
        <system.serviceModel>      
            …  
            <extensions>  
                <!-- This was added manually because svcutil.exe does not add this extension to the file -->  
                <bindingExtensions>  
                    <add name="sampleProfileUdpBinding" type="Microsoft.ServiceModel.Samples.SampleProfileUdpBindingCollectionElement, UdpTransport" />  
                </bindingExtensions>  
            </extensions>  
        </system.serviceModel>  
    </configuration>  
    ```  
  
> [!IMPORTANT]
>  Puede que los ejemplos ya estén instalados en su equipo.Compruebe el siguiente directorio \(valor predeterminado\) antes de continuar.  
>   
>  `<>InstallDrive:\WF_WCF_Samples`  
>   
>  Si no existe este directorio, vaya a la página de [ejemplos de Windows Communication Foundation \(WCF\) y Windows Workflow Foundation \(WF\) Samples para .NET Framework 4](http://go.microsoft.com/fwlink/?LinkId=150780) para descargar todos los ejemplos de [!INCLUDE[wf1](../../../../includes/wf1-md.md)] y [!INCLUDE[indigo1](../../../../includes/indigo1-md.md)].Este ejemplo se encuentra en el siguiente directorio.  
>   
>  `<unidadDeInstalación>:\WF_WCF_Samples\WCF\Extensibility\Transactions\TransactionMessagePropertyUDPTransport`  
  
## Vea también  
 [Transporte: UDP](../../../../docs/framework/wcf/samples/transport-udp.md)