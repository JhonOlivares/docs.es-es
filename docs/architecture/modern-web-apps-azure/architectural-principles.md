---
title: Principios de la arquitectura
description: Diseño de aplicaciones web modernas con ASP.NET Core y Azure | Principios de la arquitectura
author: ardalis
ms.author: wiwagn
ms.date: 02/16/2019
ms.openlocfilehash: 74ff7196ce17807b98a975687a524041f15a7f5b
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68675582"
---
# <a name="architectural-principles"></a><span data-ttu-id="95995-103">Principios de la arquitectura</span><span class="sxs-lookup"><span data-stu-id="95995-103">Architectural principles</span></span>

> <span data-ttu-id="95995-104">"Si los constructores construyeran los edificios como los programadores escriben los programas, el primer pájaro carpintero que apareciera destruiría la civilización".</span><span class="sxs-lookup"><span data-stu-id="95995-104">"If builders built buildings the way programmers wrote programs, then the first woodpecker that came along would destroy civilization."</span></span>  
> <span data-ttu-id="95995-105">_\- Gerald Weinberg_</span><span class="sxs-lookup"><span data-stu-id="95995-105">_\- Gerald Weinberg_</span></span>

<span data-ttu-id="95995-106">Las soluciones de software se deben diseñar y crear con el mantenimiento en mente.</span><span class="sxs-lookup"><span data-stu-id="95995-106">You should architect and design software solutions with maintainability in mind.</span></span> <span data-ttu-id="95995-107">Los principios que se describen en esta sección le ayudarán a tomar decisiones arquitectónicas que darán como resultado aplicaciones limpias y fácil de mantener.</span><span class="sxs-lookup"><span data-stu-id="95995-107">The principles outlined in this section can help guide you toward architectural decisions that will result in clean, maintainable applications.</span></span> <span data-ttu-id="95995-108">Por lo general, estos principios le ayudarán a compilar aplicaciones a partir de componentes discretos que no están estrechamente relacionados con otras partes de la aplicación, sino que se comunican a través de interfaces explícitas o sistemas de mensajería.</span><span class="sxs-lookup"><span data-stu-id="95995-108">Generally, these principles will guide you toward building applications out of discrete components that are not tightly coupled to other parts of your application, but rather communicate through explicit interfaces or messaging systems.</span></span>

## <a name="common-design-principles"></a><span data-ttu-id="95995-109">Principios de diseño comunes</span><span class="sxs-lookup"><span data-stu-id="95995-109">Common design principles</span></span>

### <a name="separation-of-concerns"></a><span data-ttu-id="95995-110">Separación de intereses</span><span class="sxs-lookup"><span data-stu-id="95995-110">Separation of concerns</span></span>

<span data-ttu-id="95995-111">Un principio fundamental durante el desarrollo es la **separación de intereses**.</span><span class="sxs-lookup"><span data-stu-id="95995-111">A guiding principle when developing is **Separation of Concerns**.</span></span> <span data-ttu-id="95995-112">Este principio afirma que el software se debe separar en función de los tipos de trabajo que realiza.</span><span class="sxs-lookup"><span data-stu-id="95995-112">This principle asserts that software should be separated based on the kinds of work it performs.</span></span> <span data-ttu-id="95995-113">Por ejemplo, considere una aplicación que incluye lógica para identificar los elementos de interés que se van a mostrar al usuario y que da formato a estos elementos de una manera determinada para hacerlos más evidentes.</span><span class="sxs-lookup"><span data-stu-id="95995-113">For instance, consider an application that includes logic for identifying noteworthy items to display to the user, and which formats such items in a particular way to make them more noticeable.</span></span> <span data-ttu-id="95995-114">El comportamiento responsable de elegir a qué elementos dar formato se debe mantener separado del comportamiento responsable de dar formato a los elementos, puesto que estos son intereses independientes que solo se relacionan entre sí de manera casual.</span><span class="sxs-lookup"><span data-stu-id="95995-114">The behavior responsible for choosing which items to format should be kept separate from the behavior responsible for formatting the items, since these are separate concerns that are only coincidentally related to one another.</span></span>

