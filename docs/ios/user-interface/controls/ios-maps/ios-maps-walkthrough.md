---
title: Anotaciones y superposiciones en Xamarin.iOS
description: Este artículo presenta un tutorial paso a paso que muestran cómo trabajar con las características de anotación y superposición del Kit de mapa. Muestra cómo agregar una asignación a una aplicación que muestra una anotación y superposición en la ubicación de la conferencia de 2013 de evolucionar de Xamarin.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 445661513b0cf79df99d54ed0bb4b0261dd75c2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381505"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Anotaciones y superposiciones en Xamarin.iOS

La aplicación, vamos a crear en este tutorial se muestra a continuación:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Una aplicación de ejemplo MapKit")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
Puede encontrar el código completo en el [ejemplo de tutorial de mapas](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/).

Comencemos creando un nuevo **iOS proyecto vacío**y asígnele un nombre relevante. Se empezará agregando código a nuestro controlador de vista para mostrar el objeto MapView y, a continuación, creará nuevas clases para nuestro MapDelegate y las anotaciones personalizadas. Para ello, siga los pasos indicados a continuación:

## <a name="viewcontroller"></a>ViewController


1. Agregue los siguientes espacios de nombres para el `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Agregar un `MKMapView` instancia variable a la clase, junto con un `MapDelegate` instancia. Vamos a crear la `MapDelegate` en breve:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. En el controlador `LoadView` método, agregue un `MKMapView` y establézcalo en la `View` propiedad del controlador:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    A continuación, agregaremos código para inicializar el mapa en el ' ViewDidLoad'' método.

1. En `ViewDidLoad` agregue código para establecer el tipo de mapa, mostrar la ubicación del usuario y permitir el zoom y panorámica:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. A continuación, agregue código para centrar el mapa y establecer su región:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Crear una nueva instancia de `MapDelegate` y asígnelo a la `Delegate` de la `MKMapView`. Una vez más, vamos a implcodeent el `MapDelegate` en breve:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. A partir de iOS 8, debe solicitar autorización del usuario para usar su ubicación, así que vamos a agregar esto a nuestro ejemplo. En primer lugar, defina un `CLLocationManager` variable de nivel de clase:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. En el `ViewDidLoad` método, queremos comprobar si el dispositivo ejecuta la aplicación usa iOS 8 y, si es se va a solicitar autorización la aplicación está en uso:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Por último, hay que modificar el **Info.plist** archivo aconsejar a los usuarios de la razón para solicitar su ubicación. En el **origen** menú de la **Info.plist**, agregue la siguiente clave:
    
    `NSLocationWhenInUseUsageDescription` 
    
    y la cadena: 

    `Maps Walkthrough Docs Sample`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs: una clase para anotaciones personalizadas


1. Vamos a usar una clase personalizada para la anotación que se denomina `ConferenceAnnotation`. Agregue la siguiente clase al proyecto:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapsWalkthrough
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - adición de la anotación y superposición

1. Con el `ConferenceAnnotation` en su lugar, podemos agregar a la asignación. En el `ViewDidLoad` método de la `ViewController`, agregar la anotación en coordenadas de centro del mapa:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. También queremos tener una superposición del hotel. Agregue el código siguiente para crear el `MKPolygon` utilizando las coordenadas proporcionadas en el hotel y agréguelo a la asignación por llamada `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Esto completa el código en `ViewDidLoad`. Ahora debemos implementar nuestra `MapDelegate` clase para controlar la creación de la anotación y vistas de superposición, respectivamente.


## <a name="mapdelegate"></a>MapDelegate

1. Cree una clase denominada `MapDelegate` que herede de `MKMapViewDelegate` e incluyen un `annotationId` variable para usarla como un identificador de la reutilización de la anotación:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Sólo tenemos una anotación para la reutilización de código no es estrictamente necesario, pero es una buena práctica para que lo incluyan.

1. Implemente el `GetViewForAnnotation` método para devolver una vista para la `ConferenceAnnotation` utilizando el **conference.png** imagen incluida en este tutorial:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Cuando el usuario pulsa en la anotación, queremos mostrar una imagen que muestra la ciudad de Austin. Agregue las siguientes variables para el `MapDelegate` para la imagen y la vista para que aparezca:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. A continuación, para mostrar la imagen cuando se pulsa la anotación, implementará la `DidSelectAnnotation` método como sigue:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Para ocultar la imagen cuando el usuario anula la selección de la anotación, puntee en cualquier lugar en el mapa, implemente el `DidSelectAnnotationView` método como sigue:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Ahora tenemos el código para la anotación en su lugar. Lo único que queda es agregar código a la `MapDelegate` para crear la vista de la superposición de hotel.

1. Agregue la siguiente implementación de `GetViewForOverlay` a la `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Ejecute la aplicación. Ahora tenemos un mapa interactivo con una anotación personalizada y una superposición. Puntee en la anotación y se muestra la imagen de Austin, tal como se muestra a continuación:

 [![](ios-maps-walkthrough-images/01-map-image.png "Puntee en la anotación y se muestra la imagen de Austin")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Resumen

En este artículo explicamos cómo agregar una anotación a una asignación, así como la adición de una superposición de un polígono especificado. También se muestra cómo agregar compatibilidad táctil a la anotación que se va a animar una imagen a través de un mapa.


## <a name="related-links"></a>Vínculos relacionados

- [Ejemplo de tutorial de mapas](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/)
- [Ejemplo de asignación de demostración](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [Maps in Xamarin.iOS](~/ios/user-interface/controls/ios-maps/index.md) (Mapas en Xamarin.iOS)
