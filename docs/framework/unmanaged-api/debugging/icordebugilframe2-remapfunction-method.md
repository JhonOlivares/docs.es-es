---
title: ICorDebugILFrame2::RemapFunction (Método)
ms.date: 03/30/2017
api_name:
- ICorDebugILFrame2.RemapFunction
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugILFrame2::RemapFunction
helpviewer_keywords:
- ICorDebugILFrame2::RemapFunction method [.NET Framework debugging]
- RemapFunction method [.NET Framework debugging]
ms.assetid: dd639ba0-f77b-426d-9ff6-f92706840348
topic_type:
- apiref
ms.openlocfilehash: f4f73b99b4cb48690a2a8611dbf5a5420adab5d4
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794352"
---
# <a name="icordebugilframe2remapfunction-method"></a>ICorDebugILFrame2::RemapFunction (Método)
Reasigna una función editada especificando el nuevo desplazamiento del lenguaje intermedio de Microsoft (MSIL).  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
HRESULT RemapFunction (  
    [in] ULONG32      newILOffset  
);  
```  
  
## <a name="parameters"></a>Parameters  
 `newILOffset`  
 de Nuevo desplazamiento de MSIL del marco de pila en el que se debe colocar el puntero de instrucción. Este valor debe ser un punto de secuencia.  
  
 Es responsabilidad del autor de la llamada garantizar la validez de este valor. Por ejemplo, el desplazamiento de MSIL no es válido si está fuera de los límites de la función.  
  
## <a name="remarks"></a>Notas  
 Cuando se ha editado la función de un fotograma, el depurador puede llamar al método `RemapFunction` para intercambiar en la última versión de la función del marco de modo que se pueda ejecutar. La ejecución del código comenzará en el desplazamiento de MSIL especificado.  
  
> [!NOTE]
> La llamada a `RemapFunction`, como llamar a [ICorDebugILFrame:: setip](icordebugilframe-setip-method.md), invalidará inmediatamente todas las interfaces de depuración relacionadas con la generación de un seguimiento de la pila para el subproceso. Estas interfaces incluyen [ICorDebugChain](icordebugchain-interface.md), ICorDebugILFrame, ICorDebugInternalFrame (e ICorDebugNativeFrame.  
  
 Solo se puede llamar al método `RemapFunction` en el contexto del marco actual y solo en uno de los siguientes casos:  
  
- Después de la recepción de una devolución de llamada [ICorDebugManagedCallback2:: functionremapopportunity (](icordebugmanagedcallback2-functionremapopportunity-method.md) que todavía no se ha continuado.  
  
- Mientras se detiene la ejecución del código debido a un evento [ICorDebugManagedCallback:: editandcontinueremap (](icordebugmanagedcallback-editandcontinueremap-method.md) para este marco.  
  
## <a name="requirements"></a>Requisitos de  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** CorDebug.idl, CorDebug.h  
  
 **Biblioteca:** CorGuids.lib  
  
 **.NET Framework versiones:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]