<span data-ttu-id="95995-115">Arquitectónicamente, las aplicaciones se pueden crear de forma lógica para seguir este principio mediante la separación del comportamiento de negocios principal de la lógica de la interfaz de usuario y la infraestructura.</span><span class="sxs-lookup"><span data-stu-id="95995-115">Architecturally, applications can be logically built to follow this principle by separating core business behavior from infrastructure and user interface logic.</span></span> <span data-ttu-id="95995-116">Idealmente, la lógica y las reglas de negocios deben residir en un proyecto independiente, que no debería depender de otros proyectos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95995-116">Ideally, business rules and logic should reside in a separate project, which should not depend on other projects in the application.</span></span> <span data-ttu-id="95995-117">Esto ayuda a garantizar que el modelo de negocio sea fácil de probar y pueda evolucionar sin que se esté estrechamente unido a detalles de implementación de bajo nivel.</span><span class="sxs-lookup"><span data-stu-id="95995-117">This helps ensure that the business model is easy to test and can evolve without being tightly coupled to low-level implementation details.</span></span> <span data-ttu-id="95995-118">La separación de intereses es una consideración clave detrás del uso de capas en arquitecturas de aplicación.</span><span class="sxs-lookup"><span data-stu-id="95995-118">Separation of concerns is a key consideration behind the use of layers in application architectures.</span></span>

### <a name="encapsulation"></a><span data-ttu-id="95995-119">Encapsulación</span><span class="sxs-lookup"><span data-stu-id="95995-119">Encapsulation</span></span>

<span data-ttu-id="95995-120">Las diferentes partes de una aplicación deben usar la **encapsulación** para aislarse de otras partes de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95995-120">Different parts of an application should use **encapsulation** to insulate them from other parts of the application.</span></span> <span data-ttu-id="95995-121">Las capas y los componentes de la aplicación deben poder ajustar su implementación interna sin interrumpir a sus colaboradores mientras no se infrinjan los externos.</span><span class="sxs-lookup"><span data-stu-id="95995-121">Application components and layers should be able to adjust their internal implementation without breaking their collaborators as long as external contracts are not violated.</span></span> <span data-ttu-id="95995-122">El uso correcto de la encapsulación contribuye a lograr el acoplamiento flexible y la modularidad en los diseños de aplicaciones, ya que los objetos y paquetes se pueden reemplazar con implementaciones alternativas, siempre y cuando se mantenga la misma interfaz.</span><span class="sxs-lookup"><span data-stu-id="95995-122">Proper use of encapsulation helps achieve loose coupling and modularity in application designs, since objects and packages can be replaced with alternative implementations so long as the same interface is maintained.</span></span>

<span data-ttu-id="95995-123">En las clases, la encapsulación se logra mediante la limitación del acceso externo al estado interno de la clase.</span><span class="sxs-lookup"><span data-stu-id="95995-123">In classes, encapsulation is achieved by limiting outside access to the class's internal state.</span></span> <span data-ttu-id="95995-124">Si un actor externo quiere manipular el estado del objeto, deberá hacerlo a través de una función bien definida (o un establecedor de propiedades), en lugar de tener acceso directo al estado privado del objeto.</span><span class="sxs-lookup"><span data-stu-id="95995-124">If an outside actor wants to manipulate the state of the object, it should do so through a well-defined function (or property setter), rather than having direct access to the private state of the object.</span></span> <span data-ttu-id="95995-125">Del mismo modo, los componentes de aplicación y las propias aplicaciones deben exponer interfaces bien definidas para que sus colaboradores las usen, en lugar de permitir que su estado se modifique directamente.</span><span class="sxs-lookup"><span data-stu-id="95995-125">Likewise, application components and applications themselves should expose well-defined interfaces for their collaborators to use, rather than allowing their state to be modified directly.</span></span> <span data-ttu-id="95995-126">Esto libera el diseño interno de la aplicación para que evolucione con el tiempo sin tener que preocuparse de si al hacerlo afectará a los colaboradores, siempre y cuando se mantengan los contratos públicos.</span><span class="sxs-lookup"><span data-stu-id="95995-126">This frees the application's internal design to evolve over time without worrying that doing so will break collaborators, so long as the public contracts are maintained.</span></span>

### <a name="dependency-inversion"></a><span data-ttu-id="95995-127">Inversión de dependencias</span><span class="sxs-lookup"><span data-stu-id="95995-127">Dependency inversion</span></span>

