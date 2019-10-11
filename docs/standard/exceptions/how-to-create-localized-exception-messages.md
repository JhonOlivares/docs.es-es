---
title: 'Procedimiento para: Crear excepciones definidas por el usuario con mensajes de excepción localizados'
description: Obtenga información sobre cómo crear excepciones definidas por el usuario con mensajes de excepción localizados.
author: Youssef1313
ms.author: ronpet
ms.date: 09/13/2019
ms.openlocfilehash: b4aa567fccda9354bc5959d6b9838d678d53abef
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71696713"
---
# <a name="how-to-create-user-defined-exceptions-with-localized-exception-messages"></a><span data-ttu-id="98fb3-103">Procedimiento para: Crear excepciones definidas por el usuario con mensajes de excepción localizados</span><span class="sxs-lookup"><span data-stu-id="98fb3-103">How to: create user-defined exceptions with localized exception messages</span></span>

<span data-ttu-id="98fb3-104">En este artículo, obtendrá información sobre cómo crear excepciones definidas por el usuario que se hereden de la clase base <xref:System.Exception> con mensajes de excepción localizados mediante ensamblados satélite.</span><span class="sxs-lookup"><span data-stu-id="98fb3-104">In this article, you will learn how to create user-defined exceptions that are inherited from the base <xref:System.Exception> class with localized exception messages using satellite assemblies.</span></span>

## <a name="create-custom-exceptions"></a><span data-ttu-id="98fb3-105">Creación de excepciones personalizadas</span><span class="sxs-lookup"><span data-stu-id="98fb3-105">Create custom exceptions</span></span>

<span data-ttu-id="98fb3-106">.NET contiene muchas excepciones distintas que puede usar.</span><span class="sxs-lookup"><span data-stu-id="98fb3-106">.NET contains many different exceptions that you can use.</span></span> <span data-ttu-id="98fb3-107">Sin embargo, en algunos casos, cuando ninguna de ellas satisface sus necesidades, puede crear sus propias excepciones personalizadas.</span><span class="sxs-lookup"><span data-stu-id="98fb3-107">However, in some cases when none of them meets your needs, you can create your own custom exceptions.</span></span>

<span data-ttu-id="98fb3-108">Supongamos que desea crear un `StudentNotFoundException` que contiene una propiedad `StudentName`.</span><span class="sxs-lookup"><span data-stu-id="98fb3-108">Let's assume you want to create a `StudentNotFoundException` that contains a `StudentName` property.</span></span>
<span data-ttu-id="98fb3-109">Para crear una excepción personalizada, siga estos pasos:</span><span class="sxs-lookup"><span data-stu-id="98fb3-109">To create a custom exception, follow these steps:</span></span>

1. <span data-ttu-id="98fb3-110">Cree una clase serializable que herede de <xref:System.Exception>.</span><span class="sxs-lookup"><span data-stu-id="98fb3-110">Create a serializable class that inherits from <xref:System.Exception>.</span></span> <span data-ttu-id="98fb3-111">El nombre de clase debe terminar en "Exception":</span><span class="sxs-lookup"><span data-stu-id="98fb3-111">The class name should end in "Exception":</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception { }
    ```

1. <span data-ttu-id="98fb3-112">Agregue los constructores predeterminados:</span><span class="sxs-lookup"><span data-stu-id="98fb3-112">Add the default constructors:</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }
    }
    ```

1. <span data-ttu-id="98fb3-113">Defina cualquier propiedad y constructores adicionales:</span><span class="sxs-lookup"><span data-stu-id="98fb3-113">Define any additional properties and constructors:</span></span>

    ```csharp
    [Serializable]
    public class StudentNotFoundException : Exception
    {
        public string StudentName { get; }

        public StudentNotFoundException() { }

        public StudentNotFoundException(string message)
            : base(message) { }

        public StudentNotFoundException(string message, Exception inner)
            : base(message, inner) { }

        public StudentNotFoundException(string message, string studentName)
            : this(message)
        {
            StudentName = studentName;
        }
    }
    ```

