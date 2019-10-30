---
title: Enlace de datos de WPF con LINQ to XML
ms.date: 10/22/2019
ms.topic: conceptual
ms.openlocfilehash: c423ad9c8069b78b2e69a88d25d8e12bd3a3a1b7
ms.sourcegitcommit: 82f94a44ad5c64a399df2a03fa842db308185a76
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2019
ms.locfileid: "72921027"
---
# <a name="overview-of-wpf-data-binding-with-linq-to-xml"></a><span data-ttu-id="db64f-102">Información general sobre el enlace de datos de WPF con LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="db64f-102">Overview of WPF data binding with LINQ to XML</span></span>

<span data-ttu-id="db64f-103">En este artículo se presentan las características de enlace de datos dinámicos en el espacio de nombres <xref:System.Xml.Linq>.</span><span class="sxs-lookup"><span data-stu-id="db64f-103">This article introduces the dynamic data binding features in the <xref:System.Xml.Linq> namespace.</span></span> <span data-ttu-id="db64f-104">Estas características se pueden usar como origen de datos para los elementos de la interfaz de usuario (IU) en aplicaciones de Windows Presentation Foundation (WPF).</span><span class="sxs-lookup"><span data-stu-id="db64f-104">These features can be used as a data source for user interface (UI) elements in Windows Presentation Foundation (WPF) apps.</span></span> <span data-ttu-id="db64f-105">Este escenario se basa en las *propiedades dinámicas* especiales de <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> y <xref:System.Xml.Linq.XElement?displayProperty=fullName>.</span><span class="sxs-lookup"><span data-stu-id="db64f-105">This scenario relies upon special *dynamic properties* of <xref:System.Xml.Linq.XAttribute?displayProperty=fullName> and <xref:System.Xml.Linq.XElement?displayProperty=fullName>.</span></span>

## <a name="xaml-and-linq-to-xml"></a><span data-ttu-id="db64f-106">XAML y LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="db64f-106">XAML and LINQ to XML</span></span>

<span data-ttu-id="db64f-107">El lenguaje XAML es un dialecto XML creado por Microsoft para admitir tecnologías de .NET.</span><span class="sxs-lookup"><span data-stu-id="db64f-107">The Extensible Application Markup Language (XAML) is an XML dialect created by Microsoft to support .NET technologies.</span></span> <span data-ttu-id="db64f-108">Se usa en WPF para representar los elementos de la interfaz de usuario y las características relacionadas como, por ejemplo, el enlace de datos y eventos.</span><span class="sxs-lookup"><span data-stu-id="db64f-108">It is used in WPF to represent user interface elements and related features, such as events and data binding.</span></span> <span data-ttu-id="db64f-109">En Windows Workflow Foundation, XAML se utiliza para representar la estructura del programa, como por ejemplo el control de programas (*flujos de trabajo*).</span><span class="sxs-lookup"><span data-stu-id="db64f-109">In Windows Workflow Foundation, XAML is used to represent program structure, such as program control (*workflows*).</span></span> <span data-ttu-id="db64f-110">XAML permite que los aspectos declarativos de una tecnología se separen del código de procedimiento relacionado que define el comportamiento más individualizado de un programa.</span><span class="sxs-lookup"><span data-stu-id="db64f-110">XAML enables the declarative aspects of a technology to be separated from the related procedural code that defines the more individualized behavior of a program.</span></span>

<span data-ttu-id="db64f-111">XAML puede interactuar con LINQ to XML de dos grandes formas:</span><span class="sxs-lookup"><span data-stu-id="db64f-111">There are two broad ways that XAML and LINQ to XML can interact:</span></span>

- <span data-ttu-id="db64f-112">Como los archivos XAML son código XML bien formado, se pueden consultar y manipular mediante tecnologías XML como LINQ to XML.</span><span class="sxs-lookup"><span data-stu-id="db64f-112">Because XAML files are well-formed XML, they can be queried and manipulated through XML technologies such as LINQ to XML.</span></span>

- <span data-ttu-id="db64f-113">Como las consultas de LINQ to XML representan un origen de datos, esas consultas se pueden usar como origen de datos en el enlace de datos para elementos de IU de WPF.</span><span class="sxs-lookup"><span data-stu-id="db64f-113">Because LINQ to XML queries represent a source of data, these queries can be used as a data source for data binding for WPF UI elements.</span></span>

<span data-ttu-id="db64f-114">Esta documentación describe el segundo escenario.</span><span class="sxs-lookup"><span data-stu-id="db64f-114">This documentation describes the second scenario.</span></span>

