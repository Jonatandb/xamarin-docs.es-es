---
title: Integraciones de ejemplo
ms.prod: xamarin
ms.assetid: 327DAD2E-1F76-4EB5-BCD0-9E7384D99E48
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 458ebd0d47b3728f6207b30ddaf839ed5509dceb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2018
---
# <a name="sample-integrations"></a>Integraciones de ejemplo

## <a name="sample-integrations"></a>Integraciones de ejemplo

Consulte la [cocina receptor] [ KitchenSink] ejemplo para obtener un ejemplo de una integración. Simplemente compilar `KitchenSink.sln` en Visual Studio para Mac o Visual Studio y a continuación, abra `KitchenSink.workbook`.

[![Captura de pantalla de integración de receptor de cocina](samples-images/kitchensinkintegrationscreenshot.png)](samples-images/kitchensinkintegrationscreenshot.png#lightbox)

El ejemplo de receptor de cocina muestra ambos conjuntos de conceptos:

* Las partes de representación muestran cómo usar `RepresentationManager` para mejorar la presentación utilizando las representaciones integradas.
* El `Person` objeto y el representador de JavaScript asociado muestran cómo utilizar `ISerializableObject` sin tener que pasar a través de un proveedor de representación.

Consulte también [SkiaSharp] [ skiasharp] para obtener un ejemplo del mundo real de integración de base de datos que usa las existentes [representaciones](~/tools/workbooks/sdk/representations.md) proporcionada por los libros de Xamarin para representar sus tipos.

[KitchenSink]: https://github.com/xamarin/Workbooks/tree/master/SDK/Samples/KitchenSink
[skiasharp]: https://github.com/mono/SkiaSharp/tree/master/source/SkiaSharp.Workbooks