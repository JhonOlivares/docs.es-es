---
title: IMetaDataImport::GetCustomAttributeProps (Método)
ms.date: 03/30/2017
api_name:
- IMetaDataImport.GetCustomAttributeProps
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- IMetaDataImport::GetCustomAttributeProps
helpviewer_keywords:
- GetCustomAttributeProps method [.NET Framework metadata]
- IMetaDataImport::GetCustomAttributeProps method [.NET Framework metadata]
ms.assetid: 6eefb243-a281-41c1-bcdc-7e17513bc446
topic_type:
- apiref
ms.openlocfilehash: 9a80336db4a5a8d7cfdebb7eb8d25bcb8f96e87c
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/23/2019
ms.locfileid: "74437648"
---
# <a name="imetadataimportgetcustomattributeprops-method"></a>IMetaDataImport::GetCustomAttributeProps (Método)
Obtiene el valor del atributo personalizado a partir de su token de metadatos.  
  
## <a name="syntax"></a>Sintaxis  
  
```cpp  
HRESULT GetCustomAttributeProps (  
   [in]            mdCustomAttribute   cv,  
   [out, optional] mdToken             *ptkObj,  
   [out, optional] mdToken             *ptkType,  
   [out, optional] void const          **ppBlob,  
   [out, optional] ULONG               *pcbSize  
);  
```  
  
## <a name="parameters"></a>Parámetros  
 `cv`  
 [in] Token de metadatos que representa el atributo personalizado que se va a recuperar.  
  
 `ptkObj`  
 [out, optional] Token de metadatos que representa el objeto que es modificado por el atributo personalizado. Este valor puede ser cualquier tipo de token de metadatos, excepto `mdCustomAttribute`.  
  
 `ptkType`  
 [out, optional] Token de metadatos `mdMethodDef` o `mdMemberRef` que representa el objeto <xref:System.Type> del atributo personalizado devuelto.  
  
 `ppBlob`  
 [out, optional] Puntero a una matriz de datos que es el valor del atributo personalizado.  
  
 `pcbSize`  
 [out, optional] Tamaño en bytes de los datos devueltos en *`ppBlob`.  
  
## <a name="remarks"></a>Comentarios  
 Un atributo personalizado se almacena como una matriz de datos, que es el formato que reconoce el motor de metadatos.  
  
## <a name="requirements"></a>Requisitos  
 **Plataformas:** Vea [Requisitos de sistema](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Encabezado:** Cor. h  
  
 **Biblioteca:** Se incluye como recurso en MsCorEE. dll  
  
 **Versiones de .NET Framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>Vea también

- [IMetaDataImport (interfaz)](../../../../docs/framework/unmanaged-api/metadata/imetadataimport-interface.md)
- [IMetaDataImport2 (interfaz)](../../../../docs/framework/unmanaged-api/metadata/imetadataimport2-interface.md)
