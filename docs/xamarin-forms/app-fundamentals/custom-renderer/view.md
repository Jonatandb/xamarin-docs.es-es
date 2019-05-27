---
title: Implementación de una vista
description: En este artículo se explica cómo crear un representador personalizado para un control personalizado de Xamarin.Forms que se usa para mostrar una secuencia de vídeo de vista previa de la cámara del dispositivo.
ms.prod: xamarin
ms.assetid: 915E25E7-4A6B-4F34-B7B4-07D5F4B240F2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/10/2018
ms.openlocfilehash: 88be56cae52e881792ec7a187ef7e158790e8a1b
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926599"
---
# <a name="implementing-a-view"></a>Implementación de una vista

[![Descargar ejemplo](~/media/shared/download.png) Descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/View/)

_Los controles de interfaz de usuario personalizados de Xamarin.Forms deben derivar de la clase View, que se usa para colocar diseños y controles en la pantalla. En este artículo se muestra cómo crear un representador personalizado para un control personalizado de Xamarin.Forms que se usa para mostrar una secuencia de vídeo de vista previa de la cámara del dispositivo._

Todas las vistas de Xamarin.Forms tienen un representador adjunto para cada plataforma que crea una instancia de un control nativo. Cuando una aplicación de Xamarin.Forms representa una [`View`](xref:Xamarin.Forms.View) en iOS se crea la instancia de la clase `ViewRenderer`, que a su vez crea una instancia del control `UIView` nativo. En la plataforma de Android, la clase `ViewRenderer` crea una instancia de un control `View` nativo. En Plataforma universal de Windows (UWP), la clase `ViewRenderer` crea una instancia de un control `FrameworkElement` nativo. Para obtener más información sobre el representador y las clases de control nativo a las que se asignan los controles de Xamarin.Forms, vea [Renderer Base Classes and Native Controls](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md) (Clases base y controles nativos del representador).

El siguiente diagrama muestra la relación entre la clase [`View`](xref:Xamarin.Forms.View) y los controles nativos correspondientes que la implementan:

![](view-images/view-classes.png "Relación entre la clase View y sus clases nativas de implementación")

El proceso de representación puede usarse para implementar personalizaciones específicas de plataforma al crear un representador personalizado para una clase [`View`](xref:Xamarin.Forms.View) en cada plataforma. Para hacerlo, siga este procedimiento:

