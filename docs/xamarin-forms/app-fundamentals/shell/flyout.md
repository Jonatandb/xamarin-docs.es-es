---
title: Control flotante de Xamarin.Forms Shell
description: El control flotante es el menú raíz de una aplicación de Shell y es accesible por medio de un icono o al deslizar el dedo desde el lado de la pantalla. El control flotante consta de un encabezado opcional, elementos de control flotante y elementos de menú opcionales.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: a64e96e1ee3804cd7aefd9834486613ba8d09d5f
ms.sourcegitcommit: 0596004d4a0e599c1da1ddd75a6ac928f21191c2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66005226"
---
# <a name="xamarinforms-shell-flyout"></a>Control flotante de Xamarin.Forms Shell

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)

El control flotante es el menú raíz de una aplicación de Shell y es accesible por medio de un icono o al deslizar el dedo desde el lado de la pantalla. El control flotante consta de un encabezado opcional, elementos de control flotante y elementos de menú opcionales:

![Captura de pantalla de un control flotante anotado de Shell](flyout-images/flyout-annotated.png "Control flotante anotado")

Si es necesario, el color de fondo del control flotante se puede establecer [`Color`](xref:Xamarin.Forms.Color) mediante la propiedad enlazable `Shell.FlyoutBackgroundColor`. Esta propiedad también se puede establecer con una hoja de estilo CSS. Para más información, consulte [Propiedades específicas de Xamarin.Forms Shell](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="flyout-icon"></a>Icono de control flotante

De forma predeterminada, las aplicaciones de Shell tienen un icono de tres barras que, cuando se presiona, abre el control flotante. Este icono se puede cambiar si se establece la propiedad enlazable `Shell.FlyoutIcon`, de tipo [`ImageSource`](xref:Xamarin.Forms.ImageSource), en un icono adecuado:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>Comportamiento del control flotante

Al control flotante se tiene acceso mediante el icono de tres barras o al deslizar el dedo desde el lado de la pantalla. Sin embargo, este comportamiento se puede cambiar si se establece la propiedad adjunta `Shell.FlyoutBehavior` en uno de los miembros de la enumeración `FlyoutBehavior`:

- `Disabled`: indica que el usuario no puede abrir el control flotante.
- `Flyout`: indica que el usuario puede abrir y cerrar el control flotante. Este es el valor predeterminado de la propiedad `FlyoutBehavior`.
- `Locked`: indica que el usuario no puede cerrar el control flotante, y que este no solapa el contenido.

En el siguiente ejemplo se muestra cómo deshabilitar el control flotante.

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> La propiedad adjunta `FlyoutBehavior` se puede establecer en los objetos `Shell`, `FlyoutItem`, `ShellContent` y en objetos de página, para invalidar el comportamiento predeterminado del control flotante.

Además, el control flotante se puede abrir y cerrar mediante programación si se establece la propiedad enlazable `Shell.FlyoutIsPresented` en un valor `boolean` que indica si el control flotante es visible actualmente:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>Encabezado de control flotante

El encabezado de control flotante es el contenido que aparece opcionalmente en la parte superior del control flotante y su apariencia se define mediante un elemento `object` que se puede establecer mediante el valor de la propiedad `Shell.FlyoutHeader`:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

El tipo `FlyoutHeader` se muestra en el ejemplo siguiente:

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

El resultado es el siguiente encabezado de control flotante:

![Captura de pantalla del encabezado de control flotante](flyout-images/flyout-header.png "Encabezado de control flotante")

Como alternativa, la apariencia del encabezado de control flotante se puede definir mediante el establecimiento de la propiedad `Shell.FlyoutHeaderTemplate` en un objeto [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

De forma predeterminada, el encabezado de control flotante se corregirá en el control flotante mientras el contenido que va a continuación se desplazará si hay suficientes elementos. Sin embargo, este comportamiento se puede cambiar si se establece la propiedad enlazable `Shell.FlyoutHeaderBehavior` en uno de los miembros de la enumeración `FlyoutHeaderBehavior`:

- `Default`: indica que se usará el comportamiento predeterminado para la plataforma. Este es el valor predeterminado de la propiedad `FlyoutHeaderBehavior`.
- `Fixed`: indica que el encabezado de control flotante permanece sin cambios y visible en todo momento.
- `Scroll`: indica que el encabezado de control flotante se desplaza fuera de la vista cuando el usuario desplaza los elementos.
- `CollapseOnScroll`: indica que el encabezado de control flotante se contrae solo en un título, a medida que el usuario desplaza los elementos.

En el ejemplo siguiente se muestra cómo contraer el encabezado de control flotante a medida que el usuario se desplaza:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-items"></a>Elementos del control flotante

Cada objeto `Shell` en subclase debe contener uno o varios objetos `FlyoutItem`, donde cada objeto `FlyoutItem` representa un elemento del control flotante. En el ejemplo siguiente se crea un control flotante que contiene un encabezado y dos elementos:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

En este ejemplo, solo se puede acceder a cada objeto [`ContentPage`](xref:Xamarin.Forms.ContentPage) mediante elementos de control flotante:

[![Captura de pantalla de una aplicación de Shell de dos páginas con elementos de control flotante en iOS y Android](flyout-images/two-page-app-flyout.png "Aplicación de Shell de dos páginas con elementos de control flotante")](flyout-images/two-page-app-flyout-large.png#lightbox "Shell two page app with flyout items")

> [!NOTE]
> Cuando no existe un encabezado de control flotante, aparecen elementos de control flotante en la parte superior del control flotante. En caso contrario, aparecen debajo del encabezado de control flotante.

Shell tiene operadores de conversión implícita que permiten simplificar la jerarquía visual de Shell, sin introducir vistas adicionales en el árbol visual. Esto es posible porque un objeto `Shell` en subclase solo puede contener objetos `FlyoutItem`, que solo pueden contener objetos `Tab`, que solo pueden contener objetos `ShellContent`. Estos operadores de conversión implícita pueden usarse para quitar los objetos `FlyoutItem`, `Tab` y `ShellContent` del ejemplo anterior:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage IconImageSource="cat.png" />
    <views:DogsPage IconImageSource="dog.png" />
</Shell>
```

Esta conversión implícita encapsula automáticamente cada objeto [`ContentPage`](xref:Xamarin.Forms.ContentPage) en objetos `ShellContent`, que se encapsulan en objetos `Tab`, que se encapsulan en objetos `FlyoutItem`.

> [!IMPORTANT]
> En una aplicación de Shell, cada [ `ContentPage`](xref:Xamarin.Forms.ContentPage) que es un elemento secundario de un objeto `ShellContent` se crea durante el inicio de la aplicación. Agregar objetos `ShellContent` adicionales mediante esta estrategia dará lugar a la creación de páginas adicionales durante el inicio de la aplicación, lo que puede conducir a una experiencia de inicio deficiente. Sin embargo, Shell también es capaz de crear páginas a petición, en respuesta a la navegación. Para más información, consulte [Carga eficiente de páginas](tabs.md#efficient-page-loading) en la guía [Pestañas de Xamarin.Forms Shell](tabs.md).

### <a name="flyoutitem-class"></a>Clase FlyoutItem

La clase `FlyoutItem` incluye las siguientes propiedades que controlan su apariencia y comportamiento:

- `FlyoutDisplayOptions`, de tipo `FlyoutDisplayOptions`, define cómo se muestran el elemento y sus elementos secundarios en el control flotante. El valor predeterminado es `AsSingleItem`.
- `CurrentItem`, de tipo `Tab`, el elemento seleccionado.
- `Items`, de tipo `ShellSectionCollection`, define todas las pestañas dentro de `FlyoutItem`.
- `FlyoutIcon`, de tipo `ImageSource`, el icono que se usará para el elemento. Si esta propiedad no está establecida, se volverá a usar el valor de la propiedad `Icon`.
- `Icon`, de tipo `ImageSource`, define el icono que se mostrará en las partes del cromo que no son el control flotante.
- `IsChecked`, de tipo `boolean`, define si el elemento está actualmente resaltado en la ventana flotante.
- `IsEnabled`, de tipo `boolean`, define si el elemento es seleccionable en el cromo.
- `IsTabStop`, de tipo `bool`, indica si se incluye un objeto `FlyoutItem` en la navegación entre pestañas. Su valor predeterminado es `true`, y cuando su valor es `false`, la infraestructura de navegación entre pestañas omite el objeto `FlyoutItem`, independientemente de si se ha definido un objeto `TabIndex`.
- `TabIndex`, de tipo `int`, indica el orden en que los objetos `FlyoutItem` reciben el foco cuando el usuario navega por los elementos presionando la tecla de tabulación. El valor predeterminado de la propiedad es 0.
- `Title`, de tipo `string`, el título que se mostrará en la interfaz de usuario.
- `Route`, de tipo `string`, la cadena usada para abordar el elemento.

Todas estas propiedades, excepto la propiedad `Route`, están respaldadas por objetos [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), lo que significa que las propiedades pueden ser destinos de los enlaces de datos.

> [!NOTE]
> Todos los objetos `FlyoutItem` del objeto Shell en subclase se agregan a la colección `Shell.Items`, que define la lista de elementos que se mostrarán en el control flotante.

Además, la clase `FlyoutItem` expone los siguientes métodos reemplazables:

- `OnTabIndexPropertyChanged`, que se llama cada vez que cambia la propiedad `TabIndex`.
- `OnTabStopPropertyChanged`, que se llama cada vez que cambia la propiedad `IsTabStop`.
- `TabIndexDefaultValueCreator`, devuelve un elemento `int`, y se llama para establecer el valor predeterminado de la propiedad `TabIndex`.
- `TabStopDefaultValueCreator`, devuelve un elemento `bool`, y se llama para establecer el valor predeterminado de la propiedad `TabStop`.

## <a name="flyout-display-options"></a>Opciones de presentación de control flotante

La enumeración `FlyoutDisplayOptions` define los miembros siguientes:

- `AsSingleItem`, indica que el elemento será visible como un único elemento.
- `AsMultipleItems`, indica que el elemento y sus elementos secundarios serán visibles en el control flotante como un grupo de elementos.

Al establecer la propiedad `FlyoutItem.FlyoutDisplayOptions` en `AsMultipleItems`, se creará un elemento de control flotante para cada objeto `Tab` dentro de un objeto `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

En este ejemplo, se crean elementos de control flotante para el objeto `Tab`, que es secundario del objeto `FlyoutItem`, y el objeto `Shellontent`, que son elementos secundarios del objeto `FlyoutItem`. Esto ocurre porque cada objeto `ShellContent` que es secundario del objeto `FlyoutItem` se encapsula automáticamente en un objeto `Tab`. Además, se crea un elemento de control flotante para el objeto `ShellContent` final, que se encapsula en un objeto `Tab` y, luego, en un objeto `FlyoutItem`.

El resultado son los siguientes elementos de control flotante:

[![Captura de pantalla de control flotante que contiene objetos FlyoutItem en iOS y Android](flyout-images/flyout-reduced.png "Control flotante de Shell que contiene objetos FlyoutItem")](flyout-images/flyout-reduced-large.png#lightbox "Shell flyout containing FlyoutItem objects")

## <a name="define-flyoutitem-appearance"></a>Definición de la apariencia de FlyoutItem

Se puede personalizar la apariencia de cada `FlyoutItem` mediante el establecimiento de la propiedad `Shell.ItemTemplate` en un objeto [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

En este ejemplo se muestra el título de cada objeto `FlyoutItem` en cursiva:

[![Captura de pantalla de objetos FlyoutItem con plantilla en iOS y Android](flyout-images/flyoutitem-templated.png "Objetos FlyoutItem con plantilla de Shell")](flyout-images/flyoutitem-templated-large.png#lightbox "Shell templated FlyoutItem objects")

> [!NOTE]
> Shell proporciona las propiedades `Title` y `Icon` para el objeto [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) de `ItemTemplate`.

## <a name="flyoutitem-tab-order"></a>Orden de tabulación de FlyoutItem

De forma predeterminada, el orden de tabulación de los objetos `FlyoutItem` es el mismo orden en que se enumeran en XAML o se agregan mediante programación a una colección secundaria. Este es el orden en que se navegará por los objetos `FlyoutItem` con un teclado y, con frecuencia, este orden predeterminado es el mejor orden posible.

Se puede cambiar el orden de tabulación predeterminado mediante el establecimiento de la propiedad `FlyoutItem.TabIndex`, que indica el orden en que los objetos `FlyoutItem` reciben el foco cuando el usuario navega por los elementos presionando la tecla de tabulación. El valor predeterminado de la propiedad es 0, y se puede establecer en cualquier valor `int`.

Las reglas siguientes aplican cuando se usa el orden de tabulación predeterminado, o cuando se establece la propiedad `TabIndex`:

- Los objetos `FlyoutItem` con un objeto `TabIndex` igual a 0 se agregan al orden de tabulación según su orden de declaración en XAML o colecciones secundarias.
- Los objetos `FlyoutItem` con un valor de `TabIndex` mayor que 0 se agregan al orden de tabulación en función de su valor `TabIndex`.
- Los objetos `FlyoutItem` con un valor de `TabIndex` menor que 0 se agregan al orden de tabulación y aparecen antes que cualquier valor 0.
- Los conflictos relacionados con `TabIndex` se resuelven por orden de declaración.

Tras definirse un orden de tabulación, al presionar la tecla TAB se recorrerá cíclicamente el foco a través de objetos `FlyoutItem` en orden de `TabIndex` ascendente, con una encapsulación alrededor del principio una vez alcanzado el control final.

Además de establecer el orden de tabulación de los objetos `FlyoutItem`, puede ser necesario excluir algunos objetos de dicho orden. Para ello, se puede usar la propiedad `FlyoutItem.IsTabStop`, que indica si un objeto `FlyoutItem` se incluye en la navegación entre pestañas. Su valor predeterminado es `true`, y cuando su valor es `false`, la infraestructura de navegación entre pestañas omite el objeto `FlyoutItem`, independientemente de si se ha definido un objeto `TabIndex`.

## <a name="set-the-current-flyoutitem"></a>Establecimiento del objeto FlyoutItem actual

La clase `Shell` tiene una propiedad enlazable llamada `CurrentItem`, de tipo `FlyoutItem`, que representa actualmente el objeto `FlyoutItem` seleccionado. La primera vez que se ejecuta una aplicación de Shell, esta propiedad se establecerá en el primer objeto `FlyoutItem` del objeto `Shell` en subclase. Sin embargo, la propiedad se puede establecer en otro objeto `FlyoutItem`, como se muestra en el ejemplo siguiente:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

Este código establece el objeto `ShellContent` llamado `aboutItem` como la propiedad `CurrentItem`, que hace que se muestre. En este ejemplo, se usa una conversión implícita para encapsular el objeto `ShellContent` en un objeto `Tab`, que se encapsula en un objeto `FlyoutItem`.

El código de C# equivalente es el siguiente:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>Elementos de menú

Opcionalmente, los elementos de menú aparecen en el control flotante, debajo de los elementos de control flotante. Cada elemento de menú se representa mediante un objeto [`MenuItem`](xref:Xamarin.Forms.MenuItem).

> [!NOTE]
> La clase `MenuItem` tiene un evento [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) y una propiedad [`Command`](xref:Xamarin.Forms.MenuItem.Command). Por lo tanto, los objetos `MenuItem` permiten escenarios que ejecutan una acción en respuesta al elemento `MenuItem` que se pulsa. Estos escenarios incluyen realizar la navegación y abrir un explorador web en una página web específica.

Se pueden agregar objetos [`MenuItem`](xref:Xamarin.Forms.MenuItem) al control flotante, tal como se muestra en el ejemplo siguiente:

```xaml
<Shell ...>
    ...            
    <MenuItem Text="Random"
              IconImageSource="random.png"
              Command="{Binding RandomPageCommand}" />
    <MenuItem Text="Help"
              IconImageSource="help.png"
              Command="{Binding HelpCommand}"
              CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />    
</Shell>
```

Este código agrega dos objetos [`MenuItem`](xref:Xamarin.Forms.MenuItem) al control flotante:

[![Captura de pantalla de ventana flotante que contiene objetos MenuItem en iOS y Android](flyout-images/flyout.png "Control flotante de Shell que contiene objetos MenuItem")](flyout-images/flyout-large.png#lightbox "Shell flyout containing MenuItem objects")

El primer objeto [`MenuItem`](xref:Xamarin.Forms.MenuItem) ejecuta un elemento `ICommand` llamado `RandomPageCommand`, que lleva a una página aleatoria de la aplicación. El segundo objeto `MenuItem` ejecuta un elemento `ICommand` llamado `HelpCommand`, que abre la dirección URL especificada por la propiedad `CommandParameter` en un explorador web.

> [!NOTE]
> El elemento [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) de cada `MenuItem` se hereda del objeto `Shell` en subclase.

## <a name="define-menuitem-appearance"></a>Definición de la apariencia de MenuItem

Se puede personalizar la apariencia de cada `MenuItem` mediante el establecimiento de la propiedad `Shell.MenuItemTemplate` en un objeto [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.2*" />
                <ColumnDefinition Width="0.8*" />
            </Grid.ColumnDefinitions>
            <Image Source="{Binding Icon}"
                   Margin="5"
                   HeightRequest="45" />
            <Label Grid.Column="1"
                   Text="{Binding Text}"
                   FontAttributes="Italic"
                   VerticalTextAlignment="Center" />
        </Grid>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

En este ejemplo se muestra el título de cada objeto `MenuItem` en cursiva:

[![Captura de pantalla de los objetos MenuItem con plantilla en iOS y Android](flyout-images/menuitem-templated.png "Objetos MenuItem con plantilla de Shell")](flyout-images/menuitem-templated-large.png#lightbox "Shell templated MenuItem objects")

> [!NOTE]
> Shell proporciona las propiedades [`Text`](xref:Xamarin.Forms.MenuItem.Text) y [`IconImageSource`](xref:Xamarin.Forms.MenuItem.IconImageSource) para el elemento [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) del objeto `MenuItemTemplate`.`

## <a name="related-links"></a>Vínculos relacionados

- [Xaminals (ejemplo)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/Xaminals/)
