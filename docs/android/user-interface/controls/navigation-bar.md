---
title: Barra de navegación de Xamarin. Android
ms.prod: xamarin
ms.assetid: 6023DB7E-9E72-4B90-A96A-11BC297B8A3D
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/01/2017
ms.openlocfilehash: 70e009ed1a017b2336b6acb443a4d9cd87ff3e68
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510265"
---
# <a name="xamarinandroid-navigation-bar"></a>Barra de navegación de Xamarin. Android

Android 4 presentó una nueva característica de interfaz de usuario del sistema denominada *barra de navegación*, que proporciona controles de navegación en los dispositivos que no incluyen botones de hardware para **Inicio**, **atrás**y **menú**.
En la captura de pantalla siguiente se muestra la barra de navegación desde un dispositivo primo de Nexus:

 [![Ejemplo de una barra de navegación de Android](navigation-bar-images/19-navbar.png)](navigation-bar-images/19-navbar.png#lightbox)

Hay disponibles varias nuevas marcas que controlan la visibilidad de la barra de navegación y sus controles, así como la visibilidad de la barra del sistema que se presentó en Android 3. Las marcas se definen en la `Android.View.View` clase y se enumeran a continuación:

-   `SystemUiFlagVisible`&ndash; Hace que la barra de navegación esté visible. 
-   `SystemUiFlagLowProfile`&ndash; Atenúa los controles de la barra de navegación. 
-   `SystemUiFlagHideNavigation`&ndash; Oculta la barra de navegación. 


Estas marcas se pueden aplicar a cualquier vista de la jerarquía de vistas estableciendo la `SystemUiVisibility` propiedad. Si varias vistas tienen esta propiedad establecida, el sistema las combina con una operación OR y las aplica siempre que la ventana en la que se establecen las marcas conserva el foco. Al quitar una vista, también se quitarán las marcas que haya establecido.

En el ejemplo siguiente se muestra una aplicación sencilla en la que al hacer clic `SystemUiVisibility`en cualquiera de los botones cambia:

 [![Capturas de pantallas en las que se muestra la SystemUiVisibility visible, bajo perfil y oculto](navigation-bar-images/18-systemuivisibility.png)](navigation-bar-images/18-systemuivisibility.png#lightbox)

El código para cambiar el `SystemUiVisibility` establece la propiedad en un `TextView` controlador de eventos Click de cada botón, como se muestra a continuación:

```csharp
var tv = FindViewById<TextView> (Resource.Id.systemUiFlagTextView);
var lowProfileButton = FindViewById<Button>(Resource.Id.lowProfileButton);
var hideNavButton = FindViewById<Button> (Resource.Id.hideNavigation);
var visibleButton = FindViewById<Button> (Resource.Id.visibleButton);
           
lowProfileButton.Click += delegate {
    tv.SystemUiVisibility =
        (StatusBarVisibility)View.SystemUiFlagLowProfile;
};
           
hideNavButton.Click += delegate {
    tv.SystemUiVisibility =
       (StatusBarVisibility)View.SystemUiFlagHideNavigation;        
};
           
visibleButton.Click += delegate {
    tv.SystemUiVisibility = (StatusBarVisibility)View.SystemUiFlagVisible;
}
```

Además, un `SystemUiVisibility` cambio genera un `SystemUiVisibilityChange` evento. Al igual que si `SystemUiVisibility` se establece la propiedad, se `SystemUiVisibilityChange` puede registrar un controlador para el evento para cualquier vista de la jerarquía. Por ejemplo, el código siguiente usa la `TextView` instancia para registrarse para el evento:

```csharp
tv.SystemUiVisibilityChange +=
  delegate(object sender, View.SystemUiVisibilityChangeEventArgs e) {
        tv.Text = String.Format ("Visibility = {0}", e.Visibility);
  };
```



## <a name="related-links"></a>Vínculos relacionados

- [SystemUIVisibilityDemo (sample)](https://developer.xamarin.com/samples/monodroid/SystemUIVisibilityDemo/)
- [Presentación del bocadillo de helado](http://www.android.com/about/ice-cream-sandwich/)
- [Plataforma Android 4,0](https://developer.android.com/sdk/android-4.0.html)
