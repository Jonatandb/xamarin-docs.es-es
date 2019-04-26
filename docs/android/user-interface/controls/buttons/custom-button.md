---
title: Botón personalizado
ms.prod: xamarin
ms.assetid: C523D41E-5855-248D-079D-6B12B74B7617
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: b5ccefa1eb7e659584c1c82481bbd4473a3a8abc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076280"
---
# <a name="custom-button"></a>Botón personalizado

En esta sección, creará un botón con una imagen personalizada en lugar de texto, utilizando el [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) widget y un archivo XML que define tres imágenes diferentes que puede usar para los Estados del botón diferente. Cuando se presiona el botón, se mostrará un mensaje breve.

Haga clic en y descargar las tres imágenes siguientes y, a continuación, cópielos en el **recursos/drawable** directorio del proyecto. Se usará para los Estados del botón diferente.

 [![Icono verde Android para el estado normal](custom-button-images/android-normal.png)](custom-button-images/android-normal.png#lightbox) [ ![naranja Android icono de estado focused](custom-button-images/android-focused.png)](custom-button-images/android-focused.png#lightbox) [ ![icono amarillo Android para el estado presionado](custom-button-images/android-pressed.png)](custom-button-images/android-pressed.png#lightbox)

Cree un nuevo archivo en el **recursos/drawable** directorio denominado **android_button.xml**. Inserte el siguiente código XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@drawable/android_pressed"
          android:state_pressed="true" />
    <item android:drawable="@drawable/android_focused"
          android:state_focused="true" />
    <item android:drawable="@drawable/android_normal" />
</selector>
```

Esto define un único recurso drawable, lo que cambiará su imagen de según el estado actual del botón. La primera `<item>` define **android_pressed.png** como la imagen cuando se presiona el botón (que se haya activado); el segundo `<item>` define **android_focused.png** como la imagen cuando el botón recibe el foco (cuando se resalta el botón con la bola de seguimiento o un mando de dirección); y el tercero `<item>` define **android_normal.png** como imagen para el estado normal (cuando se presiona ni centrado). Este archivo XML ahora representa un único recurso puede dibujar y cuando hace referencia a un [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) para su fondo, la imagen que aparece cambiará en función de estos tres estados.


> [!NOTE]
> El orden de los `<item>` elementos es importante. Cuando se hace referencia este drawable, el `<item>`son recorren en orden para determinar cuál de ellos es adecuado para el estado actual del botón.
> Dado que la imagen "normal" es el última, es sólo se aplica cuando las condiciones `android:state_pressed` y `android:state_focused` evalúen ambas false.

Abra el **Resources/layout/Main.axml** y agréguele el [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) elemento:

```xml
<Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:padding="10dp"
        android:background="@drawable/android_button" />
```

El `android:background` atributo especifica el recurso drawable se utilizará para el fondo del botón (que, cuando se guarda en **Resources/drawable/android.xml**, se hace referencia como `@drawable/android`). Esto reemplaza la imagen de fondo normal utilizada para los botones en todo el sistema. En orden para el drawable cambiar su imagen de según el estado del botón, se debe aplicar la imagen al fondo.

Para que el botón hacer algo cuando presiona, agregue el código siguiente al final de la [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/Android.OS.PersistableBundle/)
método:

```csharp
Button button = FindViewById<Button>(Resource.Id.button);

button.Click += (o, e) => {
    Toast.MakeText (this, "Beep Boop", ToastLength.Short).Show ();
};
```

Esta forma se capturan el [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) del diseño, a continuación, agrega un [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) mensaje que se mostrará cuando el [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) se hace clic en.

Ahora ejecute la aplicación.


*Las partes de esta página son modificaciones en función de trabajo creado y compartido por el Android Open Source Project y usarse de acuerdo con los términos descritos en el*
[*licencia de atribución 2.5 de Creative Commons* ](http://creativecommons.org/licenses/by/2.5/).
