---
title: Almacenar secretos de aplicación de forma segura durante el desarrollo
description: 'Seguridad en microservicios y aplicaciones web de .NET: no almacene sus secretos de aplicación (contraseñas, cadenas de conexión o claves de API) en el control de código fuente y aprenda las opciones que puede usar en ASP.NET Core (en particular, debe aprender a controlar los "secretos de usuario").'
author: mjrousos
ms.author: wiwagn
ms.date: 10/19/2018
ms.openlocfilehash: fe8e7fa11c9a4f4cae133c2e09f9e4b4dd40a546
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675702"
---
# <a name="store-application-secrets-safely-during-development"></a><span data-ttu-id="cf658-103">Almacenar secretos de aplicación de forma segura durante el desarrollo</span><span class="sxs-lookup"><span data-stu-id="cf658-103">Store application secrets safely during development</span></span>

<span data-ttu-id="cf658-104">Para conectar con los recursos protegidos y otros servicios, las aplicaciones de ASP.NET Core normalmente necesitan usar cadenas de conexión, contraseñas u otras credenciales que contienen información confidencial.</span><span class="sxs-lookup"><span data-stu-id="cf658-104">To connect with protected resources and other services, ASP.NET Core applications typically need to use connection strings, passwords, or other credentials that contain sensitive information.</span></span> <span data-ttu-id="cf658-105">Estos fragmentos de información confidenciales se denominan *secretos*.</span><span class="sxs-lookup"><span data-stu-id="cf658-105">These sensitive pieces of information are called *secrets*.</span></span> <span data-ttu-id="cf658-106">Es un procedimiento recomendado no incluir secretos en el código fuente y, ciertamente, no almacenar secretos en el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="cf658-106">It's a best practice to not include secrets in source code and making sure not to store secrets in source control.</span></span> <span data-ttu-id="cf658-107">En su lugar, debe usar el modelo de configuración de ASP.NET Core para leer los secretos desde ubicaciones más seguras.</span><span class="sxs-lookup"><span data-stu-id="cf658-107">Instead, you should use the ASP.NET Core configuration model to read the secrets from more secure locations.</span></span>

<span data-ttu-id="cf658-108">Debe separar los secretos usados para acceder a los recursos de desarrollo y almacenamiento provisional de los usados para acceder a los recursos de producción, ya que distintas personas deben tener acceso a los diferentes conjuntos de secretos.</span><span class="sxs-lookup"><span data-stu-id="cf658-108">You must separate the secrets for accessing development and staging resources from the ones used for accessing production resources, because different individuals will need access to those different sets of secrets.</span></span> <span data-ttu-id="cf658-109">Para almacenar secretos usados durante el desarrollo, los enfoques comunes son almacenar secretos en variables de entorno o usar la herramienta ASP.NET Core Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="cf658-109">To store secrets used during development, common approaches are to either store secrets in environment variables or by using the ASP.NET Core Secret Manager tool.</span></span> <span data-ttu-id="cf658-110">Para un almacenamiento más seguro en entornos de producción, los microservicios pueden almacenar secretos en un Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cf658-110">For more secure storage in production environments, microservices can store secrets in an Azure Key Vault.</span></span>

## <a name="store-secrets-in-environment-variables"></a><span data-ttu-id="cf658-111">Almacenamiento de secretos en variables de entorno</span><span class="sxs-lookup"><span data-stu-id="cf658-111">Store secrets in environment variables</span></span>

