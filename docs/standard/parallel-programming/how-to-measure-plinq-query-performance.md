---
title: Procedimiento para medir el rendimiento de consultas PLINQ
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to measure performance
ms.assetid: 491ba43b-2c10-473d-9aab-e2cb96446711
ms.openlocfilehash: 37bd3bc464f719876b2fe13ee1a11ebca4339988
ms.sourcegitcommit: 1c1a1f9ec0bd1efb3040d86a79f7ee94e207cca5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2020
ms.locfileid: "80635830"
---
# <a name="how-to-measure-plinq-query-performance"></a>Procedimiento para medir el rendimiento de consultas PLINQ

En este ejemplo se muestra cómo utilizar la clase <xref:System.Diagnostics.Stopwatch> para medir el tiempo que tarda en ejecutarse una consulta PLINQ.  
  
## <a name="example"></a>Ejemplo  
 Este ejemplo utiliza un bucle `foreach` vacío (`For Each` en Visual Basic) para medir el tiempo que tarda en ejecutarse la consulta. En el código real, el bucle normalmente contiene pasos de procesamiento adicional que se agregan al tiempo de ejecución de consulta total. Verá que el cronómetro no se inicia hasta justo antes del bucle, ya que ese es el momento en el que comienza la ejecución de las consultas. Si necesita una medición más específica, puede utilizar la propiedad `ElapsedTicks` en lugar de `ElapsedMilliseconds`.  
  
 [!code-csharp[PLINQ#19](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/measure2.cs#19)]
 [!code-vb[PLINQ#19](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/measure2.vb#19)]  
  
 El tiempo de ejecución total es una métrica útil cuando se experimenta con implementaciones de consulta, pero no siempre muestra todo el panorama. Para obtener una vista más detallada y completa de la interacción de los subprocesos de consulta entre sí y con otros procesos en ejecución, use el [Visualizador de simultaneidad](/visualstudio/profiling/concurrency-visualizer).  
  
## <a name="see-also"></a>Vea también

- [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/introduction-to-plinq.md)
