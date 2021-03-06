---
title: Colas en Windows Communication Foundation
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF]
ms.assetid: 43008409-1bb4-4bd4-85d7-862c8f10ae20
ms.openlocfilehash: 92bd3a6809386764251c4a69c6bc583ed745205d
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76921359"
---
# <a name="queues-in-windows-communication-foundation"></a>Colas en Windows Communication Foundation
En los temas de esta sección se describe la compatibilidad de Windows Communication Foundation (WCF) con las colas de. WCF proporciona compatibilidad para la puesta en cola aprovechando Microsoft Message Queuing (conocido anteriormente como MSMQ) como transporte y habilita los siguientes escenarios:  
  
- Aplicaciones acopladas débilmente Las aplicaciones emisoras pueden enviar mensajes a colas sin necesidad de conocer si la aplicación receptora está disponible para procesar el mensaje. La cola proporciona independencia de procesamiento que permite a una aplicación emisora enviar mensajes a la cola a una tasa que no depende de cómo de rápido las aplicaciones receptoras puedan procesar los mensajes. La disponibilidad del sistema total aumenta al enviar los mensajes a una cola que no está fuertemente acoplada al procesamiento de mensajes.  
  
- Aislamiento de errores. Las aplicaciones que envían o reciben mensajes a una cola pueden producir un error sin afectar al resto. Por ejemplo, si se produce un error en la aplicación receptora, la aplicación emisora puede continuar enviando mensajes a la cola. Cuando el receptor esté operativo de nuevo, podrá procesar los mensajes de la cola. El aislamiento de errores aumenta la fiabilidad y disponibilidad total del sistema.  
  
- Nivelación de la carga. Las aplicaciones emisoras pueden agobiar a las aplicaciones receptoras con mensajes. Las colas pueden administrar tasas de consumo y producciones de mensajes desequilibradas, de modo que el receptor no se agobie.  
  
- Operaciones desconectadas. Las operaciones de envío, recepción y procesamiento se pueden desconectar al comunicar a través de redes de latencia alta o redes de disponibilidad limitada, como es el caso de los dispositivos móviles. Las colas permiten a estas operaciones continuar, incluso cuando los puntos de conexión están desconectados. Cuando se restablece la conexión, la cola reenvía los mensajes a la aplicación receptora.  
  
 Para usar la característica de colas en una aplicación WCF, puede usar uno de los enlaces estándar o puede crear un enlace personalizado si uno de los enlaces estándar no satisface sus requisitos. Para obtener más información sobre los enlaces estándar pertinentes y cómo elegir uno, consulte [Cómo: intercambiar mensajes con extremos de WCF y aplicaciones de Message Queue Server](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md). Para obtener más información sobre cómo crear enlaces personalizados, vea [Enlaces personalizados](../../../../docs/framework/wcf/extending/custom-bindings.md).  
  
## <a name="in-this-section"></a>Esta sección  
 [Información general de colas](../../../../docs/framework/wcf/feature-details/queues-overview.md)  
 Una información general de conceptos de colas de mensajes.  
  
 [Colas en WCF](../../../../docs/framework/wcf/feature-details/queuing-in-wcf.md)  
 Información general sobre la compatibilidad con las colas de WCF.  
  
 [Intercambio de mensajes en cola con puntos de conexión de WCF](../../../../docs/framework/wcf/feature-details/how-to-exchange-queued-messages-with-wcf-endpoints.md)  
 Explica cómo usar la clase <xref:System.ServiceModel.NetMsmqBinding> para comunicarse entre un cliente WCF y un servicio WCF.  
  
 [Intercambio de mensajes con puntos de conexión de WCF y aplicaciones de Message Queue Server](../../../../docs/framework/wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)  
 Explica cómo usar el <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> para comunicarse entre WCF y las aplicaciones de Message Queuing.  
  
 [Agrupación de los mensajes en cola de una sesión](../../../../docs/framework/wcf/feature-details/grouping-queued-messages-in-a-session.md)  
 Explica cómo agrupar los mensajes en una cola para facilitar el procesamiento de mensajes correlacionado por una aplicación receptora única.  
  
 [Mensajes por lotes en una transacción](../../../../docs/framework/wcf/feature-details/batching-messages-in-a-transaction.md)  
 Explica cómo procesar mensajes por lotes en una transacción.  
  
 [Utilización de las colas de mensajes no enviados para administrar los errores en la transferencia de mensajes](../../../../docs/framework/wcf/feature-details/using-dead-letter-queues-to-handle-message-transfer-failures.md)  
 Explica cómo administrar los errores de entrega y transferencia de mensajes mediante las colas de mensajes no enviados y cómo procesar mensajes desde la cola de mensajes no enviados.  
  
 [Control de mensajes dudosos](../../../../docs/framework/wcf/feature-details/poison-message-handling.md)  
 Explica cómo administrar los mensajes dudosos (mensajes que han superado el número máximo de intentos de entrega a la aplicación receptora).  
  
 [Diferencias en las características de cola en Windows Vista, Windows Server 2003 y Windows XP](../../../../docs/framework/wcf/feature-details/diff-in-queue-in-vista-server-2003-windows-xp.md)  
 Resume las diferencias en la característica de colas de WCF entre Windows Vista, Windows Server 2003 y Windows XP.  
  
 [Protección de mensajes utilizando la seguridad de transporte](../../../../docs/framework/wcf/feature-details/securing-messages-using-transport-security.md)  
 Describe cómo utilizar la seguridad de transporte para proteger los mensajes en cola.  
  
 [Protección de mensajes mediante la seguridad de mensajes](../../../../docs/framework/wcf/feature-details/securing-messages-using-message-security.md)  
 Describe cómo utilizar la seguridad de mensaje para proteger los mensajes en cola.  
  
 [Solución de problemas de la mensajería en cola](../../../../docs/framework/wcf/feature-details/troubleshooting-queued-messaging.md)  
 Explica cómo solucionar los problemas comunes de la puesta en cola.  
  
 [Procedimientos recomendados para la comunicación en cola](../../../../docs/framework/wcf/feature-details/best-practices-for-queued-communication.md)  
 Explica los procedimientos recomendados para usar la comunicación en cola de WCF.  