## <a name="data-binding-in-the-windows-presentation-foundation"></a><span data-ttu-id="db64f-115">Enlace de datos en el Windows Presentation Foundation</span><span class="sxs-lookup"><span data-stu-id="db64f-115">Data binding in the Windows Presentation Foundation</span></span>

<span data-ttu-id="db64f-116">El enlace de datos en WPF permite a un elemento de interfaz de usuario asociar una de sus propiedades con un origen de datos.</span><span class="sxs-lookup"><span data-stu-id="db64f-116">WPF data binding enables a UI element to associate one of its properties with a data source.</span></span> <span data-ttu-id="db64f-117">Un ejemplo sencillo de esto es <xref:System.Windows.Controls.Label>, cuyo texto presenta el valor de una propiedad pública en un objeto definido por el usuario.</span><span class="sxs-lookup"><span data-stu-id="db64f-117">A simple example of this is a <xref:System.Windows.Controls.Label> whose text presents the value of a public property in a user-defined object.</span></span> <span data-ttu-id="db64f-118">El enlace de datos de WPF depende de los siguientes componentes:</span><span class="sxs-lookup"><span data-stu-id="db64f-118">WPF data binding relies on the following components:</span></span>

|<span data-ttu-id="db64f-119">Componente</span><span class="sxs-lookup"><span data-stu-id="db64f-119">Component</span></span>|<span data-ttu-id="db64f-120">Descripción</span><span class="sxs-lookup"><span data-stu-id="db64f-120">Description</span></span>|
|---------------|-----------------|
|<span data-ttu-id="db64f-121">Destino de enlace</span><span class="sxs-lookup"><span data-stu-id="db64f-121">Binding target</span></span>|<span data-ttu-id="db64f-122">El elemento de IU debe estar asociado con el origen de datos.</span><span class="sxs-lookup"><span data-stu-id="db64f-122">The UI element to be associated with the data source.</span></span> <span data-ttu-id="db64f-123">Los elementos visuales de WPF se derivan de la clase <xref:System.Windows.UIElement>.</span><span class="sxs-lookup"><span data-stu-id="db64f-123">Visual elements in WPF are derived from the <xref:System.Windows.UIElement> class.</span></span>|
|<span data-ttu-id="db64f-124">Propiedad de destino</span><span class="sxs-lookup"><span data-stu-id="db64f-124">Target property</span></span>|<span data-ttu-id="db64f-125">*Propiedad de dependencia* del destino de enlace que refleja el valor del origen de enlace de datos.</span><span class="sxs-lookup"><span data-stu-id="db64f-125">The *dependency property* of the binding target that reflects the value of the data-binding source.</span></span> <span data-ttu-id="db64f-126">Las propiedades de dependencia son compatibles directamente con la clase <xref:System.Windows.DependencyObject>, de la que se deriva <xref:System.Windows.UIElement>.</span><span class="sxs-lookup"><span data-stu-id="db64f-126">Dependency properties are directly supported by the <xref:System.Windows.DependencyObject> class, which <xref:System.Windows.UIElement> derives from.</span></span>|
|<span data-ttu-id="db64f-127">Origen de enlace</span><span class="sxs-lookup"><span data-stu-id="db64f-127">Binding source</span></span>|<span data-ttu-id="db64f-128">Objeto de origen para uno o más valores que se proporcionan al elemento de la IU para la presentación.</span><span class="sxs-lookup"><span data-stu-id="db64f-128">The source object for one or more values that are supplied to the UI element for presentation.</span></span> <span data-ttu-id="db64f-129">WPF admite automáticamente los siguientes tipos como orígenes de enlace: objetos CLR, objetos de datos ADO.NET, datos XML (de consultas XPath o LINQ to XML) u otro <xref:System.Windows.DependencyObject>.</span><span class="sxs-lookup"><span data-stu-id="db64f-129">WPF automatically supports the following types as binding sources: CLR objects, ADO.NET data objects, XML data (from XPath or LINQ to XML queries), or another <xref:System.Windows.DependencyObject>.</span></span>|
|<span data-ttu-id="db64f-130">Ruta de acceso de origen</span><span class="sxs-lookup"><span data-stu-id="db64f-130">Source path</span></span>|<span data-ttu-id="db64f-131">Propiedad del origen de enlace que se resuelve en valor o conjunto de valores que debe enlazar.</span><span class="sxs-lookup"><span data-stu-id="db64f-131">The property of the binding source that resolves to the value or set of values that is to be bound.</span></span>|

