---
title: Personalizar la apariencia de ListView
description: En este artículo se explica cómo personalizar la ListView en las aplicaciones de Xamarin.Forms mediante el uso de encabezados, pies de página, grupos y las celdas de alto variable.
ms.prod: xamarin
ms.assetid: DC8009B0-4371-4D60-885A-5362FC7EE3E5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/13/2018
ms.openlocfilehash: 270191890e8747ecad7d250fb0fe18ce253d3823
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61169992"
---
# <a name="customizing-listview-appearance"></a>Personalizar la apariencia de ListView

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)

[`ListView`](xref:Xamarin.Forms.ListView) tiene la capacidad para controlar la presentación de la lista, además del [ `ViewCell` ](xref:Xamarin.Forms.ViewCell) instancias para cada fila de la lista.

<a name="Grouping" />

## <a name="grouping"></a>Agrupar
A menudo, grandes conjuntos de datos pueden ser difícil de manejar cuando se presentan en una lista desplazable continuamente. Habilitar agrupación puede mejorar la experiencia del usuario en estos casos organizar mejor el contenido y activando controles específicos de la plataforma que facilitan la navegación de datos.

Cuando se activa la agrupación por una `ListView`, se agrega una fila de encabezado para cada grupo.

Para habilitar la agrupación:

- Crear una lista de listas (una lista de grupos, cada grupo que se va a obtener una lista de elementos).
- Establecer el `ListView`del `ItemsSource` a dicha lista.
- Establecer `IsGroupingEnabled` en true.
- Establecer [ `GroupDisplayBinding` ](xref:Xamarin.Forms.ListView.GroupDisplayBinding) para enlazar a la propiedad de los grupos que se usa como el título del grupo.
- [Opcional] Establecer [ `GroupShortNameBinding` ](xref:Xamarin.Forms.ListView.GroupShortNameBinding) para enlazar a la propiedad de los grupos que se usa como el nombre corto para el grupo. El nombre corto se utiliza para las listas de salto (columna derecha en iOS).

Empiece por crear una clase para los grupos:

```csharp
public class PageTypeGroup : List<PageModel>
    {
        public string Title { get; set; }
        public string ShortName { get; set; } //will be used for jump lists
        public string Subtitle { get; set; }
        private PageTypeGroup(string title, string shortName)
        {
            Title = title;
            ShortName = shortName;
        }

        public static IList<PageTypeGroup> All { private set; get; }
    }
```

En el código anterior, `All` es la lista que se asignará a nuestra ListView como origen de enlace. `Title` y `ShortName` son las propiedades que se usará para los encabezados de grupo.

En esta fase, `All` es una lista vacía. Agregue un constructor estático para que se rellenará la lista en el inicio del programa:

```csharp
static PageTypeGroup()
{
    List<PageTypeGroup> Groups = new List<PageTypeGroup> {
            new PageTypeGroup ("Alfa", "A"){
                new PageModel("Amelia", "Cedar", new switchCellPage(),""),
                new PageModel("Alfie", "Spruce", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ava", "Pine", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Archie", "Maple", new switchCellPage(), "grapefruit.jpg")
            },
            new PageTypeGroup ("Bravo", "B"){
                new PageModel("Brooke", "Lumia", new switchCellPage(),""),
                new PageModel("Bobby", "Xperia", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Bella", "Desire", new switchCellPage(), "grapefruit.jpg"),
                new PageModel("Ben", "Chocolate", new switchCellPage(), "grapefruit.jpg")
            }
        }
        All = Groups; //set the publicly accessible list
}
```

En el código anterior, también lo llamemos `Add` en elementos de `groups`, que son instancias del tipo `PageTypeGroup`. Esto es posible porque `PageTypeGroup` hereda de `List<PageModel>`. Este es un ejemplo de la lista de patrón de listas que se ha indicado anteriormente.

Este es el XAML para mostrar la lista agrupada:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage"
    <ContentPage.Content>
        <ListView  x:Name="GroupedView"
        GroupDisplayBinding="{Binding Title}"
        GroupShortNameBinding="{Binding ShortName}"
        IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                     Detail="{Binding Subtitle}" />
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

Esto da como resultado lo siguiente:

![](customizing-list-appearance-images/grouping-depth.png "Ejemplo de agrupación de ListView")

Tenga en cuenta que tenemos:

