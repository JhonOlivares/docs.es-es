---
title: Compatibilidad de SqlClient para alta disponibilidad y recuperación ante desastres
ms.date: 03/30/2017
ms.assetid: 61e0b396-09d7-4e13-9711-7dcbcbd103a0
ms.openlocfilehash: b51c3cb1eb3c8726b7de007a1c1519eae0733392
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70791616"
---
# <a name="sqlclient-support-for-high-availability-disaster-recovery"></a>Compatibilidad de SqlClient para alta disponibilidad y recuperación ante desastres
En este tema se describe la compatibilidad de SqlClient (agregada en .NET Framework 4,5) para la recuperación ante desastres de alta disponibilidad, Grupos de disponibilidad AlwaysOn.  Grupos de disponibilidad AlwaysOn característica se ha agregado a SQL Server 2012. Para obtener más información acerca de Grupos de disponibilidad AlwaysOn, vea Libros en pantalla de SQL Server.  
  
 Ahora puede especificar el agente de escucha del grupo de disponibilidad (alta disponibilidad, recuperación ante desastres) o una instancia de clúster de conmutación por error de SQL Server 2012 en la propiedad de conexión. Si una aplicación SqlClient está conectada a una base de datos de AlwaysOn que realiza conmutación por error, la conexión original se interrumpe y la aplicación debe abrir una nueva conexión para continuar trabajando después de la conmutación por error.  
  
 Si no se va a conectar a un agente de escucha de grupo de disponibilidad o a una instancia de clúster de conmutación por error de SQL Server 2012 y si varias direcciones IP están asociadas a un nombre de host, SqlClient iterará secuencialmente a través de todas las direcciones IP asociadas a la entrada DNS. Esto puede ser largo si la primera dirección IP que devuelve el servidor de DNS no está enlazada a ninguna tarjeta de interfaz de red (NIC). Al conectarse a un agente de escucha de grupo de disponibilidad o a una instancia de clúster de conmutación por error de SQL Server 2012, SqlClient intenta establecer conexiones con todas las direcciones IP en paralelo y, si un intento de conexión se realiza correctamente, el controlador descartará cualquier conexión pendiente. fallido.  
  
> [!NOTE]
> El incremento del tiempo de espera de conexión y la implementación de lógica de reintento de conexión aumentarán la probabilidad de que una aplicación se conecte a un grupo de disponibilidad. Además, dado que una conexión puede producir un error debido a una conmutación por error, debe implementar lógica de reintento de conexión, reintentando una conexión con errores hasta que se vuelva a conectar.  
  
 Se han agregado las siguientes propiedades de conexión a SqlClient en .NET Framework 4,5:  
  
- `ApplicationIntent`  
  
- `MultiSubnetFailover`  
  
 Mediante programación puede modificar estas palabras clave de cadena de conexión con:  
  
1. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.ApplicationIntent%2A>  
  
2. <xref:System.Data.SqlClient.SqlConnectionStringBuilder.MultiSubnetFailover%2A>  

> [!NOTE]
> No `MultiSubnetFailover` es `true` necesario establecer en con .NET Framework 4.6.1 o versiones posteriores.
  
## <a name="connecting-with-multisubnetfailover"></a>Conectar con MultiSubnetFailover  
 Especifique `MultiSubnetFailover=True` siempre al conectarse a un agente de escucha del grupo de disponibilidad SQL Server 2012 o a una instancia de clúster de conmutación por error de SQL Server 2012. `MultiSubnetFailover`permite una conmutación por error más rápida para todos los grupos de disponibilidad o la instancia de clúster de conmutación por error en SQL Server 2012 y reducirá significativamente el tiempo de conmutación por error para topologías AlwaysOn de una y varias subredes. Durante una conmutación por error de múltiples subredes, el cliente intentará conexiones en paralelo. Durante una conmutación por error de subred, se reintentará agresivamente la conexión TCP.  
  
 La `MultiSubnetFailover` propiedad de conexión indica que la aplicación se está implementando en un grupo de disponibilidad o SQL Server instancia de clúster de conmutación por error 2012 y que SqlClient intentará conectarse a la base de datos en la instancia principal de SQL Server intentando Conéctese a todas las direcciones IP. Cuando `MultiSubnetFailover=True` se especifica para una conexión, el cliente reintenta la conexión TCP más rápidamente que el valor predeterminado de intervalos de retransmisión TCP del sistema operativo. Esto permite una reconexión más rápida después de la conmutación por error de un grupo de disponibilidad AlwaysOn o de una instancia de clúster de conmutación por error AlwaysOn, y es aplicable a los grupos de disponibilidad y a las instancias de clúster de conmutación por error de una subred o de múltiples subredes.  
  
 Para obtener más información sobre las palabras clave de cadena de conexión de SqlClient, vea <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>.  
  
 `MultiSubnetFailover=True` Si se especifica al conectarse a algo que no sea un agente de escucha de grupo de disponibilidad o SQL Server instancia de clúster de conmutación por error 2012, se puede producir un impacto negativo en el rendimiento y no se admite.  
  
 Use las siguientes directrices para conectarse a un servidor en un grupo de disponibilidad o una instancia de clúster de conmutación por error de SQL Server 2012:  
  
- Use la propiedad de conexión `MultiSubnetFailover` al conectarse a una sola subred o a múltiples subredes; mejorará el rendimiento en ambos casos.  
  
- Para conectarse a un grupo de disponibilidad, especifique el agente de escucha del grupo de disponibilidad como el servidor en la cadena de conexión.  
  
