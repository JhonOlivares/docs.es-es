---
title: Uso de las clases de enumeración en lugar de los tipos de enumeración
description: Arquitectura de microservicios de .NET para aplicaciones .NET en contenedor | Obtenga más información sobre cómo se pueden usar las clases de enumeración en lugar de las enumeraciones como una forma de resolver algunas limitaciones de estas últimas.
ms.date: 10/08/2018
ms.openlocfilehash: 10b4c2f7b9f079ed535111e65b8154791f6575cd
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68674192"
---
# <a name="use-enumeration-classes-instead-of-enum-types"></a><span data-ttu-id="51880-103">Uso de las clases de enumeración en lugar de los tipos de enumeración</span><span class="sxs-lookup"><span data-stu-id="51880-103">Use enumeration classes instead of enum types</span></span>

<span data-ttu-id="51880-104">Las [enumeraciones](../../../../docs/csharp/language-reference/keywords/enum.md) (o *tipos enum* abreviado) son un contenedor de lenguaje fino alrededor de un tipo entero.</span><span class="sxs-lookup"><span data-stu-id="51880-104">[Enumerations](../../../../docs/csharp/language-reference/keywords/enum.md) (or *enum types* for short) are a thin language wrapper around an integral type.</span></span> <span data-ttu-id="51880-105">Es posible que quiera limitar su uso al momento en que almacena un valor de un conjunto cerrado de valores.</span><span class="sxs-lookup"><span data-stu-id="51880-105">You might want to limit their use to when you are storing one value from a closed set of values.</span></span> <span data-ttu-id="51880-106">La clasificación basada en tamaños (pequeño, mediano, grande) es un buen ejemplo.</span><span class="sxs-lookup"><span data-stu-id="51880-106">Classification based on sizes (small, medium, large) is a good example.</span></span> <span data-ttu-id="51880-107">Usar las enumeraciones para el flujo de control o abstracciones más sólidas puede producir un [problema en el código](https://deviq.com/code-smells/).</span><span class="sxs-lookup"><span data-stu-id="51880-107">Using enums for control flow or more robust abstractions can be a [code smell](https://deviq.com/code-smells/).</span></span> <span data-ttu-id="51880-108">Este tipo de uso da lugar a código frágil con muchas instrucciones de flujo de control que comprueban los valores de la enumeración.</span><span class="sxs-lookup"><span data-stu-id="51880-108">This type of usage leads to fragile code with many control flow statements checking values of the enum.</span></span>

<span data-ttu-id="51880-109">En su lugar, puede crear clases de enumeración que habilitan todas las características enriquecidas de un lenguaje orientado a objetos.</span><span class="sxs-lookup"><span data-stu-id="51880-109">Instead, you can create Enumeration classes that enable all the rich features of an object-oriented language.</span></span>

<span data-ttu-id="51880-110">Sin embargo, esto no es un tema crítico y, en muchos casos, por simplicidad, puede seguir usando [tipos enum](../../../csharp/language-reference/keywords/enum.md) normales si lo prefiere.</span><span class="sxs-lookup"><span data-stu-id="51880-110">However, this isn't a critical topic and in many cases, for simplicity, you can still use regular [enum types](../../../csharp/language-reference/keywords/enum.md) if that's your preference.</span></span> <span data-ttu-id="51880-111">En cualquier caso, el uso de las clases de enumeración está más relacionado con los conceptos de tipo empresarial.</span><span class="sxs-lookup"><span data-stu-id="51880-111">Anyway, the use of enumeration classes is more related to business-related concepts.</span></span>

## <a name="implement-an-enumeration-base-class"></a><span data-ttu-id="51880-112">Implementación de una clase base de enumeración</span><span class="sxs-lookup"><span data-stu-id="51880-112">Implement an Enumeration base class</span></span>

<span data-ttu-id="51880-113">El microservicio de pedidos en eShopOnContainers proporciona una implementación de clase base de enumeración de ejemplo, como se muestra en el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="51880-113">The ordering microservice in eShopOnContainers provides a sample Enumeration base class implementation, as shown in the following example:</span></span>

```csharp
public abstract class Enumeration : IComparable
{
    public string Name { get; private set; }

    public int Id { get; private set; }

    protected Enumeration(int id, string name) 
    {
        Id = id; 
        Name = name; 
    }

    public override string ToString() => Name;

    public static IEnumerable<T> GetAll<T>() where T : Enumeration
    {
        var fields = typeof(T).GetFields(BindingFlags.Public | 
                                         BindingFlags.Static | 
                                         BindingFlags.DeclaredOnly); 

        return fields.Select(f => f.GetValue(null)).Cast<T>();
    }

    public override bool Equals(object obj) 
    {
        var otherValue = obj as Enumeration; 

        if (otherValue == null) 
            return false;

        var typeMatches = GetType().Equals(obj.GetType());
        var valueMatches = Id.Equals(otherValue.Id);

        return typeMatches && valueMatches;
    }

    public int CompareTo(object other) => Id.CompareTo(((Enumeration)other).Id); 

    // Other utility methods ... 
}
```

<span data-ttu-id="51880-114">Puede usar esta clase como un tipo en cualquier entidad u objeto de valor, como ocurre con la clase `CardType` : `Enumeration` siguiente:</span><span class="sxs-lookup"><span data-stu-id="51880-114">You can use this class as a type in any entity or value object, as for the following `CardType` : `Enumeration` class:</span></span>

```csharp
public class CardType : Enumeration
{
    public static CardType Amex = new CardType(1, "Amex");
    public static CardType Visa = new CardType(2, "Visa");
    public static CardType MasterCard = new CardType(3, "MasterCard");

    public CardType(int id, string name)
        : base(id, name)
    {
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="51880-115">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="51880-115">Additional resources</span></span>

- <span data-ttu-id="51880-116">**Enum’s are evil—update** \ (Las enumeraciones son contraproducentes: actualice)</span><span class="sxs-lookup"><span data-stu-id="51880-116">**Enum’s are evil—update** \</span></span>
  <https://www.planetgeek.ch/2009/07/01/enums-are-evil/>

- <span data-ttu-id="51880-117">**Daniel Hardman. How Enums Spread Disease — And How To Cure It** \ (Cómo las enumeraciones contagian la enfermedad y cómo curarla)</span><span class="sxs-lookup"><span data-stu-id="51880-117">**Daniel Hardman. How Enums Spread Disease — And How To Cure It** \</span></span>
  <https://codecraft.co/2012/10/29/how-enums-spread-disease-and-how-to-cure-it/>

- <span data-ttu-id="51880-118">**Jimmy Bogard. Enumeration classes** \ (Clases de enumeración)</span><span class="sxs-lookup"><span data-stu-id="51880-118">**Jimmy Bogard. Enumeration classes** \</span></span>
  <https://lostechies.com/jimmybogard/2008/08/12/enumeration-classes/>

- <span data-ttu-id="51880-119">**Steve Smith. Enum Alternatives in C#**  \ (Alternativas de enumeración en C#)</span><span class="sxs-lookup"><span data-stu-id="51880-119">**Steve Smith. Enum Alternatives in C#** \</span></span>
  <https://ardalis.com/enum-alternatives-in-c>

- <span data-ttu-id="51880-120">**Enumeration.cs.**</span><span class="sxs-lookup"><span data-stu-id="51880-120">**Enumeration.cs.**</span></span> <span data-ttu-id="51880-121">Base Enumeration class in eShopOnContainers \ (Clase base de enumeración en eShopOnContainers)</span><span class="sxs-lookup"><span data-stu-id="51880-121">Base Enumeration class in eShopOnContainers \</span></span>
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/SeedWork/Enumeration.cs>

- <span data-ttu-id="51880-122">**CardType.cs**.</span><span class="sxs-lookup"><span data-stu-id="51880-122">**CardType.cs**.</span></span> <span data-ttu-id="51880-123">Sample Enumeration class in eShopOnContainers (CardType.cs. Clase de enumeración de ejemplo en eShopOnContainers)</span><span class="sxs-lookup"><span data-stu-id="51880-123">Sample Enumeration class in eShopOnContainers.</span></span> \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/BuyerAggregate/CardType.cs>
    
- <span data-ttu-id="51880-124">**SmartEnum**.</span><span class="sxs-lookup"><span data-stu-id="51880-124">**SmartEnum**.</span></span> <span data-ttu-id="51880-125">Ardalis - Classes to help produce strongly typed smarter enums in .NET. (Ardalis: clases para ayudar a crear enumeraciones fuertemente tipadas de manera más inteligente en .NET)</span><span class="sxs-lookup"><span data-stu-id="51880-125">Ardalis - Classes to help produce strongly typed smarter enums in .NET.</span></span> \
  <https://www.nuget.org/packages/Ardalis.SmartEnum/>

>[!div class="step-by-step"]
><span data-ttu-id="51880-126">[Anterior](implement-value-objects.md)
>[Siguiente](domain-model-layer-validations.md)</span><span class="sxs-lookup"><span data-stu-id="51880-126">[Previous](implement-value-objects.md)
[Next](domain-model-layer-validations.md)</span></span>