- Establecer `GroupShortNameBinding` a la `ShortName` propiedad definido en nuestra clase de grupo
- Establecer `GroupDisplayBinding` a la `Title` propiedad definido en nuestra clase de grupo
- Establecer `IsGroupingEnabled` en true
- Puede cambiar el `ListView`del `ItemsSource` a la lista agrupada

### <a name="customizing-grouping"></a>Personalizar la agrupación

Si se ha habilitado la agrupación en la lista, también se puede personalizar el encabezado de grupo.

Similar a cómo el `ListView` tiene un `ItemTemplate` para definir cómo se muestran las filas, `ListView` tiene un `GroupHeaderTemplate`.

Aquí se muestra un ejemplo de personalizar el encabezado de grupo en XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="DemoListView.GroupingViewPage">
    <ContentPage.Content>
        <ListView x:Name="GroupedView"
         GroupDisplayBinding="{Binding Title}"
         GroupShortNameBinding="{Binding ShortName}"
         IsGroupingEnabled="true">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding Subtitle}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.ItemTemplate>
            <!-- Group Header Customization-->
            <ListView.GroupHeaderTemplate>
                <DataTemplate>
                    <TextCell Text="{Binding Title}"
                    Detail="{Binding ShortName}"
                    TextColor="#f35e20"
                    DetailColor="#503026" />
                </DataTemplate>
            </ListView.GroupHeaderTemplate>
            <!-- End Group Header Customization -->
        </ListView>
    </ContentPage.Content>
</ContentPage>
```

<a name="Headers_and_Footers" />

## <a name="headers-and-footers"></a>Encabezados y pies de página
Es posible que un ListView presentar un encabezado y pie de página que se desplazan con los elementos de la lista. El encabezado y pie de página pueden ser cadenas de texto o un diseño más complicado. Tenga en cuenta que esto es independiente de [grupos de sección](#Grouping).

Puede establecer el `Header` o `Footer` en una cadena simple valor, o bien puede configurarlos en un diseño más complejo.
También hay `HeaderTemplate` y `FooterTemplate` propiedades que le permiten creación diseños más complejos para el encabezado y pie de página que admitan el enlace de datos.

Para crear un encabezado o pie de página simple, simplemente establezca las propiedades de encabezado o pie de página en el texto que desea mostrar. Mediante código:

```csharp
ListView HeaderList = new ListView() {
    Header = "Header",
    Footer = "Footer"
    };
```

En XAML:

```xaml
<ListView  x:Name="HeaderList"  Header="Header" Footer="Footer"></ListView>
```

![](customizing-list-appearance-images/header-default.png "ListView con encabezado y pie de página")

Para crear un encabezado personalizado y un pie de página, definir las vistas de encabezado y pie de página:

```xaml
<ListView.Header>
    <StackLayout Orientation="Horizontal">
        <Label Text="Header"
        TextColor="Olive"
        BackgroundColor="Red" />
    </StackLayout>
</ListView.Header>
<ListView.Footer>
    <StackLayout Orientation="Horizontal">
        <Label Text="Footer"
        TextColor="Gray"
        BackgroundColor="Blue" />
    </StackLayout>
</ListView.Footer>
```

![](customizing-list-appearance-images/header-custom.png "ListView con encabezado personalizado y el pie de página")

## <a name="scrollbar-visibility"></a>Visibilidad de la barra de desplazamiento

[`ListView`](xref:Xamarin.Forms.ListView) tiene `HorizontalScrollBarVisibility` y `VerticalScrollBarVisibility` propiedades, que obtención o establece un [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) valor que representa el momento de la barra de desplazamiento horizontal o vertical, está visible. Ambas propiedades se pueden establecer en los siguientes valores:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) indica el comportamiento de barra de desplazamiento predeterminado para la plataforma, y es el valor predeterminado para el `HorizontalScrollBarVisibility` y `VerticalScrollBarVisibility` propiedades.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) indica que las barras de desplazamiento serán visibles, incluso cuando el contenido se ajusta en la vista.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) indica que las barras de desplazamiento no serán visibles, incluso si el contenido no se ajusta en la vista.

<a name="Row_Separators" />

## <a name="row-separators"></a>Separadores de fila
Se muestran las líneas de separación entre `ListView` elementos de forma predeterminada en iOS y Android. Si prefiere ocultar las líneas de separación en iOS y Android, establezca el `SeparatorVisibility` propiedad en el ListView. Las opciones para `SeparatorVisibility` son:

* **Default** -muestra una línea de separación en iOS y Android.
* **Ninguno** -oculta el separador en todas las plataformas.

Visibilidad predeterminada:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.Default;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="Default" />
```

