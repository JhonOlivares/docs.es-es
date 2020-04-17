---
ms.openlocfilehash: 8e077d5cde6e824af5aac3742308593f1d91dd92
ms.sourcegitcommit: a9b8945630426a575ab0a332e568edc807666d1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391187"
---
### <a name="static-files-csv-content-type-changed-to-standards-compliant"></a><span data-ttu-id="d7642-101">Archivos estáticos: tipo de contenido CSV cambiado a compatible con los estándares</span><span class="sxs-lookup"><span data-stu-id="d7642-101">Static files: CSV content type changed to standards-compliant</span></span>

<span data-ttu-id="d7642-102">En ASP.NET Core 5.0, el valor de encabezado de respuesta `Content-Type` predeterminado que usa el [Middleware de archivo estático](/aspnet/core/fundamentals/static-files) para archivos *.csv* ha cambiado al valor `text/csv` compatible con los estándares.</span><span class="sxs-lookup"><span data-stu-id="d7642-102">In ASP.NET Core 5.0, the default `Content-Type` response header value that the [Static File Middleware](/aspnet/core/fundamentals/static-files) uses for *.csv* files has changed to the standards-compliant value `text/csv`.</span></span>

<span data-ttu-id="d7642-103">Para obtener información sobre esta incidencia, vea [dotnet/aspnetcore#17385](https://github.com/dotnet/AspNetCore/issues/17385).</span><span class="sxs-lookup"><span data-stu-id="d7642-103">For discussion on this issue, see [dotnet/aspnetcore#17385](https://github.com/dotnet/AspNetCore/issues/17385).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="d7642-104">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="d7642-104">Version introduced</span></span>

<span data-ttu-id="d7642-105">5.0 (versión preliminar 1)</span><span class="sxs-lookup"><span data-stu-id="d7642-105">5.0 Preview 1</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="d7642-106">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="d7642-106">Old behavior</span></span>

<span data-ttu-id="d7642-107">Se usó el valor de encabezado `Content-Type` `application/octet-stream`.</span><span class="sxs-lookup"><span data-stu-id="d7642-107">The `Content-Type` header value `application/octet-stream` was used.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="d7642-108">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="d7642-108">New behavior</span></span>

<span data-ttu-id="d7642-109">Se usa el valor de encabezado `Content-Type` `text/csv`.</span><span class="sxs-lookup"><span data-stu-id="d7642-109">The `Content-Type` header value `text/csv` is used.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="d7642-110">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="d7642-110">Reason for change</span></span>

<span data-ttu-id="d7642-111">Cumplimiento con el estándar [RFC 7111](https://tools.ietf.org/html/rfc7111#section-5.1).</span><span class="sxs-lookup"><span data-stu-id="d7642-111">Compliance with the [RFC 7111](https://tools.ietf.org/html/rfc7111#section-5.1) standard.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="d7642-112">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="d7642-112">Recommended action</span></span>

<span data-ttu-id="d7642-113">Si este cambio afecta a la aplicación, se puede personalizar la extensión del archivo a una asignación de tipo MIME.</span><span class="sxs-lookup"><span data-stu-id="d7642-113">If this change impacts your app, you can customize the file extension-to-MIME type mapping.</span></span> <span data-ttu-id="d7642-114">Para revertir al tipo MIME `application/octet-stream`, modifique la llamada de método <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A> en `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d7642-114">To revert to the `application/octet-stream` MIME type, modify the <xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A> method call in `Startup.Configure`.</span></span> <span data-ttu-id="d7642-115">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="d7642-115">For example:</span></span>

```csharp
var provider = new FileExtensionContentTypeProvider();
provider.Mappings[".csv"] = MediaTypeNames.Application.Octet;

app.UseStaticFiles(new StaticFileOptions
{
    ContentTypeProvider = provider
});
```

<span data-ttu-id="d7642-116">Para obtener más información sobre la personalización de la asignación, vea [FileExtensionContentTypeProvider](/aspnet/core/fundamentals/static-files#fileextensioncontenttypeprovider).</span><span class="sxs-lookup"><span data-stu-id="d7642-116">For more information on customizing the mapping, see [FileExtensionContentTypeProvider](/aspnet/core/fundamentals/static-files#fileextensioncontenttypeprovider).</span></span>

#### <a name="category"></a><span data-ttu-id="d7642-117">Categoría</span><span class="sxs-lookup"><span data-stu-id="d7642-117">Category</span></span>

<span data-ttu-id="d7642-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7642-118">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="d7642-119">API afectadas</span><span class="sxs-lookup"><span data-stu-id="d7642-119">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.StaticFiles.FileExtensionContentTypeProvider?displayProperty=nameWithType>

<!--

#### Affected APIs

`T:Microsoft.AspNetCore.StaticFiles.FileExtensionContentTypeProvider`

-->