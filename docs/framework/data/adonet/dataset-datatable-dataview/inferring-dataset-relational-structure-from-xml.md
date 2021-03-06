---
title: Inferir una estructura relacional de un conjunto de datos a partir de XML
ms.date: 03/30/2017
ms.assetid: cd2f41c6-6785-420e-aa43-3ceb0bdccdce
ms.openlocfilehash: 1c8325d7ed52fea7397a7b5aa8744bdfa90b2c6e
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2019
ms.locfileid: "70785322"
---
# <a name="inferring-dataset-relational-structure-from-xml"></a>Inferir una estructura relacional de un conjunto de datos a partir de XML
La estructura relacional, o esquema, de un <xref:System.Data.DataSet> está compuesta por tablas, columnas, restricciones y relaciones. Al cargar un <xref:System.Data.DataSet> desde XML es posible predefinir el esquema, o bien crearlo, de forma explícita o por inferencia, a partir del código XML que se carga. Para obtener más información sobre cómo cargar el esquema y el <xref:System.Data.DataSet> contenido de un desde XML, vea [cargar un conjunto de datos desde XML](loading-a-dataset-from-xml.md) y [cargar información de esquema de conjunto de datos desde XML](loading-dataset-schema-information-from-xml.md).  
  
 Si el esquema de un <xref:System.Data.DataSet> se crea a partir de XML, el método preferido consiste en especificar explícitamente el esquema mediante el lenguaje de definición de esquemas XML (XSD) (tal y como se describe en [derivar una estructura relacional de conjunto de objetos del esquema XML (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)) o en el XML: datos reducidos (XDR). Si no hay ningún esquema XML o XDR disponible en XML, el esquema del <xref:System.Data.DataSet> se puede deducir de la estructura de los elementos y atributos XML.  
  
 En esta sección se describen las reglas para deducir el esquema de un <xref:System.Data.DataSet>; para ello se muestran los elementos y atributos XML y su estructura, así como el esquema deducido del <xref:System.Data.DataSet> resultante.  
  
 No todos los atributos presentes en un documento XML deben incluirse en el proceso de deducción. Los atributos calificados por el espacio de nombres pueden incluir metadatos importantes para el documento XML, pero no para el esquema del <xref:System.Data.DataSet>. Mediante <xref:System.Data.DataSet.InferXmlSchema%2A> es posible especificar determinados espacios de nombres que desea pasar por alto durante el proceso de inferencia. Para obtener más información, vea [cargar información de esquema de conjunto de datos desde XML](loading-dataset-schema-information-from-xml.md).  
  
## <a name="in-this-section"></a>En esta sección  
 [Resumen del proceso de inferencia del esquema de DataSet](summary-of-the-dataset-schema-inference-process.md)  
 Proporciona un resumen general de las reglas para deducir el esquema de un <xref:System.Data.DataSet> a partir de XML.  
  
 [Inferencia de tablas](inferring-tables.md)  
 Describe los elementos XML que se deducen como tablas en un <xref:System.Data.DataSet>.  
  
 [Inferencia de columnas](inferring-columns.md)  
 Describe los elementos y atributos XML que se deducen como columnas de tabla.  
  
 [Inferencia de relaciones](inferring-relationships.md)  
 Describe los objetos <xref:System.Data.DataRelation> y <xref:System.Data.ForeignKeyConstraint> creados para tablas deducidas y anidadas.  
  
 [Inferencia de texto de elemento](inferring-element-text.md)  
 Describe las columnas que se crean para texto en los elementos XML y explica cuándo se pasa por alto el texto de los elementos XML.  
  
 [Limitaciones de inferencia](inference-limitations.md)  
 Describe las limitaciones de la inferencia del esquema.  
  
## <a name="related-sections"></a>Secciones relacionadas  
 [Usar XML en un conjunto de datos](using-xml-in-a-dataset.md)  
 Describe cómo interactúa el objeto <xref:System.Data.DataSet> con los datos XML.  
  
 [Derivación de una estructura relacional de un conjunto de datos a partir de un esquema XML (XSD)](deriving-dataset-relational-structure-from-xml-schema-xsd.md)  
 Describe la estructura relacional o un esquema de un <xref:System.Data.DataSet> que se crea a partir del esquema del lenguaje de definición de esquemas XML (XSD).  
  
 [Información general sobre ADO.NET](../ado-net-overview.md)  
 Describe la arquitectura y los componentes de ADO.NET, así como su uso para obtener acceso a orígenes de datos existentes y para administrar los datos de aplicación.  
  
## <a name="see-also"></a>Vea también

- [Información general sobre ADO.NET](../ado-net-overview.md)
