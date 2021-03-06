---
title: NotOverridable
ms.date: 07/20/2015
f1_keywords:
- vb.NotOverridable
- NotOverridable
helpviewer_keywords:
- sealed methods [Visual Basic]
- NotOverridable keyword [Visual Basic]
- properties [Visual Basic], redefining
- elements [Visual Basic], sealed
- sealed [elements VB]
- procedures [Visual Basic], overriding
- procedures [Visual Basic], redefining
- overriding
- methods [Visual Basic], sealed
- properties [Visual Basic], overriding
ms.assetid: 66ec6984-f5f5-4857-b362-6a3907aaf9e0
ms.openlocfilehash: c55d57bb3008b2825fe5382844908ea32f0d500c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351442"
---
# <a name="notoverridable-visual-basic"></a>NotOverridable (Visual Basic)
Especifica que una propiedad o un procedimiento no se pueden invalidar en una clase derivada.  
  
## <a name="remarks"></a>Comentarios  
 El modificador `NotOverridable` impide que una propiedad o un método se invalide en una clase derivada.  El modificador [Overridable](../../../visual-basic/language-reference/modifiers/overridable.md) permite que una propiedad o un método de una clase se invalide en una clase derivada. Para más información, vea [Fundamentos de la herencia](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md).  
  
 Si no se especifica el modificador `Overridable` o `NotOverridable`, el valor predeterminado depende de si la propiedad o el método reemplaza a una propiedad o método de clase base. Si la propiedad o el método reemplaza una propiedad o un método de clase base, la configuración predeterminada es `Overridable`; de lo contrario, se `NotOverridable`.  
  
 Un elemento que no se puede invalidar se denomina a veces elemento *sellado* .  
  
 Solo se puede usar `NotOverridable` en una instrucción de declaración de propiedad o procedimiento. Solo puede especificar `NotOverridable` en una propiedad o un procedimiento que invalide otra propiedad o procedimiento, es decir, solo en combinación con `Overrides`.  
  
## <a name="combined-modifiers"></a>Modificadores combinados  
 No se puede especificar `Overridable` ni `NotOverridable` para un método `Private`.  
  
 No se puede especificar `NotOverridable` junto con `MustOverride`, `Overridable`o `Shared` en la misma declaración.  
  
## <a name="usage"></a>Uso  
 El modificador `NotOverridable` se puede utilizar en los contextos siguientes:  
  
 [Function (instrucción)](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Property (instrucción)](../../../visual-basic/language-reference/statements/property-statement.md)  
  
 [Sub (instrucción)](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>Vea también

- [Modificadores](../../../visual-basic/language-reference/modifiers/index.md)
- [Fundamentos de la herencia](../../../visual-basic/programming-guide/language-features/objects-and-classes/inheritance-basics.md)
- [MustOverride](../../../visual-basic/language-reference/modifiers/mustoverride.md)
- [Overridable](../../../visual-basic/language-reference/modifiers/overridable.md)
- [Overrides](../../../visual-basic/language-reference/modifiers/overrides.md)
- [Palabras clave](../../../visual-basic/language-reference/keywords/index.md)
- [Sombrear en Visual Basic](../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