![](customizing-list-appearance-images/separator-default.png "ListView con separadores de fila predeterminado")

Ninguno:

C#:

```csharp
SepratorDemoListView.SeparatorVisibility = SeparatorVisibility.None;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorVisibility="None" />
```

![](customizing-list-appearance-images/separator-none.png "ListView sin separadores de fila")

También puede establecer el color de la línea de separación a través de la `SeparatorColor` propiedad:

C#:

```csharp
SepratorDemoListView.SeparatorColor = Color.Green;
```

XAML:

```xaml
<ListView x:Name="SeparatorDemoListView" SeparatorColor="Green" />
```

![](customizing-list-appearance-images/separator-custom.png "ListView con separadores de fila verde")

> [!NOTE]
> Configuración de cualquiera de estas propiedades en Android después de cargar el `ListView` conlleva una reducción del rendimiento.

<a name="Row_Heights" />

## <a name="row-heights"></a>Alto de fila
Todas las filas en una ListView tienen el mismo alto de forma predeterminada. ListView tiene dos propiedades que pueden usarse para cambiar este comportamiento:

- `HasUnevenRows` &ndash; `true`/`false` valor, las filas tienen diferentes alturas si establece en `true`. Tiene como valor predeterminado `false`.
- `RowHeight` &ndash; establece el alto de cada fila cuando `HasUnevenRows` es `false`.

Puede establecer el alto de todas las filas estableciendo el `RowHeight` propiedad en el `ListView`.

### <a name="custom-fixed-row-height"></a>Alto de fila fijo personalizado

C#:

```csharp
RowHeightDemoListView.RowHeight = 100;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" RowHeight="100" />
```

![](customizing-list-appearance-images/height-custom.png "ListView con alto de fila fijo")


### <a name="uneven-rows"></a>Filas desiguales

Si desea que las filas individuales tengan distintas alturas, puede establecer el `HasUnevenRows` propiedad `true`.
Tenga en cuenta que el alto de las filas no tiene que configurar manualmente una vez `HasUnevenRows` se ha establecido en `true`, ya que el alto se calculará automáticamente mediante Xamarin.Forms.


C#:

```csharp
RowHeightDemoListView.HasUnevenRows = true;
```

XAML:

```xaml
<ListView x:Name="RowHeightDemoListView" HasUnevenRows="true" />
```

![](customizing-list-appearance-images/height-uneven.png "ListView con filas desiguales")

### <a name="runtime-resizing-of-rows"></a>El cambio de tamaño en tiempo de ejecución de filas

Individuales `ListView` filas pueden cambiarse mediante programación en tiempo de ejecución, siempre que el `HasUnevenRows` propiedad está establecida en `true`. El [ `Cell.ForceUpdateSize` ](xref:Xamarin.Forms.Cell.ForceUpdateSize) método actualiza el tamaño de una celda, incluso cuando no está actualmente visible, como se muestra en el ejemplo de código siguiente:

```csharp
void OnImageTapped (object sender, EventArgs args)
{
    var image = sender as Image;
    var viewCell = image.Parent.Parent as ViewCell;

    if (image.HeightRequest < 250) {
        image.HeightRequest = image.Height + 100;
        viewCell.ForceUpdateSize ();
    }
}
```

El `OnImageTapped` controlador de eventos se ejecuta en respuesta a una [ `Image` ](xref:Xamarin.Forms.Image) en una celda que se pulsa y aumenta el tamaño de la `Image` mostrada en la celda para que lo esté viendo fácilmente.

![](customizing-list-appearance-images/dynamic-row-resizing.png "ListView con cambio de tamaño de fila en tiempo de ejecución")

Tenga en cuenta que hay grandes posibilidades de degradación del rendimiento si esta característica está sobreutilizada.



## <a name="related-links"></a>Vínculos relacionados

- [GROUPING (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/Grouping)
- [Vista de representador personalizado (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithListviewNative/)
- [Cambiar el tamaño de las filas dinámicas (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/DynamicUnevenListCells/)
- [notas de la versión 1.4](http://forums.xamarin.com/discussion/35451/xamarin-forms-1-4-0-released/)
- [notas de la versión 1.3](http://forums.xamarin.com/discussion/29934/xamarin-forms-1-3-0-released/)
