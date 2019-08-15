---
title: Implementación de un bus de eventos con RabbitMQ para el entorno de desarrollo o de prueba
description: Arquitectura de microservicios de .NET para aplicaciones .NET en contenedores | Uso de RabbitMQ para implementar la mensajería de un bus de eventos para entornos de integración con fines de desarrollo de entornos de prueba.
ms.date: 10/02/2018
ms.openlocfilehash: af02208bb9e680403a04377ccb740a8b15be29bc
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675892"
---
# <a name="implementing-an-event-bus-with-rabbitmq-for-the-development-or-test-environment"></a><span data-ttu-id="9fcaf-103">Implementación de un bus de eventos con RabbitMQ para el entorno de desarrollo o de prueba</span><span class="sxs-lookup"><span data-stu-id="9fcaf-103">Implementing an event bus with RabbitMQ for the development or test environment</span></span>

<span data-ttu-id="9fcaf-104">Para empezar, hay que decir que si crea el bus de eventos personalizado basado en RabbitMQ que se ejecuta en un contenedor, como hace la aplicación eShopOnContainers, debe usarse únicamente para los entornos de desarrollo y prueba.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-104">We should start by saying that if you create your custom event bus based on RabbitMQ running in a container, as the eShopOnContainers application does, it should be used only for your development and test environments.</span></span> <span data-ttu-id="9fcaf-105">No debe usarlo para el entorno de producción, salvo que lo cree como parte de un bus de servicio para entornos de producción.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-105">You should not use it for your production environment, unless you are building it as a part of a production-ready service bus.</span></span> <span data-ttu-id="9fcaf-106">En un bus de eventos personalizado simple pueden faltar muchas de las características críticas para entornos de producción que tiene un bus de servicio comercial.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-106">A simple custom event bus might be missing many production-ready critical features that a commercial service bus has.</span></span>

<span data-ttu-id="9fcaf-107">Una de las implementaciones personalizadas de bus de eventos en eShopOnContainers es básicamente una biblioteca que usa la API de RabbitMQ (hay otra implementación basada en Azure Service Bus).</span><span class="sxs-lookup"><span data-stu-id="9fcaf-107">One of the event bus custom implementation in eShopOnContainers is basically a library using the RabbitMQ API (There’s another implementation based on Azure Service Bus).</span></span>

<span data-ttu-id="9fcaf-108">La implementación del bus de eventos con RabbitMQ permite que los microservicios se suscriban a eventos, los publiquen y los reciban, tal como se muestra en la figura 6-21.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-108">The event bus implementation with RabbitMQ lets microservices subscribe to events, publish events, and receive events, as shown in Figure 6-21.</span></span>

![Para controlar la distribución, RabbitMQ funciona como intermediario entre el publicador de mensajes y los suscriptores.](./media/image22.png)

<span data-ttu-id="9fcaf-110">**Figura 6-21.**</span><span class="sxs-lookup"><span data-stu-id="9fcaf-110">**Figure 6-21.**</span></span> <span data-ttu-id="9fcaf-111">Implementación de RabbitMQ de un bus de eventos</span><span class="sxs-lookup"><span data-stu-id="9fcaf-111">RabbitMQ implementation of an event bus</span></span>

<span data-ttu-id="9fcaf-112">En el código, la clase EventBusRabbitMQ implementa la interfaz genérica de IEventBus.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-112">In the code, the EventBusRabbitMQ class implements the generic IEventBus interface.</span></span> <span data-ttu-id="9fcaf-113">Esto se basa en la inserción de dependencias para que pueda cambiar de esta versión de desarrollo/pruebas a una versión de producción.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-113">This is based on Dependency Injection so that you can swap from this dev/test version to a production version.</span></span>

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Implementation using RabbitMQ API
    //...
}
```

<span data-ttu-id="9fcaf-114">La implementación de RabbitMQ de un bus de eventos de desarrollo/pruebas de ejemplo es un código reutilizable.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-114">The RabbitMQ implementation of a sample dev/test event bus is boilerplate code.</span></span> <span data-ttu-id="9fcaf-115">Tiene que controlar la conexión con el servidor RabbitMQ y proporcionar código para publicar un evento de mensaje a las colas.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-115">It has to handle the connection to the RabbitMQ server and provide code for publishing a message event to the queues.</span></span> <span data-ttu-id="9fcaf-116">También debe implementar un diccionario de las colecciones de controladores de eventos de integración para cada tipo de evento; estos tipos de eventos pueden tener una instancia diferente y diferentes suscripciones para cada microservicio receptor, tal como se muestra en la figura 6-21.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-116">It also has to implement a dictionary of collections of integration event handlers for each event type; these event types can have a different instantiation and different subscriptions for each receiver microservice, as shown in Figure 6-21.</span></span>

## <a name="implementing-a-simple-publish-method-with-rabbitmq"></a><span data-ttu-id="9fcaf-117">Implementar un método de publicación sencillo con RabbitMQ</span><span class="sxs-lookup"><span data-stu-id="9fcaf-117">Implementing a simple publish method with RabbitMQ</span></span>

<span data-ttu-id="9fcaf-118">El código siguiente es una versión ***simplificada*** de una implementación de bus de eventos de RabbitMQ con el objetivo de presentar todo el escenario.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-118">The following code is a ***simplified*** version of an event bus implementation for RabbitMQ, to showcase the whole scenario.</span></span> <span data-ttu-id="9fcaf-119">Lo cierto es que este no es el modo de controlar la conexión.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-119">You don't really handle the connection this way.</span></span> <span data-ttu-id="9fcaf-120">Para ver la implementación completa, consulte el código real en el repositorio [dotnet-architecture/eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs).</span><span class="sxs-lookup"><span data-stu-id="9fcaf-120">To see the full implementation, see the actual code in the [dotnet-architecture/eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) repository.</span></span> 

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Member objects and other methods ...
    // ...

    public void Publish(IntegrationEvent @event)
    {
        var eventName = @event.GetType().Name;
        var factory = new ConnectionFactory() { HostName = _connectionString };
        using (var connection = factory.CreateConnection())
        using (var channel = connection.CreateModel())
        {
            channel.ExchangeDeclare(exchange: _brokerName,
                type: "direct");
            string message = JsonConvert.SerializeObject(@event);
            var body = Encoding.UTF8.GetBytes(message);
            channel.BasicPublish(exchange: _brokerName,
                routingKey: eventName,
                basicProperties: null,
                body: body);
       }
    }
}
```

