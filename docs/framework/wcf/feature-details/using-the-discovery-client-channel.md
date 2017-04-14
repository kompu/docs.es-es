---
title: "Usar el canal del cliente de detecci&#243;n | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework-4.6"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
ms.assetid: 1494242a-1d64-4035-8ecd-eb4f06c8d2ba
caps.latest.revision: 6
author: "Erikre"
ms.author: "erikre"
manager: "erikre"
caps.handback.revision: 6
---
# Usar el canal del cliente de detecci&#243;n
Al escribir una aplicación cliente de WCF, debe conocer la dirección del extremo del servicio al que está llamando.En muchas situaciones, la dirección del extremo de un servicio no se conoce de antemano o la dirección del servicio cambia con el tiempo.El canal del cliente de detección le permite escribir una aplicación cliente de WCF y describir el servicio al que desea llamar para que el canal de cliente envíe una solicitud de análisis de forma automática.Cuando un servicio responde, el canal del cliente de detección recupera la dirección del extremo para el servicio de la respuesta de análisis y la usa para llamar al servicio.  
  
## Usar el canal del cliente de detección  
 Para utilizar el canal de cliente de detección, agregue una instancia de <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> a su pila del canal de cliente.También puede usar la clase <xref:System.ServiceModel.Discovery.DynamicEndpoint> para que la clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> se agregue de forma automática al enlace si aún no lo está.  
  
> [!CAUTION]
>  Se recomienda que la clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> sea el elemento de nivel superior en la pila del canal de cliente.Cualquier elemento de enlace que se agregue en un nivel superior al de <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> debe asegurarse de que la clase <xref:System.ServiceModel.ChannelFactory> o el canal que crea no utilice la dirección del extremo o la dirección `Via` \(pasada al método `CreateChannel`\) porque puede que no contengan la dirección correcta.  
  
 La clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> contiene dos propiedades públicas:  
  
1.  <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A>, que se utiliza para describir el servicio al que desea llamar.  
  
2.  <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpoint%2A>, que especifica el extremo de detección al que se van a enviar los mensajes de detección.  
  
 La propiedad <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A> le permite especificar el contrato de servicio que está buscando, cualquier URI del ámbito necesario, así como la cantidad máxima de tiempo para intentar abrir el canal.El tipo de contrato se especifica llamando al constructor de <xref:System.ServiceModel.Discovery.FindCriteria%2A>.Los URI del ámbito se pueden agregar a la propiedad <xref:System.ServiceModel.Discovery.FindCriteria.Scopes%2A>.La propiedad <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> le permite especificar el número máximo de resultados a los que el cliente intenta conectarse.Cuando se recibe una respuesta del análisis, el cliente intenta abrir el canal mediante la dirección del extremo de la respuesta del análisis.Si se produce una excepción, el cliente pasa a la siguiente respuesta del análisis, esperando a recibir más respuestas si es necesario.Continúa así hasta que el canal se abre correctamente o hasta que se alcanza el número máximo de resultados.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] estos valores, vea <xref:System.ServiceModel.Discovery.FindCriteria%2A>.  
  
 La propiedad <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpoint%2A> le permite especificar el extremo de detección que desea usar.Normalmente, se trata de una clase <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>, pero puede ser cualquier extremo válido.  
  
 Cuando está creando el enlace que va a usar para comunicarse con el servicio, debe prestar atención y usar exactamente el mismo enlace que el servicio.La única diferencia es que el enlace del cliente tiene una clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> en la parte superior de la pila.Si el servicio usa uno de los enlaces proporcionados por el sistema, cree una nueva clase <xref:System.ServiceModel.Channels.CustomBinding> y pase el enlace proporcionado por el sistema al constructor de <xref:System.ServiceModel.CustomBinding.CustomBinding%2A>.A continuación, puede agregar la clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> llamando `Insert` en la propiedad <xref:System.ServiceModel.Channels.Binding.Elements%2A>.  
  
 Cuando haya agregado la clase <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> al enlace y la haya configurado, puede crear una instancia de la clase de cliente de WCF, abrirla y llamar a sus métodos.En el siguiente ejemplo, se usa el canal del cliente de detección para detectar un servicio WCF que implementa la clase `ICalculator` \(empleada en el tutorial de WCF de la Introducción\) y llama a su método `Add`.  
  
```  
// Create the DiscoveryClientBindingElement  
DiscoveryClientBindingElement bindingElement = new DiscoveryClientBindingElement();  
// Search for a service that implements the ICalculator interface, attempting to open  
// the channel a maximum of 2 times  
bindingElement.FindCriteria = new FindCriteria(typeof(ICalculator)) { MaxResults = 2 };  
// Use the UdpDiscoveryEndpoint  
bindingElement.DiscoveryEndpoint = new UdpDiscoveryEndpoint();  
  
// The service uses the BasicHttpBinding, so use that and insert the DiscoveryClientBindingElement at the   
// top of the stack  
CustomBinding binding = new CustomBinding(new BasicHttpBinding());  
binding.Elements.Insert(0,bindingElement);  
  
try  
{  
    // Create the WCF client and call a method  
    CalculatorClient client = new CalculatorClient(binding, new EndpointAddress("http://schemas.microsoft.com/dynamic"));  
    client.Open();  
    client.Add(1, 1);  
}  
catch (EndpointNotFoundException ex)  
{  
    Console.WriteLine("An exception occurred: " + ex.Message);  
}  
```  
  
## Seguridad y el canal del cliente de detección  
 Al usar el canal del cliente de detección, se especifican dos extremos.Uno se utiliza para los mensajes de detección, normalmente <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint>, y el otro es el extremo de la aplicación.Al implementar un servicio seguro, debe tener cuidado de proteger ambos extremos.[!INCLUDE[crabout](../../../../includes/crabout-md.md)] la seguridad, vea [Protección de servicios y clientes](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md).