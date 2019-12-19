---
title: 'Instalación de .NET Core en Fedora 29 (administrador de paquetes): .NET Core'
description: Use un administrador de paquetes para instalar el SDK y el entorno de ejecución de .NET Core en Fedora 29.
author: thraka
ms.author: adegeo
ms.date: 12/04/2019
ms.openlocfilehash: 1cd761e323467ef34fdad58de7385c1ca7b2c14a
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74998934"
---
# <a name="fedora-29-package-manager---install-net-core"></a><span data-ttu-id="f7da4-103">Administrador de paquetes de Fedora 29: instalación de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7da4-103">Fedora 29 Package Manager - Install .NET Core</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

<span data-ttu-id="f7da4-104">En este artículo se describe cómo usar un administrador de paquetes para instalar .NET Core en Fedora 29.</span><span class="sxs-lookup"><span data-stu-id="f7da4-104">This article describes how to use a package manager to install .NET Core on Fedora 29.</span></span> <span data-ttu-id="f7da4-105">Si va a instalar el entorno de ejecución, le recomendamos que instale el [entorno de ejecución de ASP.NET Core](#install-the-aspnet-core-runtime), ya que incluye los de .NET Core y ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7da4-105">If you're installing the runtime, we suggest you install the [ASP.NET Core runtime](#install-the-aspnet-core-runtime), as it includes both .NET Core and ASP.NET Core runtimes.</span></span>

## <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="f7da4-106">Registro de la clave y la fuente de Microsoft</span><span class="sxs-lookup"><span data-stu-id="f7da4-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="f7da4-107">Antes de instalar .NET, deberá realizar lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="f7da4-107">Before installing .NET, you'll need to:</span></span>

- <span data-ttu-id="f7da4-108">Registrar clave de Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f7da4-108">Register the Microsoft key</span></span>
- <span data-ttu-id="f7da4-109">Registrar el repositorio del producto.</span><span class="sxs-lookup"><span data-stu-id="f7da4-109">register the product repository</span></span>
- <span data-ttu-id="f7da4-110">Instalar las dependencias necesarias.</span><span class="sxs-lookup"><span data-stu-id="f7da4-110">Install required dependencies</span></span>

<span data-ttu-id="f7da4-111">Esto solo se debe hacer una vez por máquina.</span><span class="sxs-lookup"><span data-stu-id="f7da4-111">This only needs to be done once per machine.</span></span>

<span data-ttu-id="f7da4-112">Abra un terminal y ejecute los comandos siguientes.</span><span class="sxs-lookup"><span data-stu-id="f7da4-112">Open a terminal and run the following commands.</span></span>

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -q -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/29/prod.repo
```

## <a name="install-the-net-core-sdk"></a><span data-ttu-id="f7da4-113">Instalación del SDK de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7da4-113">Install the .NET Core SDK</span></span>

<span data-ttu-id="f7da4-114">Actualice los productos disponibles para la instalación y, después, instale el SDK de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7da4-114">Update the products available for installation, then install the .NET Core SDK.</span></span> <span data-ttu-id="f7da4-115">En el terminal, ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="f7da4-115">In your terminal, run the following command.</span></span>

```bash
sudo dnf install dotnet-sdk-3.1
```

## <a name="install-the-aspnet-core-runtime"></a><span data-ttu-id="f7da4-116">Instalación del entorno de ejecución de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f7da4-116">Install the ASP.NET Core runtime</span></span>

<span data-ttu-id="f7da4-117">Actualice los productos disponibles para la instalación y, después, instale el entorno de ejecución de ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7da4-117">Update the products available for installation, then install the ASP.NET runtime.</span></span> <span data-ttu-id="f7da4-118">En el terminal, ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="f7da4-118">In your terminal, run the following command.</span></span>

```bash
sudo dnf install aspnetcore-runtime-3.1
```

## <a name="install-the-net-core-runtime"></a><span data-ttu-id="f7da4-119">Instalación del entorno de ejecución de .NET Core</span><span class="sxs-lookup"><span data-stu-id="f7da4-119">Install the .NET Core runtime</span></span>

<span data-ttu-id="f7da4-120">Actualice los productos disponibles para la instalación y, después, instale el entorno de ejecución de .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f7da4-120">Update the products available for installation, then install the .NET Core runtime.</span></span> <span data-ttu-id="f7da4-121">En el terminal, ejecute el comando siguiente.</span><span class="sxs-lookup"><span data-stu-id="f7da4-121">In your terminal, run the following command.</span></span>

```bash
sudo dnf install dotnet-runtime-3.1
```

## <a name="how-to-install-other-versions"></a><span data-ttu-id="f7da4-122">Procedimiento para instalar otras versiones</span><span class="sxs-lookup"><span data-stu-id="f7da4-122">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]