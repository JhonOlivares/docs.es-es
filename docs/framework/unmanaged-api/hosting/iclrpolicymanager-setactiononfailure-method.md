---
title: ICLRPolicyManager::SetActionOnFailure (Método)
ms.date: 03/30/2017
api_name:
- ICLRPolicyManager.SetActionOnFailure
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- ICLRPolicyManager::SetActionOnFailure
helpviewer_keywords:
- SetActionOnFailure method [.NET Framework hosting]
- ICLRPolicyManager::SetActionOnFailure method [.NET Framework hosting]
ms.assetid: 4664033f-db97-4388-b988-2ec470796e58
topic_type:
- apiref
ms.openlocfilehash: 143052febe136e969987c35bc06f6c3b3356aedf
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73140782"
---
# <a name="iclrpolicymanagersetactiononfailure-method"></a>ICLRPolicyManager::SetActionOnFailure (Método)
Especifica la acción de la Directiva que debe realizar el Common Language Runtime (CLR) cuando se produce el error especificado.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
HRESULT SetActionOnFailure (  
    [in] EClrFailure   failure,  
    [in] EPolicyAction action  
);  
```  
  
## <a name="parameters"></a>Parámetros  
 `failure`  
 de Uno de los valores de [eclrfailure (](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md) , que indica el tipo de error para el que se realizará la acción.  
  
 `action`  
 de Uno de los valores de [EPolicyAction](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md) , que indica la acción que se debe realizar cuando se produce un error. Para obtener una lista de valores admitidos, vea la sección Comentarios.  
  
## <a name="return-value"></a>Valor devuelto  
  
|HRESULT|Descripción|  
|-------------|-----------------|  
|S_OK|`SetActionOnFailure` devolvió correctamente.|  
|HOST_E_CLRNOTAVAILABLE|CLR no se ha cargado en un proceso o CLR está en un estado en el que no puede ejecutar código administrado ni procesar la llamada correctamente.|  
|HOST_E_TIMEOUT|Se agotó el tiempo de espera de la llamada.|  
|HOST_E_NOT_OWNER|El autor de la llamada no posee el bloqueo.|  
|HOST_E_ABANDONED|Se canceló un evento mientras un subproceso o fibra bloqueados estaba esperando en él.|  
|E_FAIL|Se produjo un error grave desconocido. Una vez que un método devuelve E_FAIL, el CLR ya no se puede usar en el proceso. Las llamadas subsiguientes a métodos de hospedaje devuelven HOST_E_CLRNOTAVAILABLE.|  
|E_INVALIDARG|No se puede establecer una acción de directiva para la operación especificada o se especificó una acción de Directiva no válida para la operación.|  
  
## <a name="remarks"></a>Comentarios  
 De forma predeterminada, CLR produce una excepción cuando no puede asignar un recurso como memoria. `SetActionOnFailure` permite que el host Invalide este comportamiento especificando la acción de la Directiva que se realizará en caso de error. En la tabla siguiente se muestran las combinaciones de los valores [eclrfailure (](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md) y [EPolicyAction](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md) que se admiten. (El prefijo FAIL_ se omite de los valores [eclrfailure (](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md) ).  
  
||NonCriticalResource|CriticalResource|FatalRuntime|OrphanedLock|StackOverflow|AccessViolation|CodeContract|  
|-|-------------------------|----------------------|------------------|------------------|-------------------|---------------------|------------------|  
|`eNoAction`|x|x||||N/D||  
|eThrowException|x|x||||N/D||  
|`eAbortThread`|x|x||||N/D|x|  
|`eRudeAbortThread`|x|x||||N/D|x|  
|`eUnloadAppDomain`|x|x||x||N/D|x|  
|`eRudeUnloadAppDomain`|x|x||x|x|N/D|x|  
|`eExitProcess`|x|x||x|x|N/D|x|  
|eFastExitProcess|x|x||x|x|N/D||  
|`eRudeExitProcess`|x|x|x|x|x|N/D||  
|`eDisableRuntime`|x|x|x|x|x|N/D||  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** MSCorEE. h  
  
 **Biblioteca:** Se incluye como recurso en MSCorEE. dll  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [EClrFailure (enumeración)](../../../../docs/framework/unmanaged-api/hosting/eclrfailure-enumeration.md)
- [EPolicyAction (enumeración)](../../../../docs/framework/unmanaged-api/hosting/epolicyaction-enumeration.md)
- [ICLRControl (interfaz)](../../../../docs/framework/unmanaged-api/hosting/iclrcontrol-interface.md)
- [ICLRPolicyManager (interfaz)](../../../../docs/framework/unmanaged-api/hosting/iclrpolicymanager-interface.md)
