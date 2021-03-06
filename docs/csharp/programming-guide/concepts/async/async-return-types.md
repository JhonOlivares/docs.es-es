---
title: Tipos de valor devueltos asincrónicos (C#)
ms.date: 04/14/2020
ms.assetid: ddb2539c-c898-48c1-ad92-245e4a996df8
ms.openlocfilehash: 73a6e1924652c8635377547e2faddc864ac5540a
ms.sourcegitcommit: c91110ef6ee3fedb591f3d628dc17739c4a7071e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389133"
---
# <a name="async-return-types-c"></a>Tipos de valor devueltos asincrónicos (C#)

Los métodos asincrónicos pueden tener los siguientes tipos de valor devuelto:

- <xref:System.Threading.Tasks.Task%601>, para un método asincrónico que devuelve un valor.
- <xref:System.Threading.Tasks.Task>, para un método asincrónico que realiza una operación pero no devuelve ningún valor.
- `void`, para un controlador de eventos.
- A partir de C# 7.0, cualquier tipo que tenga un método `GetAwaiter` accesible. El objeto devuelto por el método `GetAwaiter` debe implementar la interfaz <xref:System.Runtime.CompilerServices.ICriticalNotifyCompletion?displayProperty=nameWithType>.
- A partir C# 8.0, <xref:System.Collections.Generic.IAsyncEnumerable%601>, para un método asincrónico que devuelve una *secuencia asincrónica*.

Para más información sobre los métodos async, vea [Programación asincrónica con async y await 8C#)](./index.md).  
  
## <a name="tasktresult-return-type"></a>Tipo de valor devuelto Task\<TResult\>  
El tipo de valor devuelto <xref:System.Threading.Tasks.Task%601> se usa para un método asincrónico que contiene una instrucción [return](../../../language-reference/keywords/return.md) (C#) en la que el operando es `TResult`.  
  
En el ejemplo siguiente, el método asincrónico `GetLeisureHours` contiene una instrucción `return` que devuelve un entero. Por tanto, la declaración del método debe tener un tipo de valor devuelto de `Task<int>`.  El método asincrónico <xref:System.Threading.Tasks.Task.FromResult%2A> es un marcador de posición para una operación que devuelve una cadena.
  
:::code language="csharp" source="./snippets/async-returns1.cs" id="SnippetFirstExample":::

Cuando se llama a `GetLeisureHours` desde una expresión await en el método `ShowTodaysInfo`, esta recupera el valor entero (el valor de `leisureHours`) que está almacenado en la tarea que devuelve el método `GetLeisureHours`. Para más información sobre las expresiones await, vea [await](../../../language-reference/operators/await.md).  
  
Puede comprender mejor cómo `await` recupera el resultado de `Task<T>` si separa la llamada a `GetLeisureHours` de la aplicación de `await`, como se muestra en el código siguiente. Una llamada al método `GetLeisureHours` que no se espera inmediatamente devuelve `Task<int>`, como se podría esperar de la declaración del método. La tarea se asigna a la variable `integerTask` en el ejemplo. Dado que `integerTask` es <xref:System.Threading.Tasks.Task%601>, contiene una propiedad <xref:System.Threading.Tasks.Task%601.Result> de tipo `TResult`. En este caso, `TResult` representa un tipo entero. Cuando `await` se aplica a `integerTask`, la expresión await se evalúa en el contenido de la propiedad <xref:System.Threading.Tasks.Task%601.Result%2A> de `integerTask`. El valor se asigna a la variable `ret`.  
  
> [!IMPORTANT]
> La propiedad <xref:System.Threading.Tasks.Task%601.Result%2A> es una propiedad de bloqueo. Si se intenta acceder a ella antes de que termine su tarea, se bloquea el subproceso que está activo actualmente hasta que finaliza la tarea y el valor está disponible. En la mayoría de los casos, se debe tener acceso al valor usando `await` en lugar de tener acceso directamente a la propiedad. <br/> En el ejemplo anterior se ha recuperado el valor de la propiedad <xref:System.Threading.Tasks.Task%601.Result%2A> para bloquear el subproceso principal de manera que el método `ShowTodaysInfo` pueda terminar la ejecución antes de que finalice la aplicación.  

:::code language="csharp" source="./snippets/async-returns1a.cs" id="SnippetSecondVersion":::

## <a name="task-return-type"></a>Tipo de valor devuelto Task  
Los métodos asincrónicos que no contienen una instrucción `return` o que contienen una instrucción `return` que no devuelve un operando tienen normalmente un tipo de valor devuelto de <xref:System.Threading.Tasks.Task>. Dichos métodos devuelven `void` si se ejecutan de manera sincrónica. Si se usa un tipo de valor devuelto <xref:System.Threading.Tasks.Task> para un método asincrónico, un método de llamada puede usar un operador `await` para suspender la finalización del llamador hasta que finalice el método asincrónico llamado.  
  
En el ejemplo siguiente, el método asincrónico `WaitAndApologize` no contiene una instrucción `return`, de manera que el método devuelve un objeto <xref:System.Threading.Tasks.Task>. La devolución de `Task` permite que se espere a `WaitAndApologize`. El tipo <xref:System.Threading.Tasks.Task> no incluye una propiedad `Result` porque no tiene ningún valor devuelto.  

:::code language="csharp" source="./snippets/async-returns2.cs" id="SnippetTaskReturn":::

Se espera a `WaitAndApologize` mediante una instrucción await en lugar de una expresión await, similar a la instrucción de llamada para un método sincrónico que devuelve void. En este caso, la aplicación de un operador await no genera un valor.  
  
Igual que en el ejemplo <xref:System.Threading.Tasks.Task%601> anterior, puede separar la llamada a `WaitAndApologize` desde la aplicación de un operador await, como muestra el código siguiente. Pero recuerde que una `Task` no tiene una propiedad `Result` y que no se genera ningún valor cuando se aplica un operador await a una `Task`.  
  
El código siguiente separa la llamada del método `WaitAndApologize` de la espera de la tarea que el método devuelve.  

:::code language="csharp" source="./snippets/async-returns2a.cs" id="SnippetAwaitTask":::

## <a name="void-return-type"></a>Tipo de valor devuelto Void

Usa el tipo de valor devuelto `void` en controladores de eventos asincrónicos, que necesitan un tipo de valor devuelto `void`. Para métodos que no sean controladores de eventos que no devuelven un valor, debe devolver <xref:System.Threading.Tasks.Task> en su lugar, porque no se espera a un método asincrónico que devuelva `void`. Cualquiera que realice la llamada a este método debe continuar hasta completarse sin esperar a que finalice el método asincrónico que se haya llamado. El autor de la llamada debe ser independiente de los valores o las excepciones que genere el método asincrónico.  
  
El autor de la llamada de un método asincrónico que devuelve void no puede capturar las excepciones iniciadas desde el método y es probable que esas excepciones sin controlar provoquen un error de la aplicación. Si un método que devuelve un valor <xref:System.Threading.Tasks.Task> o <xref:System.Threading.Tasks.Task%601> inicia una excepción, la excepción se almacena en la tarea devuelta. La excepción se vuelve a iniciar cuando se espera a la tarea. Por tanto, asegúrese de que cualquier método asincrónico que puede producir una excepción tiene un tipo de valor devuelto de <xref:System.Threading.Tasks.Task> o <xref:System.Threading.Tasks.Task%601> y que se esperan llamadas al método.  
  
Para obtener más información sobre cómo capturar excepciones en métodos asincrónicos, vea la sección [Excepciones en métodos asincrónicos](../../../language-reference/keywords/try-catch.md#exceptions-in-async-methods) del artículo [try-catch](../../../language-reference/keywords/try-catch.md).  
  
En el ejemplo siguiente se muestra el comportamiento de un controlador de eventos asincrónicos. En el código de ejemplo, un controlador de eventos asincrónicos debe informar al subproceso principal de que ha terminado. Después, el subproceso principal puede esperar a que un controlador de eventos asincrónicos finalice antes de salir del programa.

:::code language="csharp" source="./snippets/async-returns3.cs":::

## <a name="generalized-async-return-types-and-valuetasktresult"></a>Tipos de valor devueltos asincrónicos generalizados y ValueTask\<TResult\>

A partir de C# 7.0, un método asincrónico puede devolver cualquier tipo que tenga un método `GetAwaiter` accesible.

Como <xref:System.Threading.Tasks.Task> y <xref:System.Threading.Tasks.Task%601> son tipos de referencia, la asignación de memoria en las rutas críticas para el rendimiento, especialmente cuando las asignaciones se producen en ajustados bucles, puede afectar negativamente al rendimiento. La compatibilidad para los tipos de valor devuelto generalizados significa que puede devolver un tipo de valor ligero en lugar de un tipo de referencia para evitar asignaciones de memoria adicionales.

.NET proporciona la estructura <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> como una implementación ligera de un valor de devolución de tareas generalizado. Para usar el tipo <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType>, debe agregar el paquete NuGet de `System.Threading.Tasks.Extensions` a su proyecto. En el ejemplo siguiente se usa la estructura <xref:System.Threading.Tasks.ValueTask%601> para recuperar el valor de dos tiradas de dado.
  
:::code language="csharp" source="./snippets/async-valuetask.cs":::

## <a name="async-streams-with-iasyncenumerablet"></a>Secuencias asincrónicas con IAsyncEnumerable\<T\>

A partir de C# 8.0, un método asincrónico puede devolver una *secuencia asincrónica*, representada por <xref:System.Collections.Generic.IAsyncEnumerable%601>. Una secuencia asincrónica proporciona una manera de enumerar los elementos leídos de una secuencia cuando se generan elementos en fragmentos con llamadas asincrónicas repetidas. En el ejemplo siguiente se muestra un método asincrónico que genera una secuencia asincrónica:

:::code language="csharp" source="./snippets/AsyncStreams.cs" id="SnippetGenerateAsyncStream":::

En el ejemplo anterior, las líneas de una cadena se leen de forma asincrónica. Una vez que se ha leído cada línea, el código enumera cada palabra de la cadena. Los autores de la llamada enumerarían cada palabra mediante la instrucción `await foreach`. El método espera cuando necesita leer de forma asincrónica la línea siguiente de la cadena de origen.

## <a name="see-also"></a>Vea también

- <xref:System.Threading.Tasks.Task.FromResult%2A>
- [Tutorial: Acceso a web usando Async y Await (C#)](./walkthrough-accessing-the-web-by-using-async-and-await.md)
- [Controlar el flujo en los programas asincrónicos (C#)](./control-flow-in-async-programs.md)
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