## <a name="create-localized-exception-messages"></a><span data-ttu-id="98fb3-114">Creación de mensajes de excepción localizados</span><span class="sxs-lookup"><span data-stu-id="98fb3-114">Create localized exception messages</span></span>

<span data-ttu-id="98fb3-115">Creó una excepción personalizada y puede iniciarla en cualquier parte con código como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="98fb3-115">You have created a custom exception, and you can throw it anywhere with code like the following:</span></span>

```csharp
throw new StudentNotFoundException("The student cannot be found.", "John");
```

<span data-ttu-id="98fb3-116">El problema con la línea anterior es que `"The student cannot be found."` es simplemente una cadena constante.</span><span class="sxs-lookup"><span data-stu-id="98fb3-116">The problem with the previous line is that `"The student cannot be found."` is just a constant string.</span></span> <span data-ttu-id="98fb3-117">En una aplicación localizada, quiere tener mensajes diferentes en función de la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="98fb3-117">In a localized application, you want to have different messages depending on user culture.</span></span>
<span data-ttu-id="98fb3-118">Los [ensamblados satélite](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md) son una buena manera de hacerlo.</span><span class="sxs-lookup"><span data-stu-id="98fb3-118">[Satellite Assemblies](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md) are a good way to do that.</span></span> <span data-ttu-id="98fb3-119">Un ensamblado satélite es un archivo .dll que contiene recursos para un idioma específico.</span><span class="sxs-lookup"><span data-stu-id="98fb3-119">A satellite assembly is a .dll that contains resources for a specific language.</span></span> <span data-ttu-id="98fb3-120">Cuando se solicitan recursos específicos en tiempo de ejecución, el CLR encuentra ese recurso en función de la referencia cultural del usuario.</span><span class="sxs-lookup"><span data-stu-id="98fb3-120">When you ask for a specific resources at run time, the CLR finds that resource depending on user culture.</span></span> <span data-ttu-id="98fb3-121">Si no se encuentra ningún ensamblado satélite para esa referencia cultural, se usan los recursos de la referencia cultural predeterminada.</span><span class="sxs-lookup"><span data-stu-id="98fb3-121">If no satellite assembly is found for that culture, the resources of the default culture are used.</span></span>

<span data-ttu-id="98fb3-122">Para crear los mensajes de excepción localizados:</span><span class="sxs-lookup"><span data-stu-id="98fb3-122">To create the localized exception messages:</span></span>

1. <span data-ttu-id="98fb3-123">Cree una carpeta llamada *Recursos* para contener los archivos de recursos.</span><span class="sxs-lookup"><span data-stu-id="98fb3-123">Create a new folder named *Resources* to hold the resource files.</span></span>
1. <span data-ttu-id="98fb3-124">Agréguele un archivo de recursos nuevo.</span><span class="sxs-lookup"><span data-stu-id="98fb3-124">Add a new resource file to it.</span></span> <span data-ttu-id="98fb3-125">Para ello, en Visual Studio, haga clic con el botón derecho en la carpeta en el **Explorador de soluciones** y seleccione **Agregar** > **Nuevo elemento** > **Archivo de recursos**.</span><span class="sxs-lookup"><span data-stu-id="98fb3-125">To do that in Visual Studio, right-click the folder in **Solution Explorer**, and select **Add** > **New Item** > **Resources File**.</span></span> <span data-ttu-id="98fb3-126">Asígnele al archivo el nombre *ExceptionMessages.resx*.</span><span class="sxs-lookup"><span data-stu-id="98fb3-126">Name the file *ExceptionMessages.resx*.</span></span> <span data-ttu-id="98fb3-127">Este es el archivo de recursos predeterminado.</span><span class="sxs-lookup"><span data-stu-id="98fb3-127">This is the default resources file.</span></span>
1. <span data-ttu-id="98fb3-128">Agregue un par nombre-valor para el mensaje de excepción, como se muestra en la imagen siguiente:</span><span class="sxs-lookup"><span data-stu-id="98fb3-128">Add a name/value pair for your exception message, like the following image shows:</span></span>

   ![Incorporación de recursos a la referencia cultural predeterminada](media/add-resources-to-default-culture.jpg)

