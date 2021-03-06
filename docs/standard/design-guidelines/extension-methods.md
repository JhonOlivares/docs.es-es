---
title: Métodos de extensión
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: 5de945cb-88f4-49d7-b0e6-f098300cf357
ms.openlocfilehash: 13f92470b23d138e0d30bed947ff1932f2605d28
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741622"
---
# <a name="extension-methods"></a>Métodos de extensión
Los métodos de extensión son una característica de lenguaje que permite llamar a métodos estáticos mediante la sintaxis de llamada al método de instancia. Estos métodos deben tomar al menos un parámetro, que representa la instancia en la que se va a operar el método.

 La clase que define estos métodos de extensión se conoce como la clase "patrocinador" y se debe declarar como estática. Para usar métodos de extensión, debe importar el espacio de nombres que define la clase de patrocinador.

 ❌ evitar la definición de métodos de extensión frivolously, especialmente en tipos que no son de su propiedad.

 Si hace su propio código fuente de un tipo, considere la posibilidad de usar métodos de instancia normales en su lugar. Si no es de su propiedad y desea agregar un método, tenga mucho cuidado. El uso liberal de los métodos de extensión tiene el potencial de saturar las API de los tipos que no se diseñaron para tener estos métodos.

 ✔️ considere la posibilidad de usar métodos de extensión en cualquiera de los escenarios siguientes:

- Para proporcionar una funcionalidad auxiliar relevante para cada implementación de una interfaz, si se puede escribir la funcionalidad en términos de la interfaz principal. Esto se debe a que, de lo contrario, las implementaciones concretas no se pueden asignar a interfaces. Por ejemplo, los operadores de `LINQ to Objects` se implementan como métodos de extensión para todos los tipos de <xref:System.Collections.Generic.IEnumerable%601>. Por lo tanto, cualquier implementación de `IEnumerable<>` se habilita automáticamente para LINQ.

- Cuando un método de instancia introduciría una dependencia en algún tipo, pero dicha dependencia interrumpiría las reglas de administración de dependencias. Por ejemplo, es probable que una dependencia de <xref:System.String> a <xref:System.Uri?displayProperty=nameWithType> no sea deseable, por lo que `String.ToUri()` método de instancia que devuelve `System.Uri` sería el diseño equivocado desde una perspectiva de administración de dependencias. Un método de extensión estático `Uri.ToUri(this string str)` devolver `System.Uri` sería un diseño mucho mejor.

 ❌ Evite definir métodos de extensión en <xref:System.Object?displayProperty=nameWithType>.

 Visual Basic usuarios no podrán llamar a estos métodos en referencias a objetos mediante la sintaxis del método de extensión. Visual Basic no admite la llamada a tales métodos porque, en Visual Basic, la declaración de una referencia como objeto obliga a todas las invocaciones de método en que se enlazan en tiempo de ejecución (el miembro real llamado se determina en tiempo de ejecución), mientras que los enlaces a los métodos de extensión se determinan en tiempo de compilación (enlazado en tiempo de compilación).

 Tenga en cuenta que la instrucción se aplica a otros idiomas en los que está presente el mismo comportamiento de enlace o donde no se admiten los métodos de extensión.

 ❌ no colocan métodos de extensión en el mismo espacio de nombres que el tipo extendido a menos que sea para agregar métodos a interfaces o para la administración de dependencias.

 ❌ Evite definir dos o más métodos de extensión con la misma firma, aunque residan en espacios de nombres diferentes.

 ✔️ considere la posibilidad de definir métodos de extensión en el mismo espacio de nombres que el tipo extendido si el tipo es una interfaz y si los métodos de extensión están diseñados para usarse en la mayoría o en todos los casos.

 ❌ no definen métodos de extensión que implementan una característica de en los espacios de nombres normalmente asociados a otras características de. En su lugar, se deben definir en el espacio de nombres asociado a la característica a la que pertenecen.

 ❌ evitar nombres genéricos de espacios de nombres dedicados a métodos de extensión (por ejemplo, "extensiones"). En su lugar, use un nombre descriptivo (por ejemplo, "enrutamiento").

 *Partes © 2005, 2009 Microsoft Corporation. Todos los derechos reservados.*

 *Material reimpreso con el consentimiento de Pearson Education, Inc. y extraído de [Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) (Instrucciones de diseño de .NET Framework: convenciones, expresiones y patrones para bibliotecas .NET reutilizables, 2.ª edición), de Krzysztof Cwalina y Brad Abrams, publicado el 22 de octubre de 2008 por Addison-Wesley Professional como parte de la serie Microsoft Windows Development.*

## <a name="see-also"></a>Consulte también

- [Instrucciones de diseño de miembros](../../../docs/standard/design-guidelines/member.md)
- [Instrucciones de diseño de .NET Framework](../../../docs/standard/design-guidelines/index.md)
