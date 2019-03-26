---
title: Representadores rápidos de Xamarin.Forms
description: Este artículo presentan a los representadores rápidos, que reducen la inflación y los costos de representación de un control de Xamarin.Forms en Android mediante la reducción de la jerarquía de control nativo resultante.
ms.prod: xamarin
ms.assetid: 097f87f2-d891-4f3c-be02-fb7d195a481a
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 9a40644df6abcbbcc327b1b0c2dcb26c2dbc4db5
ms.sourcegitcommit: 247a6d00a95fd7f4cf918d923e5f357c8db56761
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58420215"
---
# <a name="xamarinforms-fast-renderers"></a>Representadores rápidos de Xamarin.Forms

![Vista previa](~/media/shared/preview.png)

_Este artículo presentan a representadores rápidos (agregados en Xamarin.Forms 2.4), que reducen la inflación y los costos de representación de un control de Xamarin.Forms en Android mediante la reducción de la jerarquía de control nativo resultante._

Tradicionalmente, la mayoría de los representadores de control original en Android se compone de dos vistas:

- Control nativo, como un `Button` o `TextView`.
- Un contenedor `ViewGroup` que controla algunas de las otras tareas, administración de gestos y el trabajo de diseño.

Sin embargo, este enfoque tiene una implicación del rendimiento que dos vistas se crean para cada control lógico, lo que resulta en un árbol visual más compleja que requiere más memoria y más capacidad de procesamiento para representar en la pantalla.

Los representadores rápidos reducen la inflación y los costos de representación de un control de Xamarin.Forms en una sola vista. Por lo tanto, en lugar de crear dos vistas y agregarlos al árbol de la vista, se crea uno solo. Esto mejora el rendimiento porque crea menos objetos, lo que significa que a su vez un árbol de vista menos complejo, y menos uso de memoria (que también tiene como resultado menos pausas de la colección de elementos no utilizados).

Representadores rápidos están disponibles para los siguientes controles en 2.4 de Xamarin.Forms en Android:

- [`Button`](xref:Xamarin.Forms.Button)
- [`Image`](xref:Xamarin.Forms.Image)
- [`Label`](xref:Xamarin.Forms.Label)
- [`Frame`](xref:Xamarin.Forms.Frame)

Funcionalmente, estos representadores rápidos no son distintos a los representadores originales. Sin embargo, que son actualmente experimentales y solo se puede usar agregando la siguiente línea de código para su `MainActivity` clase antes de llamar a `Forms.Init`:

```csharp
Forms.SetFlags("FastRenderers_Experimental");
```

> [!NOTE]
> Representadores rápidos solo son aplicables a la aplicación compatibilidad Android back-end, por lo que esta configuración se omite en las actividades de compatibilidad anterior a la aplicación.

Mejoras de rendimiento varían para cada aplicación, en función de la complejidad del diseño. Por ejemplo, son posibles mejoras de rendimiento de x2 al desplazarse a través de un [ `ListView` ](xref:Xamarin.Forms.ListView) que contienen miles de filas de datos, donde se realizan las celdas de cada fila de controles que usar representadores rápidos, que da como resultado visiblemente desplazamiento más suave.


## <a name="related-links"></a>Vínculos relacionados

- [Representadores personalizados](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
