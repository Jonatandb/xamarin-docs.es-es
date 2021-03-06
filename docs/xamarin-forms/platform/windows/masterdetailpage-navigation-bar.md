---
title: Barra de navegación MasterDetailPage en Windows
description: Las características específicas de la plataforma permiten consumir funcionalidad que solo está disponible en una plataforma específica, sin necesidad de implementar representadores o efectos personalizados. En este artículo se explica cómo consumir la plataforma específica de Windows que contrae la barra de navegación en un MasterDetailPage.
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 876e5449e17276efc3b368ab7d2c9e57365f948f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937831"
---
# <a name="masterdetailpage-navigation-bar-on-windows"></a>Barra de navegación MasterDetailPage en Windows

[![Descargar el ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Esta Plataforma universal de Windows específica de la plataforma se usa para contraer la barra de navegación en un [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) y se consume en XAML estableciendo las [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) propiedades adjuntas y:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Como alternativa, se puede usar desde C# con la API fluida:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

El `MasterDetailPage.On<Windows>` método especifica que este específico de la plataforma solo se ejecutará en Windows. [ `Page.SetCollapseStyle` ] (XREF: Xamarin.Forms . PlatformConfiguration. WindowsSpecific. MasterDetailPage. SetCollapseStyle ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . MasterDetailPage}, Xamarin.Forms . PlatformConfiguration. WindowsSpecific. CollapseStyle)), en el [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) espacio de nombres, se usa para especificar el estilo de contraer, con la [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) enumeración que proporciona dos valores: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) y [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) . [ `MasterDetailPage.CollapsedPaneWidth` ] (XREF: Xamarin.Forms . PlatformConfiguration. WindowsSpecific. MasterDetailPage. CollapsedPaneWidth ( Xamarin.Forms . IPlatformElementConfiguration { Xamarin.Forms . PlatformConfiguration. Windows, Xamarin.Forms . El método MasterDetailPage}, System. Double)) se usa para especificar el ancho de una barra de navegación contraída parcialmente.

El resultado es que [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) se aplica un especificado a la [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) instancia, pero también se especifica el ancho:

[![Barra de navegación contraída específica de la plataforma](masterdetailpage-navigation-bar-images/collapsed-navigation-bar.png)](masterdetailpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "Barra de navegación contraída específica de la plataforma")

## <a name="related-links"></a>Vínculos relacionados

- [PlatformSpecifics (ejemplo)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Creación funcionalidades específicas de plataforma](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API de WindowsSpecific](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
