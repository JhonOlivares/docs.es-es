---
title: Comando dotnet add reference
description: El comando dotnet add reference constituye una opción práctica para agregar referencias entre proyectos.
ms.date: 02/14/2020
ms.openlocfilehash: f2bd67d181784c4858b8971d05053d196df7818e
ms.sourcegitcommit: 927b7ea6b2ea5a440c8f23e3e66503152eb85591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2020
ms.locfileid: "81463743"
---
# <a name="dotnet-add-reference"></a>dotnet add reference

**Este artículo se aplica a:** ✔️ SDK de .NET Core 2.x y versiones posteriores

## <a name="name"></a>NOMBRE

`dotnet add reference` Agrega referencias entre proyectos (P2P) .

## <a name="synopsis"></a>Sinopsis

```dotnetcli
dotnet add [<PROJECT>] reference [-f|--framework <FRAMEWORK>]
     [--interactive] <PROJECT_REFERENCES>

dotnet add reference -h|--help
```

## <a name="description"></a>Descripción

El comando `dotnet add reference` constituye una opción práctica para agregar referencias de proyecto a un proyecto. Después de ejecutar el comando, los elementos `<ProjectReference>` se agregan al archivo del proyecto.

```xml
<ItemGroup>
  <ProjectReference Include="app.csproj" />
  <ProjectReference Include="..\lib2\lib2.csproj" />
  <ProjectReference Include="..\lib1\lib1.csproj" />
</ItemGroup>
```

## <a name="arguments"></a>Argumentos

- **`PROJECT`**

  Especifica el archivo del proyecto. Si no se especifica, el comando busca uno en el directorio actual.

- **`PROJECT_REFERENCES`**

  Referencias entre proyectos (P2P) que se van a agregar. Especifique uno o más proyectos. [El patrón glob](https://en.wikipedia.org/wiki/Glob_(programming)) se admite en sistemas basados en Unix/Linux.

## <a name="options"></a>Opciones

- **`-f|--framework <FRAMEWORK>`**

  Agrega referencias de proyecto solo cuando apunta a un[marco](../../standard/frameworks.md) específico.

- **`-h|--help`**

  Imprime una corta ayuda para el comando.

- **`--interactive`**

  Permite que el comando se detenga y espere la entrada o acción del usuario (por ejemplo, completar la autenticación). Disponible desde el SDK de .NET Core 3.0.

## <a name="examples"></a>Ejemplos

- Agregar una referencia de proyecto:

  ```dotnetcli
  dotnet add app/app.csproj reference lib/lib.csproj
  ```

- Agregar varias referencias de proyecto al proyecto en el directorio actual:

  ```dotnetcli
  dotnet add reference lib1/lib1.csproj lib2/lib2.csproj
  ```

- Agregar varias referencias de proyecto usando el patrón global en Linux/Unix:

  ```dotnetcli
  dotnet add app/app.csproj reference **/*.csproj
  ```
