---
title: Procedimiento Definir un ámbito de nombres
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- name scope [WPF], defining
- Storyboards [WPF], animating in procedural code
- animation [WPF], Storyboards [WPF], in procedural code
ms.assetid: 4f361925-6a08-40dc-8231-a61111c6b28b
ms.openlocfilehash: a03f477dd31909e8cb9dde9cd29da6f38d665758
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61904155"
---
# <a name="how-to-define-a-name-scope"></a>Procedimiento Definir un ámbito de nombres
Para animar con <xref:System.Windows.Media.Animation.Storyboard> en código, debe crear un <xref:System.Windows.NameScope> y registre los nombres de los objetos de destino con el elemento que posee ese ámbito de nombres. En el ejemplo siguiente, un <xref:System.Windows.NameScope> se crea para `myMainPanel`. Dos botones, `button1` y `button2`, se agregan a la pantalla y sus nombres registrados. Varias animaciones y un <xref:System.Windows.Media.Animation.Storyboard> se crean. Del guión gráfico <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> método se usa para iniciar las animaciones.  
  
 Dado que `button1`, `button2`, y `myMainPanel` todas comparten el mismo ámbito de nombres, se puede utilizar cualquiera de ellos con el <xref:System.Windows.Media.Animation.Storyboard> <xref:System.Windows.Media.Animation.Storyboard.Begin%2A> método para iniciar las animaciones.  
  
## <a name="example"></a>Ejemplo  
 [!code-csharp[StoryboardBeginAnimation_procedural_snip#NameScopeExample](~/samples/snippets/csharp/VS_Snippets_Wpf/StoryboardBeginAnimation_procedural_snip/CSharp/ScopeExample.cs#namescopeexample)]
 [!code-vb[StoryboardBeginAnimation_procedural_snip#NameScopeExample](~/samples/snippets/visualbasic/VS_Snippets_Wpf/StoryboardBeginAnimation_procedural_snip/visualbasic/scopeexample.vb#namescopeexample)]  
  
## <a name="see-also"></a>Vea también

- [Animar una propiedad utilizando un guión gráfico](how-to-animate-a-property-by-using-a-storyboard.md)
- [Información general sobre animaciones](animation-overview.md)
