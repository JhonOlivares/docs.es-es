---
title: Variables de objeto
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], about object variables
- variables [Visual Basic], object
- objects [Visual Basic], accessing
- object variables [Visual Basic]
ms.assetid: 6169a196-2b13-4ba5-a205-154bc1b87844
ms.openlocfilehash: 7eb860bc732f923316b8ce1d7b94ecdb368bfec3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351783"
---
# <a name="object-variables-in-visual-basic"></a>Variables de objeto en Visual Basic

Además de almacenar los valores directamente, una variable puede hacer referencia a un objeto. Un objeto se asigna a una variable por las mismas razones por las que se asigna cualquier valor a una variable:

- Un nombre de variable suele ser más corto y más fácil de recordar que la ruta de acceso completa de los métodos y las propiedades necesarios para acceder al propio objeto.

- El uso de una variable que hace referencia a un objeto es más eficaz que el acceso repetido al propio objeto a través de los métodos o propiedades necesarios.

- Puede cambiar una variable para hacer referencia a otros objetos mientras se ejecuta el código.

## <a name="making-code-shorter"></a>Crear código más corto

Puede usar variables de objeto para acortar el código que tiene que escribir. En el ejemplo siguiente se usa la ruta de acceso completa de los métodos y propiedades para tener acceso a un objeto <xref:System.Windows.Forms.Control>.

```vb
' Assume Me is a valid Form, or replace Me with a valid Form.
Me.ActiveForm.ActiveControl.Text = "Test"
Me.ActiveForm.ActiveControl.Location = New Point(100, 100)
Me.ActiveForm.ActiveControl.Show()
```

Puede acortar este código y acelerar la ejecución si usa una variable de objeto para el control. Debe declarar la variable de objeto con la clase específica que desee asignar a ella (`Control` en este caso). Una vez que se asigna un objeto a la variable, se puede tratar exactamente igual que el objeto al que hace referencia. Puede establecer o recuperar las propiedades del objeto o usar cualquiera de sus métodos. En el ejemplo siguiente se usa una variable de objeto para simplificar el código del ejemplo anterior.

```vb
Dim ctrlActv As System.Windows.Forms.Control = Me.ActiveForm.ActiveControl
ctrlActv.Text = "Test"
ctrlActv.Location = New Point(100, 100)
ctrlActv.Show()
```

## <a name="see-also"></a>Vea también

- [Declaración de variables](../../../../visual-basic/programming-guide/language-features/variables/variable-declaration.md)
- [Acelerar el acceso a un objeto con una ruta de acceso de calificación larga](../../../../visual-basic/programming-guide/language-features/variables/how-to-speed-up-access-to-an-object-with-a-long-qualification-path.md)
- [Declaración de variables de objeto](../../../../visual-basic/programming-guide/language-features/variables/object-variable-declaration.md)
- [Asignación de variables de objeto](../../../../visual-basic/programming-guide/language-features/variables/object-variable-assignment.md)
- [Valores de las variables de objeto](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