<span data-ttu-id="95995-128">La dirección de dependencia dentro de la aplicación debe estar en la dirección de la abstracción, no de los detalles de implementación.</span><span class="sxs-lookup"><span data-stu-id="95995-128">The direction of dependency within the application should be in the direction of abstraction, not implementation details.</span></span> <span data-ttu-id="95995-129">La mayoría de las aplicaciones se escriben de manera que la dependencia de tiempo de compilación fluya en la dirección de ejecución del tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="95995-129">Most applications are written such that compile-time dependency flows in the direction of runtime execution.</span></span> <span data-ttu-id="95995-130">Esto genera un gráfico de dependencias directas.</span><span class="sxs-lookup"><span data-stu-id="95995-130">This produces a direct dependency graph.</span></span> <span data-ttu-id="95995-131">Es decir, si el módulo A llama a una función en el módulo B, que llama a una función en el módulo C, en tiempo de compilación A dependerá de B que dependerá de C, como se muestra en la figura 4-1.</span><span class="sxs-lookup"><span data-stu-id="95995-131">That is, if module A calls a function in module B, which calls a function in module C, then at compile time A will depend on B which will depend on C, as shown in Figure 4-1.</span></span>

![](./media/image4-1.png)

<span data-ttu-id="95995-132">**Figura 4-1.**</span><span class="sxs-lookup"><span data-stu-id="95995-132">**Figure 4-1.**</span></span> <span data-ttu-id="95995-133">Gráfico de dependencias directas.</span><span class="sxs-lookup"><span data-stu-id="95995-133">Direct dependency graph.</span></span>

<span data-ttu-id="95995-134">Aplicar el principio de inversión de dependencias permite que A llame a métodos en una abstracción que implementa B, lo que hace posible que A llame a B en tiempo de ejecución, pero que B dependa de una interfaz controlada por A en tiempo de compilación (por tanto, se *invierte* la dependencia de tiempo de compilación normal).</span><span class="sxs-lookup"><span data-stu-id="95995-134">Applying the dependency inversion principle allows A to call methods on an abstraction that B implements, making it possible for A to call B at runtime, but for B to depend on an interface controlled by A at compile time (thus, *inverting* the typical compile-time dependency).</span></span> <span data-ttu-id="95995-135">En tiempo de ejecución, el flujo de ejecución del programa no cambia, pero la introducción de interfaces significa que se pueden conectar fácilmente otras implementaciones de estas interfaces.</span><span class="sxs-lookup"><span data-stu-id="95995-135">At run time, the flow of program execution remains unchanged, but the introduction of interfaces means that different implementations of these interfaces can easily be plugged in.</span></span>

![](./media/image4-2.png)

<span data-ttu-id="95995-136">**Figura 4-2.**</span><span class="sxs-lookup"><span data-stu-id="95995-136">**Figure 4-2.**</span></span> <span data-ttu-id="95995-137">Gráfico de dependencias invertidas.</span><span class="sxs-lookup"><span data-stu-id="95995-137">Inverted dependency graph.</span></span>

<span data-ttu-id="95995-138">La **Inversión de dependencias** es una parte fundamental de la creación de aplicaciones de acoplamiento flexible, ya que se pueden escribir detalles de implementación de los que depender e implementar abstracciones de nivel superior, en lugar de hacerlo al contrario.</span><span class="sxs-lookup"><span data-stu-id="95995-138">**Dependency inversion** is a key part of building loosely-coupled applications, since implementation details can be written to depend on and implement higher level abstractions, rather than the other way around.</span></span> <span data-ttu-id="95995-139">Como resultado, las aplicaciones son modulares y más fáciles de probar y mantener.</span><span class="sxs-lookup"><span data-stu-id="95995-139">The resulting applications are more testable, modular, and maintainable as a result.</span></span> <span data-ttu-id="95995-140">La práctica de la *inserción de dependencias* es posible si se sigue el principio de inversión de dependencias.</span><span class="sxs-lookup"><span data-stu-id="95995-140">The practice of *dependency injection* is made possible by following the dependency inversion principle.</span></span>