1. <span data-ttu-id="98fb3-130">Agregue un archivo de recursos nuevo para francés.</span><span class="sxs-lookup"><span data-stu-id="98fb3-130">Add a new resource file for French.</span></span> <span data-ttu-id="98fb3-131">Asígnele el nombre *ExceptionMessages.fr-FR.resx*.</span><span class="sxs-lookup"><span data-stu-id="98fb3-131">Name it *ExceptionMessages.fr-FR.resx*.</span></span>
1. <span data-ttu-id="98fb3-132">Vuelva a agregar un par nombre-valor para el mensaje de excepción, pero con un valor en francés:</span><span class="sxs-lookup"><span data-stu-id="98fb3-132">Add a name/value pair for the exception message again, but with a French value:</span></span>

   ![Incorporación de recursos a la referencia cultural fr-FR](media/add-resources-to-fr-culture.jpg)

1. <span data-ttu-id="98fb3-134">Después de compilar el proyecto, la carpeta de la salida de compilación debe contener la carpeta *fr-FR* con un archivo *.dll*, que es el ensamblado satélite.</span><span class="sxs-lookup"><span data-stu-id="98fb3-134">After you build the project, the build output folder should contain the *fr-FR* folder with a *.dll* file, which is the satellite assembly.</span></span>
1. <span data-ttu-id="98fb3-135">Inicia la excepción con código como el siguiente:</span><span class="sxs-lookup"><span data-stu-id="98fb3-135">You throw the exception with code like the following:</span></span>

    ```csharp
    var resourceManager = new ResourceManager("FULLY_QIALIFIED_NAME_OF_RESOURCE_FILE", Assembly.GetExecutingAssembly());
    throw new StudentNotFoundException(resourceManager.GetString("StudentNotFound"), "John");
    ```

  > [!NOTE]
  > <span data-ttu-id="98fb3-136">Si el nombre del proyecto es `TestProject` y el archivo de recursos *ExceptionMessages.resx* reside en la carpeta *Recursos* del proyecto, el nombre completo del archivo de recursos es `TestProject.Resources.ExceptionMessages`.</span><span class="sxs-lookup"><span data-stu-id="98fb3-136">If the project name is `TestProject` and the resource file *ExceptionMessages.resx* resides in the *Resources* folder of the project, the fully qualified name of the resource file is `TestProject.Resources.ExceptionMessages`.</span></span>

## <a name="see-also"></a><span data-ttu-id="98fb3-137">Vea también</span><span class="sxs-lookup"><span data-stu-id="98fb3-137">See also</span></span>

- [<span data-ttu-id="98fb3-138">Cómo crear excepciones definidas por el usuario</span><span class="sxs-lookup"><span data-stu-id="98fb3-138">How to create user-defined exceptions</span></span>](how-to-create-user-defined-exceptions.md)
- [<span data-ttu-id="98fb3-139">Crear ensamblados satélite para aplicaciones de escritorio</span><span class="sxs-lookup"><span data-stu-id="98fb3-139">Creating Satellite Assemblies for Desktop Apps</span></span>](../../framework/resources/creating-satellite-assemblies-for-desktop-apps.md)
- [<span data-ttu-id="98fb3-140">base (Referencia de C#)</span><span class="sxs-lookup"><span data-stu-id="98fb3-140">base (C# Reference)</span></span>](../../csharp/language-reference/keywords/base.md)
- [<span data-ttu-id="98fb3-141">this (Referencia de C#)</span><span class="sxs-lookup"><span data-stu-id="98fb3-141">this (C# Reference)</span></span>](../../csharp/language-reference/keywords/this.md)