1. [Cree](#Creating_the_Custom_Control) un control personalizado de Xamarin.Forms.
1. [Use](#Consuming_the_Custom_Control) el control personalizado de Xamarin.Forms.
1. [Cree](#Creating_the_Custom_Renderer_on_each_Platform) el representador personalizado para el control en cada plataforma.

Ahora se analizará en detalle cada elemento, para implementar un representador `CameraPreview` que muestre una secuencia de vídeo de vista previa de la cámara del dispositivo. Pulsar en la secuencia de vídeo la detendrá e iniciará.

<a name="Creating_the_Custom_Control" />

## <a name="creating-the-custom-control"></a>Creación de un control personalizado

Se puede crear un control personalizado mediante la creación de subclases de la clase [`View`](xref:Xamarin.Forms.View), como se muestra en el siguiente ejemplo de código:

```csharp
public class CameraPreview : View
{
  public static readonly BindableProperty CameraProperty = BindableProperty.Create (
    propertyName: "Camera",
    returnType: typeof(CameraOptions),
    declaringType: typeof(CameraPreview),
    defaultValue: CameraOptions.Rear);

  public CameraOptions Camera {
    get { return (CameraOptions)GetValue (CameraProperty); }
    set { SetValue (CameraProperty, value); }
  }
}
```

El control personalizado `CameraPreview` se crea en el proyecto de biblioteca de .NET Standard y define la API del control. El control personalizado expone una propiedad `Camera` que se usa para controlar si se debe mostrar la secuencia de vídeo desde la cámara delantera o trasera del dispositivo. Si no se especifica un valor para la propiedad `Camera` cuando se crea el control, el valor predeterminado es especificar la cámara trasera.

<a name="Consuming_the_Custom_Control" />

## <a name="consuming-the-custom-control"></a>Uso del control personalizado

En XAML, se puede hacer referencia al control personalizado `CameraPreview` en el proyecto de biblioteca de .NET Standard mediante la declaración de un espacio de nombres para su ubicación y el uso del prefijo del espacio de nombres en el elemento de control personalizado. El siguiente ejemplo de código muestra cómo se puede usar el control personalizado `CameraPreview` en una página XAML:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
             ...>
    <ContentPage.Content>
        <StackLayout>
            <Label Text="Camera Preview:" />
            <local:CameraPreview Camera="Rear"
              HorizontalOptions="FillAndExpand" VerticalOptions="FillAndExpand" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

El prefijo de espacio de nombres `local` puede tener cualquier nombre. Pero los valores `clr-namespace` y `assembly` deben coincidir con los detalles del control personalizado. Una vez que se declara el espacio de nombres, el prefijo se usa para hacer referencia al control personalizado.

El siguiente ejemplo de código muestra cómo se puede usar el control personalizado `CameraPreview` en una página C#:

```csharp
public class MainPageCS : ContentPage
{
  public MainPageCS ()
  {
    ...
    Content = new StackLayout {
      Children = {
        new Label { Text = "Camera Preview:" },
        new CameraPreview {
          Camera = CameraOptions.Rear,
          HorizontalOptions = LayoutOptions.FillAndExpand,
          VerticalOptions = LayoutOptions.FillAndExpand
        }
      }
    };
  }
}
```

Se usará una instancia del control personalizado `CameraPreview` para mostrar la secuencia de vídeo de vista previa de la cámara del dispositivo. Aparte de especificar opcionalmente un valor para la propiedad `Camera`, la personalización del control se llevará a cabo en el representador personalizado.

Ahora se puede agregar un representador personalizado a cada proyecto de aplicación para crear controles de vista previa de cámara específicos de la plataforma.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Creación del representador personalizado en cada plataforma

El proceso para crear la clase del representador personalizado es el siguiente:

1. Cree una subclase de la clase `ViewRenderer<T1,T2>` que representa el control personalizado. El primer argumento de tipo debe ser el control personalizado para el que es el representador, en este caso `CameraPreview`. El segundo argumento de tipo debe ser el control nativo que va a implementar el control personalizado.
1. Invalide el método `OnElementChanged` que representa el control personalizado y escriba lógica para personalizarlo. Se llama a este método cuando se crea el correspondiente control de Xamarin.Forms.
1. Agregue un atributo `ExportRenderer` a la clase del representador personalizado para especificar que se va a usar para representar el control personalizado de Xamarin.Forms. Este atributo se usa para registrar el representador personalizado con Xamarin.Forms.

> [!NOTE]
> Para la mayoría de los elementos de Xamarin.Forms, proporcionar un representador personalizado en cada proyecto de la plataforma es un paso opcional. Si no hay un representador personalizado registrado, se usa el representador predeterminado de la clase base del control. Pero los representadores personalizados son necesarios en cada proyecto de plataforma al representar un elemento [View](xref:Xamarin.Forms.View).

El siguiente diagrama muestra las responsabilidades de cada proyecto de la aplicación de ejemplo, junto con las relaciones entre ellos:

![](view-images/solution-structure.png "Responsabilidades de proyecto del representador personalizado de CameraPreview")

El control personalizado `CameraPreview` se representa mediante clases de representador específicas de la plataforma, que se derivan de la clase `ViewRenderer` de cada plataforma. Esto da lugar a que cada control personalizado `CameraPreview` se represente con controles específicos de la plataforma, como se muestra en las capturas de pantalla siguientes:

![](view-images/screenshots.png "CameraPreview en cada plataforma")

La clase `ViewRenderer` expone el método `OnElementChanged`, al que se llama cuando se crea el control personalizado de Xamarin.Forms para representar el control nativo correspondiente. Este método toma un parámetro `ElementChangedEventArgs` que contiene propiedades `OldElement` y `NewElement`. Estas propiedades representan al elemento de Xamarin.Forms al que *estaba* asociado el representador y al elemento de Xamarin.Forms al que *está* asociado el representador, respectivamente. En la aplicación de ejemplo, la propiedad `OldElement` es `null` y la propiedad `NewElement` contiene una referencia a la instancia de `CameraPreview`.

Una versión invalidada del método `OnElementChanged`, en cada clase de representador específica de la plataforma, es el lugar en el que realizar la personalización y la creación de instancias del control nativo. Se debe usar el método `SetNativeControl` para crear instancias del control nativo; además, este método también asigna la referencia del control a la propiedad `Control`. Además, se puede obtener una referencia al control de Xamarin.Forms que se representa mediante la propiedad `Element`.

En algunas circunstancias, se puede llamar al método `OnElementChanged` varias veces. Por lo tanto, para evitar pérdidas de memoria, se debe tener cuidado a la hora de crear instancias de un nuevo control nativo. El enfoque que usar al crear instancias de un nuevo control nativo en un presentador personalizado se muestra en el ejemplo de código siguiente:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<NativeListView> e)
{
  base.OnElementChanged (e);

  if (Control == null) {
    // Instantiate the native control and assign it to the Control property with
    // the SetNativeControl method
  }

  if (e.OldElement != null) {
    // Unsubscribe from event handlers and cleanup any resources
  }

  if (e.NewElement != null) {
    // Configure the control and subscribe to event handlers
  }
}
```

Solo se debe crear una instancia de un nuevo control nativo una vez, cuando la propiedad `Control` es `null`. Solo se debe configurar el control y suscribir los controladores de eventos cuando se adjunta el presentador personalizado a un nuevo elemento de Xamarin.Forms. De forma similar, solo se debe cancelar la suscripción de los controladores de eventos que se han suscrito cuando cambia el elemento al que está asociado el representador. La adopción de este enfoque ayuda a crear un representador personalizado eficaz que no sufra pérdidas de memoria.

Cada clase de representador personalizado se decora con un atributo `ExportRenderer` que registra el representador con Xamarin.Forms. El atributo toma dos parámetros: el nombre de tipo del control personalizado de Xamarin.Forms que se va a representar y el nombre de tipo del representador personalizado. El prefijo `assembly` para el atributo especifica que el atributo se aplica a todo el ensamblado.

En las secciones siguientes se describe la implementación de cada clase de representador personalizado específico de plataforma.

### <a name="creating-the-custom-renderer-on-ios"></a>Creación del representador personalizado en iOS

El siguiente ejemplo de código muestra el representador personalizado para la plataforma iOS:

```csharp
[assembly: ExportRenderer (typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.iOS
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, UICameraPreview>
    {
        UICameraPreview uiCameraPreview;

        protected override void OnElementChanged (ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged (e);

            if (Control == null) {
                uiCameraPreview = new UICameraPreview (e.NewElement.Camera);
                SetNativeControl (uiCameraPreview);
            }
            if (e.OldElement != null) {
                // Unsubscribe
                uiCameraPreview.Tapped -= OnCameraPreviewTapped;
            }
            if (e.NewElement != null) {
                // Subscribe
                uiCameraPreview.Tapped += OnCameraPreviewTapped;
            }
        }

        void OnCameraPreviewTapped (object sender, EventArgs e)
        {
            if (uiCameraPreview.IsPreviewing) {
                uiCameraPreview.CaptureSession.StopRunning ();
                uiCameraPreview.IsPreviewing = false;
            } else {
                uiCameraPreview.CaptureSession.StartRunning ();
                uiCameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Siempre que la propiedad `Control` sea `null`, se llama al método `SetNativeControl` para crear instancias de un nuevo control `UICameraPreview` y asignar una referencia a él para la propiedad `Control`. El control `UICameraPreview` es un control personalizado específico de la plataforma que utiliza las API de `AVCapture` para proporcionar la secuencia de vista previa de la cámara. Expone un evento `Tapped` que controla el método `OnCameraPreviewTapped` para detener e iniciar la vista previa de vídeo cuando se pulsa. Se establece una suscripción al evento `Tapped` cuando el representador personalizado se adjunta a un nuevo elemento de Xamarin.Forms y solo se cancela la suscripción cuando el elemento al que está adjunto el representador cambia.

### <a name="creating-the-custom-renderer-on-android"></a>Creación del representador personalizado en Android

En el ejemplo de código siguiente se muestra el representador personalizado para la plataforma Android:

```csharp
[assembly: ExportRenderer(typeof(CustomRenderer.CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.Droid
{
    public class CameraPreviewRenderer : ViewRenderer<CustomRenderer.CameraPreview, CustomRenderer.Droid.CameraPreview>
    {
        CameraPreview cameraPreview;

        public CameraPreviewRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(ElementChangedEventArgs<CustomRenderer.CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                cameraPreview = new CameraPreview(Context);
                SetNativeControl(cameraPreview);
            }

            if (e.OldElement != null)
            {
                // Unsubscribe
                cameraPreview.Click -= OnCameraPreviewClicked;
            }
            if (e.NewElement != null)
            {
                Control.Preview = Camera.Open((int)e.NewElement.Camera);

                // Subscribe
                cameraPreview.Click += OnCameraPreviewClicked;
            }
        }

        void OnCameraPreviewClicked(object sender, EventArgs e)
        {
            if (cameraPreview.IsPreviewing)
            {
                cameraPreview.Preview.StopPreview();
                cameraPreview.IsPreviewing = false;
            }
            else
            {
                cameraPreview.Preview.StartPreview();
                cameraPreview.IsPreviewing = true;
            }
        }
        ...
    }
}
```

Siempre que la propiedad `Control` sea `null`, se llama al método `SetNativeControl` para crear instancias de un nuevo control `CameraPreview` y asignar una referencia a él para la propiedad `Control`. El control `CameraPreview` es un control personalizado específico de la plataforma que utiliza la API de `Camera` para proporcionar la secuencia de vista previa de la cámara. Después, el control `CameraPreview` se configura, siempre que el representador personalizado esté asociado a un nuevo elemento de Xamarin.Forms. Esta configuración implica la creación de un nuevo objeto `Camera` nativo para acceder a una cámara de hardware concreto y registrar un controlador de eventos para procesar el evento de `Click`. A su vez este controlador detendrá e iniciará la vista previa de vídeo cuando se pulse. Se cancela la suscripción del evento `Click` solo si cambia el representador al que está adjunto el elemento de Xamarin.Forms.

### <a name="creating-the-custom-renderer-on-uwp"></a>Creación del representador personalizado en UWP

En el siguiente ejemplo de código se muestra el representador personalizado para UWP:

```csharp
[assembly: ExportRenderer(typeof(CameraPreview), typeof(CameraPreviewRenderer))]
namespace CustomRenderer.UWP
{
    public class CameraPreviewRenderer : ViewRenderer<CameraPreview, Windows.UI.Xaml.Controls.CaptureElement>
    {
        ...
        CaptureElement _captureElement;
        bool _isPreviewing;

        protected override void OnElementChanged(ElementChangedEventArgs<CameraPreview> e)
        {
            base.OnElementChanged(e);

            if (Control == null)
            {
                ...
                _captureElement = new CaptureElement();
                _captureElement.Stretch = Stretch.UniformToFill;

                SetupCamera();
                SetNativeControl(_captureElement);
            }
            if (e.OldElement != null)
            {
                // Unsubscribe
                Tapped -= OnCameraPreviewTapped;
                ...
            }
            if (e.NewElement != null)
            {
                // Subscribe
                Tapped += OnCameraPreviewTapped;
            }
        }

        async void OnCameraPreviewTapped(object sender, TappedRoutedEventArgs e)
        {
            if (_isPreviewing)
            {
                await StopPreviewAsync();
            }
            else
            {
                await StartPreviewAsync();
            }
        }
        ...
    }
}
```

Siempre que la propiedad `Control` sea `null`, se crea una instancia de un nuevo `CaptureElement` y se llama al método `SetupCamera`, que usa la API de `MediaCapture` para proporcionar la secuencia de vista previa de la cámara. Después se llama al método `SetNativeControl` para asignar una referencia a la instancia de `CaptureElement` para la propiedad `Control`. El control `CaptureElement` expone un evento `Tapped` que controla el método `OnCameraPreviewTapped` para detener e iniciar la vista previa de vídeo cuando se pulsa. Se establece una suscripción al evento `Tapped` cuando el representador personalizado se adjunta a un nuevo elemento de Xamarin.Forms y solo se cancela la suscripción cuando el elemento al que está adjunto el representador cambia.

> [!NOTE]
> Es importante detener y eliminar los objetos que proporcionan acceso a la cámara en una aplicación de UWP. Si no lo hace puede interferir con otras aplicaciones que intentan acceder a la cámara del dispositivo. Para obtener más información, vea [Display the camera preview](/windows/uwp/audio-video-camera/simple-camera-preview-access/) (Mostar la vista previa de la cámara).

## <a name="summary"></a>Resumen

En este artículo se mostró cómo crear un representador personalizado para un control personalizado de Xamarin.Forms que se usa para mostrar una secuencia de vídeo de vista previa de la cámara del dispositivo. Los controles de interfaces de usuario personalizadas de Xamarin.Forms deben derivar de la clase [`View`](xref:Xamarin.Forms.View), que se usa para colocar los diseños y los controles en la pantalla.


## <a name="related-links"></a>Vínculos relacionados

- [CustomRendererView (sample)](https://developer.xamarin.com/samples/xamarin-forms/CustomRenderers/View/) (CustomRendererView [ejemplo])