<span data-ttu-id="cf658-112">Una manera de mantener secretos fuera del código fuente es que los desarrolladores establezcan secretos basados en cadena como [variables de entorno](/aspnet/core/security/app-secrets#environment-variables) en sus máquinas de desarrollo.</span><span class="sxs-lookup"><span data-stu-id="cf658-112">One way to keep secrets out of source code is for developers to set string-based secrets as [environment variables](/aspnet/core/security/app-secrets#environment-variables) on their development machines.</span></span> <span data-ttu-id="cf658-113">Cuando use variables de entorno para almacenar secretos con nombres jerárquicos, como las anidadas en las secciones de configuración, debe asignar un nombre a las variables para incluir la jerarquía completa de sus secciones, delimitada por signos de dos puntos (:).</span><span class="sxs-lookup"><span data-stu-id="cf658-113">When you use environment variables to store secrets with hierarchical names, such as the ones nested in configuration sections, you must name the variables to include the complete hierarchy of its sections, delimited with colons (:).</span></span>

<span data-ttu-id="cf658-114">Por ejemplo, establecer una variable de entorno `Logging:LogLevel:Default` to `Debug` sería equivalente a un valor de configuración del archivo JSON siguiente:</span><span class="sxs-lookup"><span data-stu-id="cf658-114">For example, setting an environment variable `Logging:LogLevel:Default` to `Debug` value would be equivalent to a configuration value from the following JSON file:</span></span>

```json
{
    "Logging": {
        "LogLevel": {
            "Default": "Debug"
        }
    }
}
```

<span data-ttu-id="cf658-115">Para acceder a estos valores de variables de entorno, la aplicación solo debe llamar a AddEnvironmentVariables en su ConfigurationBuilder al construir un objeto IConfigurationRoot.</span><span class="sxs-lookup"><span data-stu-id="cf658-115">To access these values from environment variables, the application just needs to call AddEnvironmentVariables on its ConfigurationBuilder when constructing an IConfigurationRoot object.</span></span>

<span data-ttu-id="cf658-116">Tenga en cuenta que las variables de entorno suelen almacenarse como texto sin formato, por lo que si se pone en peligro la máquina o el proceso con las variables de entorno, se verán los valores de las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="cf658-116">Note that environment variables are commonly stored as plain text, so if the machine or process with the environment variables is compromised, the environment variable values will be visible.</span></span>

## <a name="store-secrets-with-the-aspnet-core-secret-manager"></a><span data-ttu-id="cf658-117">Almacenamiento de secretos mediante ASP.NET Core Secret Manager</span><span class="sxs-lookup"><span data-stu-id="cf658-117">Store secrets with the ASP.NET Core Secret Manager</span></span>

<span data-ttu-id="cf658-118">La herramienta [Secret Manager](/aspnet/core/security/app-secrets#secret-manager) de ASP.NET Core proporciona otro método para mantener secretos fuera del código fuente.</span><span class="sxs-lookup"><span data-stu-id="cf658-118">The ASP.NET Core [Secret Manager](/aspnet/core/security/app-secrets#secret-manager) tool provides another method of keeping secrets out of source code.</span></span> <span data-ttu-id="cf658-119">Para usar la herramienta Secret Manager, instale el paquete **Microsoft.Extensions.Configuration.SecretManager** en su archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="cf658-119">To use the Secret Manager tool, install the package **Microsoft.Extensions.Configuration.SecretManager** in your project file.</span></span> <span data-ttu-id="cf658-120">Una vez que esa dependencia está presente y se ha restaurado, se puede usar el comando `dotnet user-secrets` para establecer el valor de los secretos desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="cf658-120">Once that dependency is present and has been restored, the `dotnet user-secrets` command can be used to set the value of secrets from the command line.</span></span> <span data-ttu-id="cf658-121">Estos secretos se almacenarán en un archivo JSON en el directorio del perfil del usuario (los detalles varían según el sistema operativo), lejos del código fuente.</span><span class="sxs-lookup"><span data-stu-id="cf658-121">These secrets will be stored in a JSON file in the user’s profile directory (details vary by OS), away from source code.</span></span>

<span data-ttu-id="cf658-122">La propiedad `UserSecretsId` del proyecto que está usando los secretos organiza los secretos que establece la herramienta Secret Manager.</span><span class="sxs-lookup"><span data-stu-id="cf658-122">Secrets set by the Secret Manager tool are organized by the `UserSecretsId` property of the project that's using the secrets.</span></span> <span data-ttu-id="cf658-123">Por tanto, debe asegurarse de establecer la propiedad UserSecretsId en el archivo del proyecto, como se muestra en el siguiente fragmento.</span><span class="sxs-lookup"><span data-stu-id="cf658-123">Therefore, you must be sure to set the UserSecretsId property in your project file, as shown in the snippet below.</span></span> <span data-ttu-id="cf658-124">El valor predeterminado es un GUID asignado por Visual Studio, pero la cadena real no es importante mientras sea única en su equipo.</span><span class="sxs-lookup"><span data-stu-id="cf658-124">The default value is a GUID assigned by Visual Studio, but the actual string is not important as long as it's unique in your computer.</span></span>

```xml
<PropertyGroup>
    <UserSecretsId>UniqueIdentifyingString</UserSecretsId>
</PropertyGroup>
```

<span data-ttu-id="cf658-125">Para usar los secretos almacenados con Secret Manager en una aplicación, debe llamar a `AddUserSecrets<T>` en la instancia de ConfigurationBuilder para incluir los secretos de la aplicación en su configuración.</span><span class="sxs-lookup"><span data-stu-id="cf658-125">Using secrets stored with Secret Manager in an application is accomplished by calling `AddUserSecrets<T>` on the ConfigurationBuilder instance to include secrets for the application in its configuration.</span></span> <span data-ttu-id="cf658-126">El parámetro genérico T debe ser un tipo del ensamblado que se aplicó a UserSecretId.</span><span class="sxs-lookup"><span data-stu-id="cf658-126">The generic parameter T should be a type from the assembly that the UserSecretId was applied to.</span></span> <span data-ttu-id="cf658-127">Normalmente, usar `AddUserSecrets<Startup>` está bien.</span><span class="sxs-lookup"><span data-stu-id="cf658-127">Usually using `AddUserSecrets<Startup>` is fine.</span></span>

<span data-ttu-id="cf658-128">`AddUserSecrets<Startup>()` se incluye en las opciones predeterminadas del entorno de desarrollo al usar el método `CreateDefaultBuilder` en *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="cf658-128">The `AddUserSecrets<Startup>()` is included in the default options for the Development environment when using the `CreateDefaultBuilder` method in *Program.cs*.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="cf658-129">[Anterior](authorization-net-microservices-web-applications.md)
>[Siguiente](azure-key-vault-protects-secrets.md)</span><span class="sxs-lookup"><span data-stu-id="cf658-129">[Previous](authorization-net-microservices-web-applications.md)
[Next](azure-key-vault-protects-secrets.md)</span></span>