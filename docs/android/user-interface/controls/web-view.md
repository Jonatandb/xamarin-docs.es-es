---
title: Vista Web
ms.prod: xamarin
ms.assetid: 807F214A-166D-B342-0BBA-525517577F6B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 2718953c9e5628374c45fa3741d1ad3be3125dd9
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510146"
---
# <a name="xamarinandroid-web-view"></a>Vista Web de Xamarin. Android

[`WebView`](xref:Android.Webkit.WebView)permite crear su propia ventana para ver páginas web (o incluso desarrollar un explorador completo). En este tutorial, creará un sencillo[`Activity`](xref:Android.App.Activity)
que puede ver y navegar por las páginas Web.

Cree un nuevo proyecto denominado **HelloWebView**.

Abra **Resources/layout/main. axml** e inserte lo siguiente:

```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent" />
```

Dado que esta aplicación tendrá acceso a Internet, debe agregar los permisos adecuados al archivo de manifiesto de Android. Abra las propiedades del proyecto para especificar los permisos que la aplicación necesita para funcionar. Habilite el `INTERNET` permiso como se muestra a continuación:

![Establecer el permiso de INTERNET en el manifiesto de Android](web-view-images/01-set-internet-permissions.png)

Ahora Abra **MainActivity.CS** y agregue una directiva using para WebKit:

```csharp
using Android.Webkit;
```

En la parte superior de `MainActivity` la clase, declare un [`WebView`](xref:Android.Webkit.WebView) objeto:

```csharp
WebView web_view;
```

Cuando se pide a la **vista** web que cargue una dirección URL, delegará de forma predeterminada la solicitud en el explorador predeterminado. Para hacer que la **vista** web cargue la dirección URL (en lugar del explorador predeterminado), debe subclase `Android.Webkit.WebViewClient` e invalidar el `ShouldOverriderUrlLoading` método. Se proporciona una instancia de `WebViewClient` este personalizado `WebView`al. Para ello, agregue la siguiente `HelloWebViewClient` clase anidada en: `MainActivity`

```csharp
public class HelloWebViewClient : WebViewClient
{
    public override bool ShouldOverrideUrlLoading (WebView view, string url)
    {
        view.LoadUrl(url);
        return false;
    }
}
```

Cuando `ShouldOverrideUrlLoading` `WebView` devuelve `false`, indica a Android que la instancia actual ha controlado la solicitud y que no es necesaria ninguna otra acción. 

Si tiene como destino el nivel de API 24 o posterior, use la sobrecarga `ShouldOverrideUrlLoading` de que toma `IWebResourceRequest` un para el segundo argumento en lugar de `string`un:

```csharp
public class HelloWebViewClient : WebViewClient
{
    // For API level 24 and later
    public override bool ShouldOverrideUrlLoading (WebView view, IWebResourceRequest request)
    {
        view.LoadUrl(request.Url.ToString());
        return false;
    }
}
```

A continuación, use el siguiente código para [`OnCreate()`](xref:Android.App.Activity.OnCreate*)el método):

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    web_view = FindViewById<WebView> (Resource.Id.webview);
    web_view.Settings.JavaScriptEnabled = true;
    web_view.SetWebViewClient(new HelloWebViewClient());
    web_view.LoadUrl ("https://www.xamarin.com/university");
}
```

Esto inicializa [`WebView`](xref:Android.Webkit.WebView) el miembro con el [`Activity`](xref:Android.App.Activity) del diseño y habilita JavaScript para el [`WebView`](xref:Android.Webkit.WebView) con `= true` [`JavaScriptEnabled`](xref:Android.Webkit.WebSettings.JavaScriptEnabled) 
 (vea la [llamada C\# desde JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) receta para obtener información sobre cómo llamar a\# funciones de C desde JavaScript). Por último, se carga una página web inicial [`LoadUrl(String)`](xref:Android.Webkit.WebView)con.

Compile y ejecute la aplicación. Debería ver una aplicación sencilla de visor de páginas web como la que se ve en la siguiente captura de pantalla:

[![Ejemplo de aplicación que muestra un WebView](web-view-images/02-simple-webview-app-sml.png)](web-view-images/02-simple-webview-app.png#lightbox)

Para controlar la pulsación de tecla del botón **atrás** , agregue la siguiente instrucción using:

```csharp
using Android.Views;
```

A continuación, agregue el método siguiente dentro `HelloWebView` de la actividad:

```csharp
public override bool OnKeyDown (Android.Views.Keycode keyCode, Android.Views.KeyEvent e)
{
    if (keyCode == Keycode.Back && web_view.CanGoBack ())
    {
        web_view.GoBack ();
        return true;
    }
    return base.OnKeyDown (keyCode, e);
}
```

El[`OnKeyDown(int, KeyEvent)`](xref:Android.App.Activity.OnKeyDown*)
se llamará al método de devolución de llamada siempre que se presione un botón mientras la actividad se está ejecutando. La condición dentro de usa [`KeyEvent`](xref:Android.Views.KeyEvent) el para comprobar si la tecla presionada es el [`WebView`](xref:Android.Webkit.WebView) botón **atrás** y si es realmente capaz de desplazarse hacia atrás (si tiene un historial). Si ambos son true, se llama [`GoBack()`](xref:Android.Webkit.WebView.GoBack) al método, que navegará hacia atrás un paso en el [`WebView`](xref:Android.Webkit.WebView) historial. La `true` devolución indica que el evento se ha controlado. Si no se cumple esta condición, el evento se devuelve al sistema.

Vuelva a ejecutar la aplicación. Ahora debería poder seguir los vínculos y navegar hacia atrás por el historial de páginas:

[![Capturas de pantallas de ejemplo del botón atrás en acción](web-view-images/03-back-button-sml.png)](web-view-images/03-back-button.png#lightbox)

*Algunas partes de esta página son modificaciones basadas en el trabajo creado y compartido por el proyecto de código abierto de Android y que se usan según los términos descritos en la*
licencia de atribución de[*Creative Commons 2,5*](http://creativecommons.org/licenses/by/2.5/).

## <a name="related-links"></a>Vínculos relacionados

- [Call C# from JavaScript](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/webview/call_csharp_from_javascript) (Llamada a C# desde JavaScript)
- [Android.Webkit.WebView](xref:Android.Webkit.WebView)
- [KeyEvent](xref:Android.Webkit.WebView)
