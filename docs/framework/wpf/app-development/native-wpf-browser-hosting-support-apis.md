---
title: API de soporte de hospedaje de navegadores nativos
ms.date: 03/30/2017
f1_keywords:
- AutoGeneratedOrientationPage
helpviewer_keywords:
- browser hosting support [WPF]
- WPF browser hosting support APIs [WPF]
ms.assetid: 82c133a8-d760-45fb-a2b9-3a997537f1d4
ms.openlocfilehash: 514b93423ddb029413b6f7b05241cdde88a80fd4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2020
ms.locfileid: "79186893"
---
# <a name="native-wpf-browser-hosting-support-apis"></a>API para el hospedaje nativo de WPF en explorador
El hospedaje de aplicaciones WPF en exploradores Web se facilita mediante un servidor de documentos activos (también conocido como DocObject) registrado fuera del host WPF. Internet Explorer puede activarse e integrarse directamente con un documento activo. Para hospedar XBAPs y documentos XAML sueltos en los exploradores de Mozilla, WPFWPF proporciona un complemento NPAPI, que proporciona un entorno de hospedaje similar al servidor de documentos activos de WPF como lo hace Internet Explorer. Sin embargo, la forma práctica más fácil de hospedar XBAPs y documentos XAML en otros exploradores y aplicaciones independientes es a través del control Explorador web de Internet Explorer. El control Explorador web proporciona el complejo entorno de hospedaje del servidor de documentos activos, pero permite que su propio host personalice y amplíe ese entorno y se comunique directamente con el objeto de documento activo actual.  
  
 El servidor WPF Active Document implementa varias interfaces de hospedaje comunes, como [IOleObject](/windows/win32/api/oleidl/nn-oleidl-ioleobject), [IOleDocument](/windows/win32/api/docobj/nn-docobj-ioledocument), [IOleInPlaceActiveObject](/windows/win32/api/oleidl/nn-oleidl-ioleinplaceactiveobject), [IPersistMoniker](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms775042(v=vs.85)), [IOleCommandTarget](/windows/win32/api/docobj/nn-docobj-iolecommandtarget). Cuando se hospedan en el control Explorador Web, estas interfaces pueden ser consultas desde el objeto devuelto por la propiedad [IWebBrowser2::Document.](https://docs.microsoft.com/previous-versions/aa752116(v=vs.85))  
  
## <a name="iolecommandtarget"></a>IOleCommandTarget  
 Wpf Active Document Server implementación de [IOleCommandTarget](/windows/win32/api/docobj/nn-docobj-iolecommandtarget) admite numerosos comandos relacionados con la navegación y específicos del explorador del grupo de comandos OLE estándar (con un GUID de grupo de comandos nulo). Además, reconoce un grupo de comandos personalizado denominado CGID_PresentationHost. Actualmente, solo hay un comando definido dentro de este grupo.  
  
```cpp  
DEFINE_GUID(CGID_PresentationHost, 0xd0288c55, 0xd6, 0x4f5e, 0xa8, 0x51, 0x79, 0xde, 0xc5, 0x1b, 0x10, 0xec);  
enum PresentationHostCommands {
   PHCMDID_TABINTO = 1
};  
```  
  
 PHCMDID_TABINTO indica PresentationHost para cambiar el foco al primer o último elemento enfocable en su contenido, dependiendo del estado de la tecla Mayús.  
  
## <a name="in-this-section"></a>En esta sección  
 [IEnumRAWINPUTDEVICE](ienumrawinputdevice.md)  
 [IWpfHostSupport](iwpfhostsupport.md)
