---
ms.openlocfilehash: c0be08023f80bf0f96cc08f34b9ea8c5a73839e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2020
ms.locfileid: "77466087"
---
### <a name="aspnet-validationcontextmembername-is-not-null-when-using-custom-dataannotationsvalidationattribute"></a>ValidationContext.MemberName de APS.NET no es NULL cuando se usa DataAnnotations.ValidationAttribute personalizado

|   |   |
|---|---|
|Detalles|En .NET Framework 4.7.2 y versiones anteriores, cuando se usa <xref:System.ComponentModel.DataAnnotations.ValidationAttribute?displayProperty=nameWithType> personalizado, la propiedad <xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType> devuelve `null`. En .NET Framework, versión 4.8, anterior a la actualización de octubre de 2019, devuelve el nombre del miembro. A partir de la [versión preliminar de .NET Framework de octubre de 2019 del paquete acumulativo de actualizaciones de calidad](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/) para .NET Framework 4.8, devuelve `null` de forma predeterminada, pero puede optar por devolver el nombre del miembro en su lugar. |
|Sugerencia|Agregue la siguiente configuración a su archivo *web.config* para que la propiedad devuelva el nombre de miembro en la [versión preliminar de .NET Framework de octubre de 2019 del paquete acumulativo de actualizaciones de calidad](https://devblogs.microsoft.com/dotnet/net-framework-october-2019-preview-of-quality-rollup/) para .NET Framework 4.8 y versiones posteriores:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:GetValidationMemberName&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>En .NET Framework, versión 4.8, anterior a la actualización de octubre de 2019, al agregarlo al archivo *web.config* se restaura el comportamiento anterior y la propiedad devuelve `null`.|
|Ámbito|Desconocido|
|Versión|4.8|
|Tipo|Tiempo de ejecución|
|API afectadas|<ul><li><xref:System.ComponentModel.DataAnnotations.ValidationContext.MemberName?displayProperty=nameWithType></li></ul>|