### <a name="explicit-dependencies"></a><span data-ttu-id="95995-141">Dependencias explícitas</span><span class="sxs-lookup"><span data-stu-id="95995-141">Explicit dependencies</span></span>

<span data-ttu-id="95995-142">**Los métodos y las clases deben requerir explícitamente todos los objetos de colaboración que necesiten para funcionar correctamente.**</span><span class="sxs-lookup"><span data-stu-id="95995-142">**Methods and classes should explicitly require any collaborating objects they need in order to function correctly.**</span></span> <span data-ttu-id="95995-143">Los constructores de clases proporcionan una oportunidad para que las clases identifiquen lo que necesitan para poder tener un estado válido y funcionar correctamente.</span><span class="sxs-lookup"><span data-stu-id="95995-143">Class constructors provide an opportunity for classes to identify the things they need in order to be in a valid state and to function properly.</span></span> <span data-ttu-id="95995-144">Si se definen clases que se pueden construir y llamar, pero que solo funcionarán correctamente si existen determinados componentes globales o de infraestructura, estas clases no estarán siendo *honestas* con sus clientes.</span><span class="sxs-lookup"><span data-stu-id="95995-144">If you define classes that can be constructed and called, but which will only function properly if certain global or infrastructure components are in place, these classes are being *dishonest* with their clients.</span></span> <span data-ttu-id="95995-145">El contrato de constructor indica al cliente que solo necesita los elementos especificados (posiblemente ninguno si la clase solo usa un constructor sin parámetros), pero después, en tiempo de ejecución, en realidad el objeto necesita algo más.</span><span class="sxs-lookup"><span data-stu-id="95995-145">The constructor contract is telling the client that it only needs the things specified (possibly nothing if the class is just using a parameterless constructor), but then at runtime it turns out the object really did need something else.</span></span>

<span data-ttu-id="95995-146">Si siguen el principio de dependencias explícitas, las clases y métodos estarán siendo sinceros con sus clientes con respecto a lo que necesitan para poder funcionar.</span><span class="sxs-lookup"><span data-stu-id="95995-146">By following the explicit dependencies principle, your classes and methods are being honest with their clients about what they need in order to function.</span></span> <span data-ttu-id="95995-147">Esto hace que el código sea más autoexplicativo y los contratos de codificación más fáciles de usar, puesto que los usuarios confiarán en eso siempre que proporcionen lo que se necesita en forma de parámetros de método o constructor, los objetos con los trabajan se comportarán correctamente en tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="95995-147">This makes your code more self-documenting and your coding contracts more user-friendly, since users will come to trust that as long as they provide what's required in the form of method or constructor parameters, the objects they're working with will behave correctly at runtime.</span></span>

### <a name="single-responsibility"></a><span data-ttu-id="95995-148">Responsabilidad única</span><span class="sxs-lookup"><span data-stu-id="95995-148">Single responsibility</span></span>

<span data-ttu-id="95995-149">El principio de responsabilidad única se aplica al diseño orientado a objetos, pero también se puede considerar como un principio de arquitectura similar a la separación de intereses.</span><span class="sxs-lookup"><span data-stu-id="95995-149">The single responsibility principle applies to object-oriented design, but can also be considered as an architectural principle similar to separation of concerns.</span></span> <span data-ttu-id="95995-150">Indica que los objetos solo deben tener una responsabilidad y solo una razón para cambiar.</span><span class="sxs-lookup"><span data-stu-id="95995-150">It states that objects should have only one responsibility and that they should have only one reason to change.</span></span> <span data-ttu-id="95995-151">En concreto, la única situación en la que el objeto debe cambiar es si hay que actualizar la manera en la que lleva a cabo su única responsabilidad.</span><span class="sxs-lookup"><span data-stu-id="95995-151">Specifically, the only situation in which the object should change is if the manner in which it performs its one responsibility must be updated.</span></span> <span data-ttu-id="95995-152">El seguimiento de este principio ayuda a generar sistemas más modulares y de acoplamiento flexible, dado que muchos tipos de comportamientos nuevos se pueden implementar como clases nuevas, en lugar de mediante la adición de responsabilidad adicional a las clases existentes.</span><span class="sxs-lookup"><span data-stu-id="95995-152">Following this principle helps to produce more loosely-coupled and modular systems, since many kinds of new behavior can be implemented as new classes, rather than by adding additional responsibility to existing classes.</span></span> <span data-ttu-id="95995-153">Agregar clases nuevas siempre es más seguro que cambiar las existentes, puesto que todavía no hay código que dependa de las clases nuevas.</span><span class="sxs-lookup"><span data-stu-id="95995-153">Adding new classes is always safer than changing existing classes, since no code yet depends on the new classes.</span></span>

