---
title: Vistas de Xamarin.Forms
description: Las vistas de Xamarin.Forms son los bloques de creación de interfaces de usuario móviles multiplataforma. En este artículo se enumera las vistas que se incluyen en Xamarin.Forms.
ms.prod: xamarin
ms.assetid: AC070686-A423-4A98-8BB6-0B9F94C062CC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/21/2019
ms.openlocfilehash: 779b39441893324e38843e036614484443e37574
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971290"
---
# <a name="xamarinforms-views"></a>Vistas de Xamarin.Forms

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/FormsGallery/)

_Las vistas de Xamarin.Forms son los bloques de creación de interfaces de usuario móviles multiplataforma._

Las vistas son objetos de interfaz de usuario, como etiquetas, botones y los controles deslizantes que se conocen normalmente como *controles* o *widgets* en otros entornos de programación de gráficos. Las vistas compatibles con Xamarin.Forms todos se derivan los [ `View` ](xref:Xamarin.Forms.View) clase. Pueden dividirse en varias categorías:

## <a name="views-for-presentation"></a>Vistas de presentación

### <a name="label"></a>Etiqueta

|     |     |
| --- | --- |
| [`Label`](xref:Xamarin.Forms.Label) Muestra las cadenas de texto de una línea o bloques de varias líneas de texto, ya sea con formato constantes o variables. Establecer el [ `Text` ](xref:Xamarin.Forms.Label.Text) propiedad en una cadena de constante de formato, o conjunto el [ `FormattedText` ](xref:Xamarin.Forms.Label.FormattedText) propiedad a un [ `FormattedString` ](xref:Xamarin.Forms.FormattedString) objeto de variable el formato.<br /><br />[Documentación de API](xref:Xamarin.Forms.Label) / [guía](~/xamarin-forms/user-interface/text/label.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/) | [![Ejemplo de etiqueta](views-images/Label.png "etiquetar ejemplo")](views-images/Label-Large.png#lightbox "ejemplo de etiqueta")<br /> [Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/LabelDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/LabelDemoPage.xaml) |
|     |     |

### <a name="image"></a>Imagen

|     |     |
| --- | --- |
| [`Image`](xref:Xamarin.Forms.Image) muestra un mapa de bits. Los mapas de bits se pueden descargar a través de Web, incrustados como recursos en el proyecto común o proyectos de plataforma o creadas con .NET `Stream` objeto.<br /><br />[Documentación de API](xref:Xamarin.Forms.Image) / [guía](~/xamarin-forms/user-interface/images.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithImages/) | [![Ejemplo de la imagen](views-images/Image.png "la imagen de ejemplo")](views-images/Image-Large.png#lightbox "la imagen de ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageDemoPage.xaml) |
|     |     |

### <a name="boxview"></a>BoxView

|     |    |
| --- | ---|
| [`BoxView`](xref:Xamarin.Forms.BoxView) muestra un rectángulo relleno con color de la [ `Color` ](xref:Xamarin.Forms.BoxView.Color) propiedad. `BoxView` tiene una solicitud de tamaño predeterminado de 40 x 40. Para otros tamaños, asigne el [ `WidthRequest` ](xref:Xamarin.Forms.VisualElement.WidthRequest) y [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) propiedades.<br /><br />[Documentación de API](xref:Xamarin.Forms.BoxView) / [guía](~/xamarin-forms/user-interface/boxview.md) / [1 de ejemplo](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BasicBoxView), [2](https://developer.xamarin.com/samples/xamarin-forms/BoxView/TextDecoration), [3](https://developer.xamarin.com/samples/xamarin-forms/BoxView/ColorListBox), [4 ](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife), [5](https://developer.xamarin.com/samples/xamarin-forms/BoxView/DotMatrixClock), y [6](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock) | [![Ejemplo de BoxView](views-images/BoxView.png "BoxView ejemplo")](views-images/BoxView-Large.png#lightbox "BoxView ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/BoxViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/BoxViewDemoPage.xaml) |
|     |     |

### <a name="webview"></a>WebView

|     |     |
| --- | --- |
| [`WebView`](xref:Xamarin.Forms.WebView) Muestra las páginas Web o contenido HTML en función de si el [ `Source` ](xref:Xamarin.Forms.WebView.Source) propiedad está establecida en un [ `UriWebViewSource` ](xref:Xamarin.Forms.UrlWebViewSource) o un [ `HtmlWebViewSource` ](xref:Xamarin.Forms.HtmlWebViewSource) objeto.<br /><br />[Documentación de API](xref:Xamarin.Forms.WebView) / [guía](~/xamarin-forms/user-interface/webview.md) / [1 de ejemplo](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithWebview/) y [2](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/WebView) | [![Ejemplo de WebView](views-images/WebView.png "ejemplo WebView")](views-images/WebView-Large.png#lightbox "ejemplo WebView")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/WebViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/WebViewDemoPage.xaml) |
|     |     |

### <a name="openglview"></a>OpenGLView

|     |     |
| --- | --- |
| [`OpenGLView`](xref:Xamarin.Forms.OpenGLView) Muestra gráficos OpenGL en proyectos de iOS y Android. No hay ninguna compatibilidad para la plataforma Universal de Windows. Los proyectos de Android y iOS requieren una referencia a la **OpenTK 1.0** ensamblado o el **OpenTK** ensamblado de la versión 1.0.0.0. `OpenGLView` es más fácil de usar en un proyecto compartido. Si se usa en una biblioteca .NET Standard, un servicio de dependencia también será necesario (como se muestra en el código de ejemplo).<br /><br />Se trata de la instalación de solo gráficos que está integrada en Xamarin.Forms, pero también puede representar una aplicación de Xamarin.Forms utilizando gráficos [ `CocosSharp` ](~/xamarin-forms/user-interface/graphics/cocossharp.md), [ `SkiaSharp` ](~/xamarin-forms/user-interface/graphics/skiasharp/index.md), o [ `UrhoSharp` ](~/xamarin-forms/user-interface/graphics/urhosharp.md).<br /><br />[Documentación de la API](xref:Xamarin.Forms.OpenGLView)<br /><br /> | [![Ejemplo de OpenGLView](views-images/OpenGLView.png "OpenGLView ejemplo")](views-images/OpenGLView-Large.png#lightbox "OpenGLView ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/OpenGLViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/OpenGLViewDemoPage.xaml.cs) |
|     |     |

### <a name="map"></a>Asignación

|     |     |
| --- | --- |
| [`Map`](xref:Xamarin.Forms.Maps.Map) muestra un mapa. El **xamarin.Forms.Maps para** debe instalarse el paquete Nuget. Android y plataforma Universal de Windows requieren una clave de autorización de mapa.<br /><br />[Documentación de API](xref:Xamarin.Forms.Maps.Map) / [guía](~/xamarin-forms/user-interface/map.md) / [ejemplo](https://developer.xamarin.com/samples/WorkingWithMaps/) | [![Ejemplo de mapa](views-images/Map.png "asignar ejemplo")](views-images/Map-Large.png#lightbox "ejemplo de mapa")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/MapDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/MapDemoPage.xaml) |
|     |     |

## <a name="views-that-initiate-commands"></a>Vistas que inician comandos

### <a name="button"></a>Botón

|     |     |
| --- | --- |
| [`Button`](xref:Xamarin.Forms.Button) es un objeto rectangular que muestra el texto, y que se desencadena una [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) eventos cuando se ha presionado.<br /><br />[Documentación de API](xref:Xamarin.Forms.Button) / [guía](~/xamarin-forms/user-interface/button.md) / [ejemplo](https://developer.xamarin.com/samples/UserInterface/ButtonDemos/) | [![Ejemplo de botón](views-images/Button.png "botón ejemplo")](views-images/Button-Large.png#lightbox "ejemplo de botón")<br /> [Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ButtonDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ButtonDemoPage.xaml.cs) |
|     |     |

### <a name="imagebutton"></a>ImageButton

|     |     |
| --- | --- |
| `ImageButton` es un objeto un rectangular que muestra una imagen y que se activa un `Clicked` eventos cuando se ha presionado.<br /><br /> [Guía de](~/xamarin-forms/user-interface/imagebutton.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/FormsGallery/) | [![Ejemplo de ImageButton](views-images/ImageButton.png "ejemplo ImageButton")](views-images/ImageButton-Large.png#lightbox "ejemplo ImageButton")<br /> [Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ImageButtonDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ImageButtonDemoPage.xaml.cs) |
|     |     |

### <a name="searchbar"></a>SearchBar

|     |     |
| --- | --- |
| [`SearchBar`](xref:Xamarin.Forms.SearchBar) muestra un área para el usuario escriba una cadena de texto y un botón (o una tecla del teclado) que se indica a la aplicación para realizar una búsqueda. El [ `Text` ](xref:Xamarin.Forms.SearchBar.Text) propiedad proporciona acceso al texto y el [ `SearchButtonPressed` ](xref:Xamarin.Forms.SearchBar.SearchButtonPressed) evento indica que se ha presionado el botón.<br /><br />[Documentación de la API](xref:Xamarin.Forms.SearchBar) | [![Ejemplo de SearchBar](views-images/SearchBar.png "SearchBar ejemplo")](views-images/SearchBar-Large.png#lightbox "SearchBar ejemplo")<br /> [Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SearchBarDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SearchBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-for-setting-values"></a>Para establecer valores de las vistas

### <a name="slider"></a>Slider

|     |     |
| --- | --- |
| [`Slider`](xref:Xamarin.Forms.Slider) permite al usuario seleccionar un `double` valor desde un intervalo continuo especificado con el [ `Minimum` ](xref:Xamarin.Forms.Slider.Minimum) y [ `Maximum` ](xref:Xamarin.Forms.Slider.Maximum) propiedades.<br /><br />[Documentación de API](xref:Xamarin.Forms.Slider) / [guía](~/xamarin-forms/user-interface/slider.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/SliderDemos) | [![Ejemplo de control deslizante](views-images/Slider.png "ejemplo del control deslizante")](views-images/Slider-Large.png#lightbox "ejemplo del control deslizante")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SliderDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SliderDemoPage.xaml) |
|     |     |

### <a name="stepper"></a>Motor paso a paso

|     |     |
| --- | --- |
| [`Stepper`](xref:Xamarin.Forms.Stepper) permite al usuario seleccionar un `double` valor de un intervalo de valores incrementales especificado con el [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum), [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum), y [ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) propiedades.<br /><br />[Documentación de API](xref:Xamarin.Forms.Stepper)  / [guía](~/xamarin-forms/user-interface/stepper.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos) | [![Ejemplo de componente](views-images/Stepper.png "ejemplo motor paso a paso")](views-images/Stepper-Large.png#lightbox "ejemplo motor paso a paso")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/StepperDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/StepperDemoPage.xaml) |
|     |     |

### <a name="switch"></a>Modificador

|     |     |
| --- | --- |
| [`Switch`](xref:Xamarin.Forms.Switch) adopta la forma de un modificador on/off para permitir al usuario seleccionar un valor booleano. El [ `IsToggled` ](xref:Xamarin.Forms.Switch.IsToggled) propiedad es el estado del conmutador y el [ `Toggled` ](xref:Xamarin.Forms.Switch.Toggled) evento se desencadena cuando cambia el estado.<br /><br />[Documentación de la API](xref:Xamarin.Forms.Switch) | [![Cambiar el ejemplo](views-images/Switch.png "cambiar ejemplo")](views-images/Switch-Large.png#lightbox "cambiar ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/SwitchDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/SwitchDemoPage.xaml) |
|     |     |

### <a name="datepicker"></a>DatePicker

|     |     |
| --- | --- |
| [`DatePicker`](xref:Xamarin.Forms.DatePicker) permite al usuario seleccionar una fecha con el selector de fecha de la plataforma. Establecer un intervalo de fechas permitidos con el [ `MinimumDate` ](xref:Xamarin.Forms.DatePicker.MinimumDate) y [ `MaximumDate` ](xref:Xamarin.Forms.DatePicker.MaximumDate) propiedades. El [ `Date` ](xref:Xamarin.Forms.DatePicker.Date) propiedad es la fecha seleccionada y el [ `DateSelected` ](xref:Xamarin.Forms.DatePicker.DateSelected) evento se desencadena cuando se cambia dicha propiedad.<br /><br />[Documentación de API](xref:Xamarin.Forms.DatePicker) / [guía](~/xamarin-forms/user-interface/datepicker.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/DatePicker) | [![Ejemplo de DatePicker](views-images/DatePicker.png "ejemplo DatePicker")](views-images/DatePicker-Large.png#lightbox "ejemplo DatePicker")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/DatePickerDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/DatePickerDemoPage.xaml) |
|     |     |

### <a name="timepicker"></a>TimePicker

|     |     |
| --- | --- |
| [`TimePicker`](xref:Xamarin.Forms.TimePicker) permite al usuario seleccionar una hora con el selector de hora de la plataforma. El [ `Time` ](xref:Xamarin.Forms.TimePicker.Time) propiedad es la hora seleccionada. Una aplicación puede supervisar los cambios en el `Time` propiedad instalando un controlador para el [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged) eventos.<br /><br />[Documentación de API](xref:Xamarin.Forms.TimePicker) / [guía](~/xamarin-forms/user-interface/timepicker.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TimePicker) | [![Ejemplo de TimePicker](views-images/TimePicker.png "ejemplo TimePicker")](views-images/TimePicker-Large.png#lightbox "TimePicker ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TimePickerDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TimePickerDemoPage.xaml) |
|     |     |

## <a name="views-for-editing-text"></a>Vistas para editar texto

Estas dos clases se derivan de la [ `InputView` ](xref:Xamarin.Forms.InputView) (clase), que define el [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) propiedad.

<a name="entry" />

### <a name="entry"></a>Entrada

|     |     |
| --- | --- |
| [`Entry`](xref:Xamarin.Forms.Entry) permite al usuario escribir y editar una sola línea de texto. El texto está disponible como la [ `Text` ](xref:Xamarin.Forms.Entry.Text) propiedad y el [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged) y [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) los eventos se activan cuando los cambios de texto o el usuario señales de finalización al pulsar la tecla ENTRAR.<br /><br />Use un [ `Editor` ](#editor) para escribir y editar varias líneas de texto.<br /><br />[Documentación de API](xref:Xamarin.Forms.Entry) / [guía](~/xamarin-forms/user-interface/text/entry.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Ejemplo de entrada de](views-images/Entry.png "ejemplo de entrada de")](views-images/Entry-Large.png#lightbox "ejemplo de entrada")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EntryDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EntryDemoPage.xaml) |
|     |     |

<a name="editor" />

### <a name="editor"></a>Editor

|     |     |
| --- | --- |
| [`Editor`](xref:Xamarin.Forms.Editor) permite al usuario escribir y editar varias líneas de texto. El texto está disponible como la [ `Text` ](xref:Xamarin.Forms.Editor.Text) propiedad y el [ `TextChanged` ](xref:Xamarin.Forms.Editor.TextChanged) y [ `Completed` ](xref:Xamarin.Forms.Editor.Completed) los eventos se activan cuando los cambios de texto o el usuario indica la finalización.<br /><br />Use un [ `Entry` ](#entry) vista para escribir y editar una sola línea de texto.<br /><br />[Documentación de API](xref:Xamarin.Forms.Editor) / [guía](~/xamarin-forms/user-interface/text/editor.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text) | [![Ejemplo de entrada de](views-images/Editor.png "ejemplo Editor")](views-images/Editor-Large.png#lightbox "Editor de ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/EditorDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/EditorDemoPage.xaml) |
|     |     |

## <a name="views-to-indicate-activity"></a>Vistas para indicar actividad

<a name="activityindicator" />

### <a name="activityindicator"></a>ActivityIndicator

|     |     |
| --- | --- |
| [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator) usa una animación para mostrar que la aplicación esté implicada en una actividad larga sin dar ninguna indicación del progreso. El [ `IsRunning` ](xref:Xamarin.Forms.ActivityIndicator.IsRunning) propiedad controla la animación.<br /><br />Si se conoce el progreso de la actividad, use un [ `ProgressBar` ](#progressbar) en su lugar.<br /><br />[Documentación de la API](xref:Xamarin.Forms.ActivityIndicator) | [![Ejemplo de ActivityIndicator](views-images/ActivityIndicator.png "ActivityIndicator ejemplo")](views-images/ActivityIndicator-Large.png#lightbox "ActivityIndicator ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ActivityIndicatorDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ActivityIndicatorDemoPage.xaml) |
|     |     |

<a name="progressbar" />

### <a name="progressbar"></a>ProgressBar

|     |     |
| --- | --- |
| [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) usa una animación para mostrar que la aplicación está progresando a través de una actividad larga. Establecer el [ `Progress` ](xref:Xamarin.Forms.ProgressBar.Progress) propiedad a valores entre 0 y 1 para indicar el progreso.<br /><br />Si se desconoce el progreso de la actividad, use un [ `ActivityIndicator` ](#activityindicator) en su lugar.<br /><br />[Documentación de la API](xref:Xamarin.Forms.ProgressBar) | [![Ejemplo de ProgressBar](views-images/ProgressBar.png "ejemplo ProgressBar")](views-images/ProgressBar-Large.png#lightbox "ejemplo ProgressBar")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ProgressBarDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ProgressBarDemoPage.xaml.cs) |
|     |     |

## <a name="views-that-display-collections"></a>Vistas que muestran colecciones

### <a name="collectionview"></a>CollectionView

|     |     |
| --- | --- |
| [`CollectionView`](xref:Xamarin.Forms.CollectionView) muestra una lista desplazable de elementos de datos seleccionable, utilizando las especificaciones de diseño diferente. Su objetivo es proporcionar una forma más flexible y una alternativa de alto rendimiento [ `ListView` ](xref:Xamarin.Forms.ListView). Establecer el `ItemsSource` propiedad a una colección de objetos y conjunto el `ItemTemplate` propiedad a un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) objeto que describe cómo los elementos son para dar formato. El `SelectionChanged` eventos indica que se ha realizado una selección, que está disponible como la `SelectedItem` propiedad.<br /><br />[Guía de](~/xamarin-forms/user-interface/collectionview/index.md) / [ejemplo](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/) | [![Ejemplo de CollectionView](views-images/CollectionView.png "ejemplo CollectionView")](views-images/CollectionView-Large.png#lightbox "ejemplo CollectionView")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/CodeExamples/CollectionViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/forms40/FormsGallery/FormsGallery/FormsGallery/XamlExamples/CollectionViewDemoPage.xaml) |
|     |     |

<a name="listView" />

### <a name="listview"></a>ListView

|     |     |
| --- | --- |
| [`ListView`](xref:Xamarin.Forms.ListView) se deriva de [ `ItemsView` ](xref:Xamarin.Forms.ItemsView`1) y muestra una lista desplazable de elementos de datos seleccionable. Establecer el [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) propiedad a una colección de objetos y establezca el [ `ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) propiedad a un [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) objeto que describe cómo los elementos son para tener el formato. El [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) eventos indica que se ha realizado una selección, que está disponible como la [ `SelectedItem` ](xref:Xamarin.Forms.ListView.SelectedItem) propiedad.<br /><br />[Documentación de API](xref:Xamarin.Forms.ListView) / [guía](~/xamarin-forms/user-interface/listview/index.md) / [ejemplo](https://developer.xamarin.com/samples/WorkingWithListview) | [![Ejemplo de ListView](views-images/ListView.png "ejemplo de ListView")](views-images/ListView-Large.png#lightbox "ejemplo de ListView")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/ListViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/ListViewDemoPage.xaml) |
|     |     |

### <a name="picker"></a>Selector

|     |     |
| --- | --- |
| [`Picker`](xref:Xamarin.Forms.Picker) muestra un elemento seleccionado de una lista de cadenas de texto y permite al seleccionar este elemento cuando se pulsa la vista. Establecer el [ `Items` ](xref:Xamarin.Forms.Picker.Items) propiedad a una lista de cadenas, o la [ `ItemsSource` ](xref:Xamarin.Forms.Picker.ItemsSource) propiedad a una colección de objetos. El [ `SelectedIndexChanged` ](xref:Xamarin.Forms.Picker.SelectedIndexChanged) evento se desencadena cuando se selecciona un elemento.<br /><br />La `Picker` muestra la lista de elementos solo cuando está seleccionado. Use un [ `ListView` ](#listView) o [ `TableView` ](#tableView) para obtener una lista desplazable que permanece en la página.<br /><br />[Documentación de API](xref:Xamarin.Forms.Picker) / [guía](~/xamarin-forms/user-interface/picker/index.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PickerDemo/) | [![Ejemplo de selector](views-images/Picker.png "ejemplo selector")](views-images/Picker-Large.png#lightbox "ejemplo selector")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/PickerDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml) con [código subyacente](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/PickerDemoPage.xaml.cs) |
|     |     |

<a name="tableView" />

### <a name="tableview"></a>TableView

|     |     |
| --- | --- |
| [`TableView`](xref:Xamarin.Forms.TableView) muestra una lista de filas de tipo [ `Cell` ](xref:Xamarin.Forms.Cell) con encabezados opcionales y subencabezados. Establecer el [ `Root` ](xref:Xamarin.Forms.TableView.Root) propiedad a un objeto de tipo [ `TableRoot` ](xref:Xamarin.Forms.TableRoot)y agregue [ `TableSection` ](xref:Xamarin.Forms.TableSection) objetos a los que `TableRoot`. Cada `TableSection` es una colección de `Cell` objetos.<br /><br />[Documentación de API](xref:Xamarin.Forms.TableView) / [guía](~/xamarin-forms/user-interface/tableview.md) / [ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/TableView) | [![Ejemplo de TableView](views-images/TableView.png "ejemplo TableView")](views-images/TableView-Large.png#lightbox "TableView ejemplo")<br />[Código C# para esta página](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/CodeExamples/TableViewDemoPage.cs) / [página XAML](https://github.com/xamarin/xamarin-forms-samples/blob/master/FormsGallery/FormsGallery/FormsGallery/XamlExamples/TableViewDemoPage.xaml) |
|     |     |

## <a name="related-links"></a>Vínculos relacionados

- [Ejemplo de Xamarin.Forms FormsGallery](https://developer.xamarin.com/samples/FormsGallery/)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/) (Ejemplos de Xamarin.Forms)
- [Documentación de la API de Xamarin.Forms](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