<span data-ttu-id="db64f-132">Una propiedad de dependencia es un concepto específico de WPF que representa una propiedad calculada dinámicamente de un elemento de IU.</span><span class="sxs-lookup"><span data-stu-id="db64f-132">A dependency property is a concept specific to WPF that represent a dynamically computed property of a UI element.</span></span> <span data-ttu-id="db64f-133">Por ejemplo, las propiedades de dependencia suelen presentar valores predeterminados o valores proporcionados por un elemento primario.</span><span class="sxs-lookup"><span data-stu-id="db64f-133">For example, dependency properties often have default values or values that are provided by a parent element.</span></span> <span data-ttu-id="db64f-134">Estas propiedades especiales están asistidas por instancias de la clase <xref:System.Windows.DependencyProperty> (y no campos como en las propiedades estándar).</span><span class="sxs-lookup"><span data-stu-id="db64f-134">These special properties are backed by instances of the <xref:System.Windows.DependencyProperty> class (and not fields as with standard properties).</span></span> <span data-ttu-id="db64f-135">Para obtener más información sobre las propiedades de dependencia, vea [Información general sobre las propiedades de dependencia](/dotnet/framework/wpf/advanced/dependency-properties-overview).</span><span class="sxs-lookup"><span data-stu-id="db64f-135">For more information, see [Dependency Properties Overview](/dotnet/framework/wpf/advanced/dependency-properties-overview).</span></span>

### <a name="dynamic-data-binding-in-wpf"></a><span data-ttu-id="db64f-136">Enlace de datos dinámicos en WPF</span><span class="sxs-lookup"><span data-stu-id="db64f-136">Dynamic data binding in WPF</span></span>

<span data-ttu-id="db64f-137">De forma predeterminada el enlace de datos se produce solamente cuando se inicializa el elemento de IU de destino.</span><span class="sxs-lookup"><span data-stu-id="db64f-137">By default, data binding occurs only when the target UI element is initialized.</span></span> <span data-ttu-id="db64f-138">Se trata de un enlace *único*.</span><span class="sxs-lookup"><span data-stu-id="db64f-138">This is called *one-time* binding.</span></span> <span data-ttu-id="db64f-139">Esto es insuficiente para la mayoría de finalidades. Normalmente una solución de enlace de datos requiere que los cambios se propaguen dinámicamente en tiempo de ejecución mediante uno de los siguientes enlaces:</span><span class="sxs-lookup"><span data-stu-id="db64f-139">For most purposes, this is insufficient; typically, a data-binding solution requires that the changes be dynamically propagated at run time using one of the following:</span></span>

- <span data-ttu-id="db64f-140">El enlace *unidireccional* hace que los cambios en un lado se propaguen automáticamente.</span><span class="sxs-lookup"><span data-stu-id="db64f-140">*One-way* binding causes the changes to one side to be propagated automatically.</span></span> <span data-ttu-id="db64f-141">En la mayoría de casos, los cambios en el origen se reflejan en el destino, aunque la operación inversa puede resultar útil a veces.</span><span class="sxs-lookup"><span data-stu-id="db64f-141">Most commonly, changes to the source are reflected in the target, but the reverse can sometimes be useful.</span></span>

- <span data-ttu-id="db64f-142">En el enlace *bidireccional*, los cambios en el origen se propagan automáticamente al destino y los cambios en el destino se propagan automáticamente al origen.</span><span class="sxs-lookup"><span data-stu-id="db64f-142">In *two-way* binding, changes to the source are automatically propagated to the target, and changes to the target are automatically propagated to the source.</span></span>

<span data-ttu-id="db64f-143">Para que se produzca el enlace unidireccional o bidireccional, el origen debe implementar un mecanismo de notificación de cambio, por ejemplo implementando la interfaz <xref:System.ComponentModel.INotifyPropertyChanged> o usando un patrón *PropertyNameChanged* para cada propiedad admitida.</span><span class="sxs-lookup"><span data-stu-id="db64f-143">For one-way or two-way binding to occur, the source must implement a change notification mechanism, for example by implementing the <xref:System.ComponentModel.INotifyPropertyChanged> interface or by using a *PropertyNameChanged* pattern for each property supported.</span></span>

