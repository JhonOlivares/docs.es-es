---
title: Estructura DacpMethodDescData
ms.date: 02/01/2019
api.name:
- DacpMethodDescData Structure
api.location:
- mscordacwks.dll
api.type:
- COM
f1.keywords:
- DacpMethodDescData Structure
helpviewer.keywords:
- DacpMethodDescData Structure [.NET Framework debugging]
topic_type:
- apiref
author: hoyosjs
ms.author: juhoyosa
ms.openlocfilehash: cc54664ea8ad61005de3f3fae7407946d1c861b2
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76793845"
---
# <a name="dacpmethoddescdata-structure"></a>Estructura DacpMethodDescData

Define un búfer de transporte para la información de tiempo de ejecución de un método.

[!INCLUDE[debugging-api-recommended-note](../../../../includes/debugging-api-recommended-note.md)]

## <a name="syntax"></a>Sintaxis

```cpp
struct DacpMethodDescData
{
    int             bHasNativeCode;
    int             bIsDynamic;
    unsigned short  wSlotNumber;
    CLRDATA_ADDRESS NativeCodeAddr;
    CLRDATA_ADDRESS data;
    CLRDATA_ADDRESS MethodDescPtr;
    CLRDATA_ADDRESS nativeCodeInfo;
    CLRDATA_ADDRESS moduleInfo;
    mdToken         MDToken;
    CLRDATA_ADDRESS payloadGC;
    CLRDATA_ADDRESS payloadGC2;
    CLRDATA_ADDRESS managedDynamicMethodObject;
    CLRDATA_ADDRESS requestedIP;
    DacpReJitData   rejitDataCurrent;
    DacpReJitData   rejitDataRequested;
    unsigned long   cJittedRejitVersions;
};
```

## <a name="members"></a>Miembros

| Miembro                       | Descripción                                                                                     |
| ---------------------------- | ----------------------------------------------------------------------------------------------- |
| `bHasNativeCode`             | Indica si el tiempo de ejecución tiene código nativo disponible para la creación de instancias dada del método. |
| `bIsDynamic`                 | Indica si el método se genera dinámicamente a través de la generación de código ligero.           |
| `wSlotNumber`                | El número de ranura del método en la tabla de métodos.                                                   |
| `NativeCodeAddr`             | Dirección nativa inicial del método.                                                            |
| `data`                       | Puntero a un búfer utilizado internamente por el motor en tiempo de ejecución.                                             |
| `MethodDescPtr`              | Puntero al `MethodDesc` en tiempo de ejecución.                                                     |
| `nativeCodeInfo`             | Puntero a un búfer utilizado internamente por el motor en tiempo de ejecución para realizar el seguimiento de los métodos.                            |
| `moduleInfo`                 | Puntero a un búfer utilizado internamente por el motor en tiempo de ejecución para obtener información del módulo.                      |
| `MDToken`                    | Token asociado al método especificado.                                                         |
| `payloadGC`                  | Puntero a un búfer de recolección de elementos no utilizados que el Runtime usa internamente.                          |
| `payloadGC2`                 | Puntero a un búfer de recolección de elementos no utilizados que el Runtime usa internamente.                          |
| `managedDynamicMethodObject` | Si el método es dinámico, el motor en tiempo de ejecución utiliza este búfer internamente para realizar el seguimiento de la información.     |
| `requestedIP`                | Se usa para rellenar la estructura por solicitud cuando se proporciona una dirección de código nativo.                    |
| `rejitDataCurrent`           | Información sobre la última versión instrumentada del método.                                   |
| `rejitDataRequested`         | Rejit información de la dirección nativa solicitada.                                             |
| `cJittedRejitVersions`       | Número de veces que se ha rejitted el método a través de la instrumentación.                           |

## <a name="remarks"></a>Notas

Esta estructura reside dentro del tiempo de ejecución y no se expone a través de los encabezados o archivos de biblioteca. Para usarlo, defina la estructura tal y como se especificó anteriormente.

## <a name="requirements"></a>Requisitos de
**Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
**Encabezado:** Ninguna  
**Biblioteca:** Ninguna  
**.NET Framework versiones:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]  

## <a name="see-also"></a>Vea también

- [Depuración](index.md)
- [Estructuras de depuración](debugging-structures.md)
- [Tipos de datos comunes](../../../../docs/framework/unmanaged-api/common-data-types-unmanaged-api-reference.md)