<span data-ttu-id="9fcaf-121">El [código real](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) del método Publish en la aplicación eShopOnContainers se ha mejorado con una directiva de reintentos de [Polly](https://github.com/App-vNext/Polly), que vuelve a intentar la tarea un número determinado de veces en caso de que el contenedor RabbitMQ no esté listo.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-121">The [actual code](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) of the Publish method in the eShopOnContainers application is improved by using a [Polly](https://github.com/App-vNext/Polly) retry policy, which retries the task a certain number of times in case the RabbitMQ container is not ready.</span></span> <span data-ttu-id="9fcaf-122">Esto puede ocurrir cuando docker-compose inicia los contenedores; por ejemplo, el contenedor de RabbitMQ puede iniciarse más lentamente que los otros contenedores.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-122">This can occur when docker-compose is starting the containers; for example, the RabbitMQ container might start more slowly than the other containers.</span></span>

<span data-ttu-id="9fcaf-123">Como se ha mencionado anteriormente, hay muchas configuraciones posibles en RabbitMQ, por lo que este código debe usarse únicamente para entornos de desarrollo y pruebas.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-123">As mentioned earlier, there are many possible configurations in RabbitMQ, so this code should be used only for dev/test environments.</span></span>

## <a name="implementing-the-subscription-code-with-the-rabbitmq-api"></a><span data-ttu-id="9fcaf-124">Implementar el código de suscripción con la API de RabbitMQ</span><span class="sxs-lookup"><span data-stu-id="9fcaf-124">Implementing the subscription code with the RabbitMQ API</span></span>

<span data-ttu-id="9fcaf-125">Al igual que con el código de publicación, el código siguiente es una simplificación de parte de la implementación del bus de eventos para RabbitMQ.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-125">As with the publish code, the following code is a simplification of part of the event bus implementation for RabbitMQ.</span></span> <span data-ttu-id="9fcaf-126">Una vez más, normalmente no deberá cambiarlo a menos que lo esté mejorando.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-126">Again, you usually do not need to change it unless you are improving it.</span></span>

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Member objects and other methods ...
    // ...

    public void Subscribe<T, TH>()
        where T : IntegrationEvent
        where TH : IIntegrationEventHandler<T>
    {
        var eventName = _subsManager.GetEventKey<T>();
        
        var containsKey = _subsManager.HasSubscriptionsForEvent(eventName);
        if (!containsKey)
        {
            if (!_persistentConnection.IsConnected)
            {
                _persistentConnection.TryConnect();
            }

            using (var channel = _persistentConnection.CreateModel())
            {
                channel.QueueBind(queue: _queueName,
                                    exchange: BROKER_NAME,
                                    routingKey: eventName);
            }
        }

        _subsManager.AddSubscription<T, TH>();
    }
}
```

<span data-ttu-id="9fcaf-127">Cada tipo de evento tiene un canal relacionado para obtener eventos de RabbitMQ.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-127">Each event type has a related channel to get events from RabbitMQ.</span></span> <span data-ttu-id="9fcaf-128">Puede tener tantos controladores de eventos por tipo de canal y evento como sea necesario.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-128">You can then have as many event handlers per channel and event type as needed.</span></span>

<span data-ttu-id="9fcaf-129">El método Subscribe acepta un objeto IIntegrationEventHandler, que es similar a un método de devolución de llamada en el microservicio actual, además de su objeto IntegrationEvent relacionado.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-129">The Subscribe method accepts an IIntegrationEventHandler object, which is like a callback method in the current microservice, plus its related IntegrationEvent object.</span></span> <span data-ttu-id="9fcaf-130">Después, el código agrega ese controlador de eventos a la lista de controladores de eventos que puede tener cada tipo de evento de integración por microservicio cliente.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-130">The code then adds that event handler to the list of event handlers that each integration event type can have per client microservice.</span></span> <span data-ttu-id="9fcaf-131">Si el código cliente ya no se ha suscrito al evento, el código crea un canal para el tipo de evento de forma que pueda recibir eventos en un estilo de inserción de RabbitMQ cuando ese evento se publique desde cualquier otro servicio.</span><span class="sxs-lookup"><span data-stu-id="9fcaf-131">If the client code has not already been subscribed to the event, the code creates a channel for the event type so it can receive events in a push style from RabbitMQ when that event is published from any other service.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="9fcaf-132">[Anterior](integration-event-based-microservice-communications.md)
>[Siguiente](subscribe-events.md)</span><span class="sxs-lookup"><span data-stu-id="9fcaf-132">[Previous](integration-event-based-microservice-communications.md)
[Next](subscribe-events.md)</span></span>