<span data-ttu-id="db64f-144">Para más información sobre el enlace de datos, consulte [Enlace de datos (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).</span><span class="sxs-lookup"><span data-stu-id="db64f-144">For more information about data binding in WPF, see [Data Binding (WPF)](/dotnet/framework/wpf/data/data-binding-wpf).</span></span>

## <a name="dynamic-properties-in-linq-to-xml-classes"></a><span data-ttu-id="db64f-145">Propiedades dinámicas en clases de LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="db64f-145">Dynamic properties in LINQ to XML classes</span></span>

<span data-ttu-id="db64f-146">La mayoría de las clases de LINQ to XML no se consideran orígenes de datos dinámicos de WPF adecuados.</span><span class="sxs-lookup"><span data-stu-id="db64f-146">Most LINQ to XML classes do not qualify as proper WPF dynamic data sources.</span></span> <span data-ttu-id="db64f-147">Parte de la información más útil solo está disponible a través de métodos (y no de propiedades) y las propiedades de esas clases no implementan las notificaciones de cambio.</span><span class="sxs-lookup"><span data-stu-id="db64f-147">Some of the most useful information is available only through methods, not properties, and properties in these classes do not implement change notifications.</span></span> <span data-ttu-id="db64f-148">Para admitir el enlace de datos de WPF, LINQ to XML expone un conjunto de *propiedades dinámicas*.</span><span class="sxs-lookup"><span data-stu-id="db64f-148">To support WPF data binding, LINQ to XML exposes a set of *dynamic properties*.</span></span>

<span data-ttu-id="db64f-149">Estas propiedades dinámicas son propiedades en tiempo de ejecución especiales que duplican la funcionalidad de métodos y propiedades existentes en las clases <xref:System.Xml.Linq.XAttribute> y <xref:System.Xml.Linq.XElement>.</span><span class="sxs-lookup"><span data-stu-id="db64f-149">These dynamic properties are special run-time properties that duplicate the functionality of existing methods and properties in the <xref:System.Xml.Linq.XAttribute> and <xref:System.Xml.Linq.XElement> classes.</span></span> <span data-ttu-id="db64f-150">Se agregaron a esas clases solamente para permitirles actuar como orígenes de datos dinámicos de WPF.</span><span class="sxs-lookup"><span data-stu-id="db64f-150">They were added to these classes solely to enable them to act as dynamic data sources for WPF.</span></span> <span data-ttu-id="db64f-151">Para satisfacer esta necesidad, todas las propiedades dinámicas implementan notificaciones de cambios.</span><span class="sxs-lookup"><span data-stu-id="db64f-151">To meet this need, all these dynamic properties implement change notifications.</span></span> <span data-ttu-id="db64f-152">En la siguiente sección se describe una referencia detallada de esas propiedades dinámicas, [Propiedades dinámicas de LINQ to XML](linq-to-xml-dynamic-properties.md).</span><span class="sxs-lookup"><span data-stu-id="db64f-152">A detailed reference for these dynamic properties is provided in the next section, [LINQ to XML Dynamic Properties](linq-to-xml-dynamic-properties.md).</span></span>

> [!NOTE]
> <span data-ttu-id="db64f-153">Muchas de las propiedades públicas estándar, encontradas en las diferentes clases del espacio de nombres <xref:System.Xml.Linq>, se pueden usar para el enlace de datos único.</span><span class="sxs-lookup"><span data-stu-id="db64f-153">Many of the standard public properties, found in the various classes in the <xref:System.Xml.Linq> namespace, can be used for one-time data binding.</span></span> <span data-ttu-id="db64f-154">No obstante, recuerde que ni el origen ni el destino se actualizarán dinámicamente con este esquema.</span><span class="sxs-lookup"><span data-stu-id="db64f-154">However, remember that neither the source nor the target will be dynamically updated under this scheme.</span></span>

### <a name="access-dynamic-properties"></a><span data-ttu-id="db64f-155">Acceso a propiedades dinámicas</span><span class="sxs-lookup"><span data-stu-id="db64f-155">Access dynamic properties</span></span>

<span data-ttu-id="db64f-156">No se puede tener acceso a las propiedades dinámicas de las clases <xref:System.Xml.Linq.XAttribute> y <xref:System.Xml.Linq.XElement> como propiedades estándar.</span><span class="sxs-lookup"><span data-stu-id="db64f-156">The dynamic properties in the <xref:System.Xml.Linq.XAttribute> and <xref:System.Xml.Linq.XElement> classes cannot be accessed like standard properties.</span></span> <span data-ttu-id="db64f-157">Por ejemplo, en los lenguajes conformes a CLR como C#, no se puede:</span><span class="sxs-lookup"><span data-stu-id="db64f-157">For example, in CLR-compliant languages such as C#, they cannot be:</span></span>

- <span data-ttu-id="db64f-158">Tener acceso a ellas directamente en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="db64f-158">Accessed directly at compile time.</span></span> <span data-ttu-id="db64f-159">Las propiedades dinámicas son invisibles para el compilador y para Visual Studio IntelliSense.</span><span class="sxs-lookup"><span data-stu-id="db64f-159">Dynamic properties are invisible to the compiler and to Visual Studio IntelliSense.</span></span>

- <span data-ttu-id="db64f-160">Detectarlas o tener acceso a ellas en tiempo de ejecución usando la reflexión .NET.</span><span class="sxs-lookup"><span data-stu-id="db64f-160">Discovered or accessed at run time using .NET reflection.</span></span> <span data-ttu-id="db64f-161">Incluso en tiempo de ejecución, no son propiedades en el sentido básico de CLR.</span><span class="sxs-lookup"><span data-stu-id="db64f-161">Even at run time, they are not properties in the basic CLR sense.</span></span>

<span data-ttu-id="db64f-162">En C#, solamente se puede tener acceso a las propiedades dinámicas en tiempo de ejecución a través de los servicios proporcionados por el espacio de nombres <xref:System.ComponentModel>.</span><span class="sxs-lookup"><span data-stu-id="db64f-162">In C#, dynamic properties can only be accessed at run time through facilities provided by the <xref:System.ComponentModel> namespace.</span></span>

<span data-ttu-id="db64f-163">Sin embargo, en un origen XML se puede tener acceso a las propiedades dinámicas a través de una notación sencilla de la siguiente forma:</span><span class="sxs-lookup"><span data-stu-id="db64f-163">In contrast, however, in an XML source dynamic properties can be accessed through a straightforward notation in the following form:</span></span>

```xml
<object>.<dynamic-property>
```

<span data-ttu-id="db64f-164">Las propiedades dinámicas para esas dos clases se resuelven en un valor que se puede usar directamente o en un indizador que se debe suministrar con un índice para obtener el valor o la colección de valores resultantes.</span><span class="sxs-lookup"><span data-stu-id="db64f-164">The dynamic properties for these two classes either resolve to a value that can be used directly, or to an indexer that must be supplied with an index to obtain the resulting value or collection of values.</span></span> <span data-ttu-id="db64f-165">La sintaxis posterior toma la forma:</span><span class="sxs-lookup"><span data-stu-id="db64f-165">The latter syntax takes the form:</span></span>

```xml
<object>.<dynamic-property>[<index-value>]
```

<span data-ttu-id="db64f-166">Para obtener más información, consulte [Propiedades dinámicas de LINQ to XML](linq-to-xml-dynamic-properties.md).</span><span class="sxs-lookup"><span data-stu-id="db64f-166">For more information, see [LINQ to XML Dynamic Properties](linq-to-xml-dynamic-properties.md).</span></span>

<span data-ttu-id="db64f-167">Para implementar el enlace dinámico en WPF, se usarán las propiedades dinámicas con soluciones proporcionadas por el espacio de nombres <xref:System.Windows.Data>, especialmente la clase <xref:System.Windows.Data.Binding>.</span><span class="sxs-lookup"><span data-stu-id="db64f-167">To implement WPF dynamic binding, dynamic properties will be used with facilities provided by the <xref:System.Windows.Data> namespace, most notably the <xref:System.Windows.Data.Binding> class.</span></span>

## <a name="see-also"></a><span data-ttu-id="db64f-168">Vea también</span><span class="sxs-lookup"><span data-stu-id="db64f-168">See also</span></span>

- [<span data-ttu-id="db64f-169">Enlace de datos WPF con LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="db64f-169">WPF Data Binding with LINQ to XML</span></span>](wpf-data-binding-with-linq-to-xml-overview.md)
- [<span data-ttu-id="db64f-170">Propiedades dinámicas de LINQ to XML</span><span class="sxs-lookup"><span data-stu-id="db64f-170">LINQ to XML Dynamic Properties</span></span>](linq-to-xml-dynamic-properties.md)
- [<span data-ttu-id="db64f-171">XAML en WPF</span><span class="sxs-lookup"><span data-stu-id="db64f-171">XAML in WPF</span></span>](/dotnet/framework/wpf/advanced/xaml-in-wpf)
- [<span data-ttu-id="db64f-172">Enlace de datos (WPF)</span><span class="sxs-lookup"><span data-stu-id="db64f-172">Data Binding (WPF)</span></span>](/dotnet/framework/wpf/data/data-binding-wpf)
- [<span data-ttu-id="db64f-173">Uso del marcado de flujo de trabajo</span><span class="sxs-lookup"><span data-stu-id="db64f-173">Using Workflow Markup</span></span>](http://go.microsoft.com/fwlink/?LinkId=98685)