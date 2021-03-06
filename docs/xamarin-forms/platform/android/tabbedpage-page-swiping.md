---
title: Página deslizante de TabbedPage en Android
description: Las características específicas de la plataforma permiten consumir funcionalidad que solo está disponible en una plataforma específica, sin necesidad de implementar representadores o efectos personalizados. En este artículo se explica cómo consumir el específico de la plataforma Android que permite deslizar rápidamente con un gesto de dedo horizontal entre las páginas de un TabbedPage.
ms.prod: xamarin
ms.assetid: D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 0043d90e631c19a55b766a877a9cd30316f14650
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997389"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Página deslizante de TabbedPage en Android

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Esta plataforma Android específica se usa para habilitar el deslizamiento con un dedo horizontal entre las páginas de un [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Se consume en XAML estableciendo la [`TabbedPage.IsSwipePagingEnabled`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) propiedad adjunta en un `boolean` valor:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Como alternativa, se puede usar desde C# con la API fluida:

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

El `TabbedPage.On<Android>` método especifica que este específico de la plataforma solo se ejecutará en Android. [ `TabbedPage.SetIsSwipePagingEnabled` ] (XREF: :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. AndroidSpecific. TabbedPage. SetIsSwipePagingEnabled ( :::no-loc(Xamarin.Forms)::: . BindableObject, System. Boolean)), en el [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) espacio de nombres, se usa para habilitar el deslizamiento entre las páginas de un [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Además, la `TabbedPage` clase del espacio de `:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific` nombres también tiene [ `EnableSwipePaging` ] (XREF: :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. AndroidSpecific. TabbedPage. EnableSwipePaging ( :::no-loc(Xamarin.Forms)::: . IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. Android, :::no-loc(Xamarin.Forms)::: . TabbedPage})) que habilita este método específico de la plataforma y un [ `DisableSwipePaging` ] (XREF: :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. AndroidSpecific. TabbedPage. DisableSwipePaging ( :::no-loc(Xamarin.Forms)::: . IPlatformElementConfiguration { :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. Android, :::no-loc(Xamarin.Forms)::: . TabbedPage})) que deshabilita este específico de la plataforma. La [`TabbedPage.OffscreenPageLimit`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) propiedad adjunta y [ `SetOffscreenPageLimit` ] (XREF: :::no-loc(Xamarin.Forms)::: . PlatformConfiguration. AndroidSpecific. TabbedPage. SetOffscreenPageLimit ( :::no-loc(Xamarin.Forms)::: . BindableObject, System. Int32)), se usan para establecer el número de páginas que deben conservarse en un estado de inactividad en cualquier lado de la página actual.

El resultado es que está habilitada la paginación de deslizamiento a través de las páginas que muestra un [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) :

![Deslizar rápidamente la paginación a través de un TabbedPage](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>Vínculos relacionados

- [PlatformSpecifics (ejemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creación funcionalidades específicas de plataforma](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API de AndroidSpecific](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific. AppCompat API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
