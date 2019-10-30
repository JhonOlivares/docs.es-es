---
ms.openlocfilehash: 8d7942ef6c36c01a9ae7ae2a9739f26dfcda5813
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394360"
---
### <a name="identity-ui-uses-static-web-assets-feature"></a><span data-ttu-id="acbd9-101">Identity: la interfaz de usuario usa la característica de recursos web estáticos</span><span class="sxs-lookup"><span data-stu-id="acbd9-101">Identity: UI uses static web assets feature</span></span>

<span data-ttu-id="acbd9-102">ASP.NET Core 3.0 ha presentado una característica de recursos web estáticos y la interfaz de usuario de Identity la ha adoptado.</span><span class="sxs-lookup"><span data-stu-id="acbd9-102">ASP.NET Core 3.0 introduced a static web assets feature, and Identity UI has adopted it.</span></span>

#### <a name="change-description"></a><span data-ttu-id="acbd9-103">Descripción del cambio</span><span class="sxs-lookup"><span data-stu-id="acbd9-103">Change description</span></span>

<span data-ttu-id="acbd9-104">Como resultado de la interfaz de usuario de Identity que adopta la característica de recursos web estáticos:</span><span class="sxs-lookup"><span data-stu-id="acbd9-104">As a result of Identity UI adopting the static web assets feature:</span></span>

- <span data-ttu-id="acbd9-105">La selección del marco se realiza mediante el uso de la propiedad `IdentityUIFrameworkVersion` en el archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="acbd9-105">Framework selection is accomplished by using the `IdentityUIFrameworkVersion` property in your project file.</span></span>
- <span data-ttu-id="acbd9-106">Bootstrap 4 es el marco de la interfaz de usuario predeterminado para la interfaz de usuario de Identity.</span><span class="sxs-lookup"><span data-stu-id="acbd9-106">Bootstrap 4 is the default UI framework for Identity UI.</span></span> <span data-ttu-id="acbd9-107">Bootstrap 3 ha alcanzado el final del ciclo de vida y debe considerar la posibilidad de migrar a una versión compatible.</span><span class="sxs-lookup"><span data-stu-id="acbd9-107">Bootstrap 3 has reached end of life, and you should consider migrating to a supported version.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="acbd9-108">Versión introducida</span><span class="sxs-lookup"><span data-stu-id="acbd9-108">Version introduced</span></span>

<span data-ttu-id="acbd9-109">3.0</span><span class="sxs-lookup"><span data-stu-id="acbd9-109">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="acbd9-110">Comportamiento anterior</span><span class="sxs-lookup"><span data-stu-id="acbd9-110">Old behavior</span></span>

<span data-ttu-id="acbd9-111">El marco de la interfaz de usuario predeterminado para la interfaz de usuario de Identity era **Bootstrap 3**.</span><span class="sxs-lookup"><span data-stu-id="acbd9-111">The default UI framework for Identity UI was **Bootstrap 3**.</span></span> <span data-ttu-id="acbd9-112">El marco de la interfaz de usuario se puede configurar mediante un parámetro para la llamada al método `AddIdentityUI` en `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="acbd9-112">The UI framework could be configured using a parameter to the `AddIdentityUI` method call in `Startup.ConfigureServices`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="acbd9-113">Comportamiento nuevo</span><span class="sxs-lookup"><span data-stu-id="acbd9-113">New behavior</span></span>

<span data-ttu-id="acbd9-114">El marco de la interfaz de usuario predeterminado para la interfaz de usuario de Identity es **Bootstrap 4**.</span><span class="sxs-lookup"><span data-stu-id="acbd9-114">The default UI framework for Identity UI is **Bootstrap 4**.</span></span> <span data-ttu-id="acbd9-115">El marco de la interfaz de usuario debe configurarse en el archivo del proyecto, en lugar de en la llamada al método `AddIdentityUI`.</span><span class="sxs-lookup"><span data-stu-id="acbd9-115">The UI framework must be configured in your project file, instead of in the `AddIdentityUI` method call.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="acbd9-116">Motivo del cambio</span><span class="sxs-lookup"><span data-stu-id="acbd9-116">Reason for change</span></span>

<span data-ttu-id="acbd9-117">La adopción de la característica de recursos web estáticos requiere que la configuración del marco de la interfaz de usuario se mueva a MSBuild.</span><span class="sxs-lookup"><span data-stu-id="acbd9-117">Adoption of the static web assets feature required that the UI framework configuration move to MSBuild.</span></span> <span data-ttu-id="acbd9-118">La decisión sobre qué marco se debe insertar es una decisión en tiempo de compilación, no una decisión en runtime.</span><span class="sxs-lookup"><span data-stu-id="acbd9-118">The decision on which framework to embed is a build-time decision, not a runtime decision.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="acbd9-119">Acción recomendada</span><span class="sxs-lookup"><span data-stu-id="acbd9-119">Recommended action</span></span>

<span data-ttu-id="acbd9-120">Revise la interfaz de usuario del sitio para garantizar que los nuevos componentes de Bootstrap 4 son compatibles.</span><span class="sxs-lookup"><span data-stu-id="acbd9-120">Review your site UI to ensure the new Bootstrap 4 components are compatible.</span></span> <span data-ttu-id="acbd9-121">En caso necesario, use la propiedad `IdentityUIFrameworkVersion` de MSBuild para revertir a Bootstrap 3.</span><span class="sxs-lookup"><span data-stu-id="acbd9-121">If necessary, use the `IdentityUIFrameworkVersion` MSBuild property to revert to Bootstrap 3.</span></span> <span data-ttu-id="acbd9-122">Agregue la propiedad a un elemento `<PropertyGroup>` en el archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="acbd9-122">Add the property to a `<PropertyGroup>` element in your project file:</span></span>

```xml
<IdentityUIFrameworkVersion>Bootstrap3</IdentityUIFrameworkVersion>
```

#### <a name="category"></a><span data-ttu-id="acbd9-123">Categoría</span><span class="sxs-lookup"><span data-stu-id="acbd9-123">Category</span></span>

<span data-ttu-id="acbd9-124">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="acbd9-124">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="acbd9-125">API afectadas</span><span class="sxs-lookup"><span data-stu-id="acbd9-125">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI(Microsoft.AspNetCore.Identity.IdentityBuilder)?displayProperty=nameWithType>

<!-- 

#### Affected APIs

`M:Microsoft.AspNetCore.Identity.IdentityBuilderUIExtensions.AddDefaultUI(Microsoft.AspNetCore.Identity.IdentityBuilder)`

-->