- La conexión a una instancia de SQL Server configurada con más de 64 direcciones IP producirá un error de conexión.  
  
- El comportamiento de una aplicación que usa `MultiSubnetFailover` la propiedad de conexión no se ve afectado en función del tipo de autenticación: Autenticación de SQL Server, autenticación Kerberos o autenticación de Windows.  
  
- Aumente el valor de `Connect Timeout` para alojar el tiempo de conmutación por error y reducir los reintentos de conexión de la aplicación.  
  
- No se admiten transacciones distribuidas.  
  
 Si el enrutamiento de solo lectura no está vigente, la conexión a una ubicación de réplica secundaria producirá un error en las situaciones siguientes:  
  
1. Si la ubicación de réplica secundaria no está configurada para aceptar conexiones.  
  
2. Si una aplicación usa `ApplicationIntent=ReadWrite` (descrito a continuación) y la ubicación de la réplica secundaria está configurada para acceso de solo lectura.  
  
 <xref:System.Data.SqlClient.SqlDependency> no se admite en las réplicas secundarias de solo lectura.  
  
 Una conexión generará un error si una réplica primaria está configurada para rechazar cargas de trabajo de solo lectura y la cadena de conexión contiene `ApplicationIntent=ReadOnly`.  
  
## <a name="upgrading-to-use-multi-subnet-clusters-from-database-mirroring"></a>Actualización para usar clústeres de múltiples subredes desde la creación de reflejo de la base de datos  
 Se producirá un error de conexión (<xref:System.ArgumentException>) si las palabras clave de conexión `MultiSubnetFailover` y `Failover Partner` están presentes en la cadena de conexión, o si se usa `MultiSubnetFailover=True` y un protocolo distinto de TCP. También se producirá un error (<xref:System.Data.SqlClient.SqlException>) si `MultiSubnetFailover` se utiliza y el SQL Server devolverá una respuesta del asociado de conmutación por error que indica que forma parte de un par de creación de reflejo de la base de datos.  
  
 Si actualiza una aplicación SqlClient que usa actualmente creación de reflejo de la base de datos a un escenario de múltiples subredes, debe quitar la propiedad de conexión `Failover Partner` y reemplazarla con `MultiSubnetFailover` establecido en `True` y reemplazar el nombre del servidor en la cadena de conexión con un agente de escucha de grupo de disponibilidad. Si una cadena de conexión usa `Failover Partner` y `MultiSubnetFailover=True`, el controlador generará un error. Sin embargo, si una cadena de conexión usa `Failover Partner` y `MultiSubnetFailover=False` (o `ApplicationIntent=ReadWrite`), la aplicación usará creación de reflejo de la base de datos.  
  
 El controlador devolverá un error si la creación de reflejo de la base de datos se usa en la base de datos principal en el AG, y si `MultiSubnetFailover=True` se usa en la cadena de conexión que se conecta a una base de datos principal en lugar de a un agente de escucha de grupo de disponibilidad.  
  
## <a name="specifying-application-intent"></a>Especificar el intento de la aplicación  
 Cuando `ApplicationIntent=ReadOnly`, el cliente solicita una carga de trabajo de lectura al conectar con una base de datos habilitada para AlwaysOn. El servidor aplicará el intento en tiempo de conexión y durante una instrucción de base de datos USE pero solo en una base de datos habilitada para AlwaysOn.  
  
 La palabra clave `ApplicationIntent` no funciona con bases de datos heredadas de solo lectura.  
  
 Una base de datos puede permitir o denegar cargas de trabajo de lectura en la base de datos de destino de AlwaysOn. (Esto se hace con la `ALLOW_CONNECTIONS` cláusula de `SECONDARY_ROLE`las `PRIMARY_ROLE` instrucciones Transact-SQL).  
  
 La palabra clave `ApplicationIntent` se usa para habilitar el enrutamiento de solo lectura.  
  
## <a name="read-only-routing"></a>Enrutamiento de solo lectura  
 El enrutamiento de solo lectura es una característica que puede garantizar la disponibilidad de una réplica de solo lectura de una base de datos. Para habilitar el enrutamiento de solo lectura:  
  
1. Debe conectarse a un agente de escucha de grupo de disponibilidad AlwaysOn.  
  
2. La palabra clave de cadena de conexión `ApplicationIntent` se debe establecer en `ReadOnly`.  
  
3. El administrador de bases de datos debe configurar el grupo de disponibilidad para habilitar el enrutamiento de solo lectura.  
  
 Es posible que varias conexiones que usen enrutamiento de solo lectura no se conecten todas a la misma réplica de solo lectura. Los cambios en la sincronización de la base de datos o los cambios en la configuración de enrutamiento del servidor pueden producir conexiones de cliente a distintas réplicas de solo lectura. Para asegurarse de que todas las solicitudes de solo lectura se conectan a la misma réplica de solo lectura, no pase un agente de escucha de grupo de disponibilidad a la palabra clave de cadena de conexión `Data Source`. En su lugar, especifique el nombre de la instancia de solo lectura.  
  
 El enrutamiento de solo lectura puede tardar más que la conexión a la réplica primaria, ya que primero se conecta a la réplica primaria y después busca la mejor réplica secundaria legible disponible. Debido a esto, debe aumentar el tiempo de espera de inicio de sesión.  
  
## <a name="see-also"></a>Vea también

- [Características de SQL Server y ADO.NET](sql-server-features-and-adonet.md)
- [Información general sobre ADO.NET](../ado-net-overview.md)