<span data-ttu-id="95995-154">En una aplicación monolítica, se puede aplicar el principio de responsabilidad única en un nivel general a las capas de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95995-154">In a monolithic application, we can apply the single responsibility principle at a high level to the layers in the application.</span></span> <span data-ttu-id="95995-155">La responsabilidad de la presentación debe mantenerse en el proyecto de la interfaz de usuario, mientras que la responsabilidad de acceso a los datos se debe mantener en un proyecto de infraestructura.</span><span class="sxs-lookup"><span data-stu-id="95995-155">Presentation responsibility should remain in the UI project, while data access responsibility should be kept within an infrastructure project.</span></span> <span data-ttu-id="95995-156">La lógica de negocios se debe mantener en el proyecto principal de la aplicación, donde se puede probar fácilmente y puede evolucionar con independencia de otras responsabilidades.</span><span class="sxs-lookup"><span data-stu-id="95995-156">Business logic should be kept in the application core project, where it can be easily tested and can evolve independently from other responsibilities.</span></span>

<span data-ttu-id="95995-157">Cuando este principio se aplica a la arquitectura de la aplicación y se lleva a su punto de conexión lógico, se obtienen microservicios.</span><span class="sxs-lookup"><span data-stu-id="95995-157">When this principle is applied to application architecture, and taken to its logical endpoint, you get microservices.</span></span> <span data-ttu-id="95995-158">Un microservicio determinado debe tener una sola responsabilidad.</span><span class="sxs-lookup"><span data-stu-id="95995-158">A given microservice should have a single responsibility.</span></span> <span data-ttu-id="95995-159">Si es necesario extender el comportamiento de un sistema, es mejor hacerlo agregando otros microservicios, en lugar de agregar responsabilidad a uno ya existente.</span><span class="sxs-lookup"><span data-stu-id="95995-159">If you need to extend the behavior of a system, it's usually better to do it by adding additional microservices, rather than by adding responsibility to an existing one.</span></span>

[<span data-ttu-id="95995-160">Más información sobre la arquitectura de microservicios</span><span class="sxs-lookup"><span data-stu-id="95995-160">Learn more about microservices architecture</span></span>](https://aka.ms/MicroservicesEbook)

### <a name="dont-repeat-yourself-dry"></a><span data-ttu-id="95995-161">Una vez y solo una (DRY)</span><span class="sxs-lookup"><span data-stu-id="95995-161">Don't repeat yourself (DRY)</span></span>

<span data-ttu-id="95995-162">La aplicación debe evitar especificar el comportamiento relacionado con un determinado concepto en varios lugares, ya que esto es una fuente de errores frecuente.</span><span class="sxs-lookup"><span data-stu-id="95995-162">The application should avoid specifying behavior related to a particular concept in multiple places as this is a frequent source of errors.</span></span> <span data-ttu-id="95995-163">En algún momento, un cambio en los requisitos hará que sea necesario cambiar este comportamiento y la probabilidad de que se produzca un error al actualizar al menos una instancia del comportamiento dará como resultado el comportamiento incoherente del sistema.</span><span class="sxs-lookup"><span data-stu-id="95995-163">At some point, a change in requirements will require changing this behavior and the likelihood that at least one instance of the behavior will fail to be updated will result in inconsistent behavior of the system.</span></span>

<span data-ttu-id="95995-164">En lugar de duplicar la lógica, se puede encapsular en una construcción de programación.</span><span class="sxs-lookup"><span data-stu-id="95995-164">Rather than duplicating logic, encapsulate it in a programming construct.</span></span> <span data-ttu-id="95995-165">Convierta esta construcción en la única autoridad sobre este comportamiento y haga que cualquier otro elemento de la aplicación que requiera este comportamiento use la nueva construcción.</span><span class="sxs-lookup"><span data-stu-id="95995-165">Make this construct the single authority over this behavior, and have any other part of the application that requires this behavior use the new construct.</span></span>

> [!NOTE]
> <span data-ttu-id="95995-166">Evite el enlace conjunto del comportamiento que solo se repita de forma causal.</span><span class="sxs-lookup"><span data-stu-id="95995-166">Avoid binding together behavior that is only coincidentally repetitive.</span></span> <span data-ttu-id="95995-167">Por ejemplo, solo porque dos constantes diferentes tengan el mismo valor no significa que debería tener solo una constante, si conceptualmente se refieren a cosas diferentes.</span><span class="sxs-lookup"><span data-stu-id="95995-167">For example, just because two different constants both have the same value, that doesn't mean you should have only one constant, if conceptually they're referring to different things.</span></span>

### <a name="persistence-ignorance"></a><span data-ttu-id="95995-168">Omisión de persistencia</span><span class="sxs-lookup"><span data-stu-id="95995-168">Persistence ignorance</span></span>

<span data-ttu-id="95995-169">La **Omisión de persistencia** (PI) hace referencia a los tipos que se deben conservar, pero cuyo código no se ve afectado por la elección de la tecnología de persistencia.</span><span class="sxs-lookup"><span data-stu-id="95995-169">**Persistence ignorance** (PI) refers to types that need to be persisted, but whose code is unaffected by the choice of persistence technology.</span></span> <span data-ttu-id="95995-170">En .NET, estos tipos a veces se denominan objeto CRL estándar (POCO), ya que no necesitan heredar de una clase base concreta o implementar una interfaz determinada.</span><span class="sxs-lookup"><span data-stu-id="95995-170">Such types in .NET are sometimes referred to as Plain Old CLR Objects (POCOs), because they do not need to inherit from a particular base class or implement a particular interface.</span></span> <span data-ttu-id="95995-171">La omisión de persistencia es útil porque permite conservar el mismo modelo de negocio de varias formas, lo que ofrece flexibilidad adicional a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="95995-171">Persistence ignorance is valuable because it allows the same business model to be persisted in multiple ways, offering additional flexibility to the application.</span></span> <span data-ttu-id="95995-172">Es posible que las opciones de persistencia cambien con el tiempo, de una tecnología de base de datos a otra, o bien que se necesiten otras formas de persistencia además de las iniciales de la aplicación (por ejemplo, el uso de una caché en Redis o Azure DocumentDb además de un base de datos relacional).</span><span class="sxs-lookup"><span data-stu-id="95995-172">Persistence choices might change over time, from one database technology to another, or additional forms of persistence might be required in addition to whatever the application started with (for example, using a Redis cache or Azure DocumentDb in addition to a relational database).</span></span>

<span data-ttu-id="95995-173">Algunos ejemplos de las infracciones de este principio son estos:</span><span class="sxs-lookup"><span data-stu-id="95995-173">Some examples of violations of this principle include:</span></span>

- <span data-ttu-id="95995-174">Una clase base requerida.</span><span class="sxs-lookup"><span data-stu-id="95995-174">A required base class.</span></span>

- <span data-ttu-id="95995-175">Una implementación de interfaz requerida.</span><span class="sxs-lookup"><span data-stu-id="95995-175">A required interface implementation.</span></span>

- <span data-ttu-id="95995-176">Clases responsables de guardarse a sí mismas (por ejemplo, el patrón de registro activo).</span><span class="sxs-lookup"><span data-stu-id="95995-176">Classes responsible for saving themselves (such as the Active Record pattern).</span></span>

- <span data-ttu-id="95995-177">Constructor sin parámetros requerido.</span><span class="sxs-lookup"><span data-stu-id="95995-177">Required parameterless constructor.</span></span>

- <span data-ttu-id="95995-178">Propiedades que requieren la palabra clave virtual.</span><span class="sxs-lookup"><span data-stu-id="95995-178">Properties requiring virtual keyword.</span></span>

- <span data-ttu-id="95995-179">Atributos requeridos específicos de la persistencia.</span><span class="sxs-lookup"><span data-stu-id="95995-179">Persistence-specific required attributes.</span></span>

<span data-ttu-id="95995-180">El requisito de que las clases tengan cualquiera de las características o comportamientos anteriores agrega acoplamiento entre los tipos que se deben conservar y la elección de la tecnología de persistencia, lo que dificulta la adopción de nuevas estrategias de acceso de datos en el futuro.</span><span class="sxs-lookup"><span data-stu-id="95995-180">The requirement that classes have any of the above features or behaviors adds coupling between the types to be persisted and the choice of persistence technology, making it more difficult to adopt new data access strategies in the future.</span></span>

### <a name="bounded-contexts"></a><span data-ttu-id="95995-181">Contextos delimitados</span><span class="sxs-lookup"><span data-stu-id="95995-181">Bounded contexts</span></span>

<span data-ttu-id="95995-182">Los **contextos delimitados** son un patrón esencial en el diseño controlado por dominios.</span><span class="sxs-lookup"><span data-stu-id="95995-182">**Bounded contexts** are a central pattern in Domain-Driven Design.</span></span> <span data-ttu-id="95995-183">Proporcionan una manera de abordar la complejidad en organizaciones o aplicaciones de gran tamaño dividiéndola en módulos conceptuales independientes.</span><span class="sxs-lookup"><span data-stu-id="95995-183">They provide a way of tackling complexity in large applications or organizations by breaking it up into separate conceptual modules.</span></span> <span data-ttu-id="95995-184">Después, cada módulo conceptual representa un contexto que está separado de otros contextos (por tanto, delimitado) y que puede evolucionar independientemente.</span><span class="sxs-lookup"><span data-stu-id="95995-184">Each conceptual module then represents a context which is separated from other contexts (hence, bounded), and can evolve independently.</span></span> <span data-ttu-id="95995-185">Idealmente, cada contexto delimitado debería poder elegir sus propios nombres para los conceptos que contiene y tener acceso exclusivo a su propio almacén de persistencia.</span><span class="sxs-lookup"><span data-stu-id="95995-185">Each bounded context should ideally be free to choose its own names for concepts within it, and should have exclusive access to its own persistence store.</span></span>

<span data-ttu-id="95995-186">Como mínimo, las aplicaciones web individuales deberían intentar ser su propio contexto delimitado, con su propio almacén de persistencia para su modelo de negocios, en lugar de compartir una base de datos con otras aplicaciones.</span><span class="sxs-lookup"><span data-stu-id="95995-186">At a minimum, individual web applications should strive to be their own bounded context, with their own persistence store for their business model, rather than sharing a database with other applications.</span></span> <span data-ttu-id="95995-187">La comunicación entre los contextos delimitados se realiza a través de interfaces de programación, en lugar de una base de datos compartida, lo que permite que la lógica de negocios y los eventos se produzcan en respuesta a los cambios que tienen lugar.</span><span class="sxs-lookup"><span data-stu-id="95995-187">Communication between bounded contexts occurs through programmatic interfaces, rather than through a shared database, which allows for business logic and events to take place in response to changes that take place.</span></span> <span data-ttu-id="95995-188">Los contextos delimitados se asignan estrechamente a los microservicios que, idealmente, también se implementan como sus propios contextos delimitados individuales.</span><span class="sxs-lookup"><span data-stu-id="95995-188">Bounded contexts map closely to microservices, which also are ideally implemented as their own individual bounded contexts.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="95995-189">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="95995-189">Additional resources</span></span>

* [<span data-ttu-id="95995-190">Patrones de diseño de JAVA: principios</span><span class="sxs-lookup"><span data-stu-id="95995-190">JAVA Design Patterns: Principles</span></span>](https://java-design-patterns.com/principles/)
* <span data-ttu-id="95995-191">[Bounded Context](https://martinfowler.com/bliki/BoundedContext.html) (Contexto delimitado)</span><span class="sxs-lookup"><span data-stu-id="95995-191">[Bounded Context](https://martinfowler.com/bliki/BoundedContext.html)</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="95995-192">[Anterior](choose-between-traditional-web-and-single-page-apps.md)
>[Siguiente](common-web-application-architectures.md)</span><span class="sxs-lookup"><span data-stu-id="95995-192">[Previous](choose-between-traditional-web-and-single-page-apps.md)
[Next](common-web-application-architectures.md)</span></span>