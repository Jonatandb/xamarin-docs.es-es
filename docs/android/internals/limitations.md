---
title: 'Xamarin.Android vs. Escritorio: las diferencias en el tiempo de ejecución Mono'
ms.prod: xamarin
ms.assetid: F953F9B4-3596-8B3A-A8E4-8219B5B9F7CA
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 115d715214d7af3174c41d9d82e894ce429dab42
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60953349"
---
# <a name="limitations"></a>Limitaciones

Puesto que las aplicaciones en Android requieren generar tipos de proxy Java durante el proceso de compilación, no es posible generar todo el código en tiempo de ejecución.

Éstas son las limitaciones de Xamarin.Android en comparación con Mono de escritorio:


## <a name="limited-dynamic-language-support"></a>Compatibilidad con lenguajes dinámicos limitado

 [Contenedores invocables Android](~/android/platform/java-integration/android-callable-wrappers.md) son necesarias siempre que el tiempo de ejecución de Android necesita invocar código administrado. Contenedores invocables Android se generan en tiempo de compilación, según el análisis estático de IL. El resultado de esto: se *no* Utilice lenguajes dinámicos (IronPython, IronRuby, etc.) en cualquier escenario en subclases de tipos de Java se requiere (incluido subclases indirecto), ya que no hay ninguna manera de extraer estos tipos dinámicos en tiempo de compilación para generar los contenedores RCW Android es necesarios.


## <a name="limited-java-generation-support"></a>Compatibilidad de generación de Java limitado

[Contenedores invocables Android](~/android/platform/java-integration/android-callable-wrappers.md) debe generarse para código de Java llamar a código administrado. *De forma predeterminada*, contenedores RCW Android solo contendrá (ciertas) tienen constructores declarados y métodos que invalida un método virtual de Java (es decir, tiene [ `RegisterAttribute` ](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)) o implementar un método de interfaz de Java () interfaz del mismo modo tiene `Attribute`).
  
Antes de la versión 4.1, no se podrían declarar métodos adicionales. Con la versión 4.1, [el `Export` y `ExportField` atributos personalizados se pueden usar para declarar métodos de Java y los campos dentro del contenedor RCW Android](~/android/platform/java-integration/working-with-jni.md).

### <a name="missing-constructors"></a>Constructores que faltan

Constructores permanecen complicados, a menos que [ `ExportAttribute` ](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute) se utiliza. El algoritmo para generar constructores Android contenedor CCW es que se va a emitir un constructor de Java si:

1. Hay una asignación de Java para todos los tipos de parámetro
2. La clase base declara el mismo constructor &ndash; esto es necesario porque el contenedor RCW Android *debe* invocar el constructor de clase base correspondiente; no pueden utilizarse argumentos predeterminados (porque no hay ninguna manera fácil determinar qué valores se deben usar dentro de Java).

Por ejemplo, considere la siguiente clase:

```csharp
[Service]
class MyIntentService : IntentService {
    public MyIntentService (): base ("value")
    {
    }
}
```

Mientras que este parece perfectamente lógico, el contenedor RCW Android resultante *en versiones de lanzamiento* no contendrá un constructor predeterminado. Por lo tanto, si intenta iniciar este servicio (por ejemplo, [ `Context.StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)), se producirá un error:

```shell
E/AndroidRuntime(31766): FATAL EXCEPTION: main
E/AndroidRuntime(31766): java.lang.RuntimeException: Unable to instantiate service example.MyIntentService: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2347)
E/AndroidRuntime(31766):        at android.app.ActivityThread.access$1600(ActivityThread.java:130)
E/AndroidRuntime(31766):        at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1277)
E/AndroidRuntime(31766):        at android.os.Handler.dispatchMessage(Handler.java:99)
E/AndroidRuntime(31766):        at android.os.Looper.loop(Looper.java:137)
E/AndroidRuntime(31766):        at android.app.ActivityThread.main(ActivityThread.java:4745)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invokeNative(Native Method)
E/AndroidRuntime(31766):        at java.lang.reflect.Method.invoke(Method.java:511)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:786)
E/AndroidRuntime(31766):        at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:553)
E/AndroidRuntime(31766):        at dalvik.system.NativeStart.main(Native Method)
E/AndroidRuntime(31766): Caused by: java.lang.InstantiationException: can't instantiate class example.MyIntentService; no empty constructor
E/AndroidRuntime(31766):        at java.lang.Class.newInstanceImpl(Native Method)
E/AndroidRuntime(31766):        at java.lang.Class.newInstance(Class.java:1319)
E/AndroidRuntime(31766):        at android.app.ActivityThread.handleCreateService(ActivityThread.java:2344)
E/AndroidRuntime(31766):        ... 10 more
```

La solución consiste en declarar un constructor predeterminado, con adornar el `ExportAttribute`y establezca el [ `ExportAttribute.SuperStringArgument` ](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/): 

```csharp
[Service]
class MyIntentService : IntentService {
    [Export (SuperArgumentsString = "\"value\"")]
    public MyIntentService (): base("value")
    {
    }

    // ...
}
```


### <a name="generic-c-classes"></a>Genérico C# clases

Genérico C# clases solo se admiten parcialmente. Existen las siguientes limitaciones:


-   No se pueden usar tipos genéricos `[Export]` o `[ExportField`]. Intenta hacer esto, se generará una `XA4207` error.

    ```csharp
    public abstract class Parcelable<T> : Java.Lang.Object, IParcelable
    {
        // Invalid; generates XA4207
        [ExportField ("CREATOR")]
        public static IParcelableCreator CreateCreator ()
        {
            ...
    }
    ```

-   Los métodos genéricos no pueden usar `[Export]` o `[ExportField]`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        
        // Invalid; generates XA4207
        [Export]
        public static void Method<T>(T value)
        {
            ...
        }
    }
    ```

-   `[ExportField]` no puede usarse en métodos que devuelven `void`:

    ```csharp
    public class Example : Java.Lang.Object
    {
        // Invalid; generates XA4208
        [ExportField ("CREATOR")]
        public static void CreateSomething ()
        {
        }
    }
    ```

-   Las instancias de tipos genéricos _no debe_ crearse desde el código de Java.
    Se pueden crear solo de forma segura desde el código administrado:

    ```csharp
    [Activity (Label="Die!", MainLauncher=true)]
    public class BadGenericActivity<T> : Activity
    {
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);
        }
    }
    ```


## <a name="partial-java-generics-support"></a>La compatibilidad con genéricos parciales de Java

La compatibilidad con elementos genéricos de enlace de Java es limitado. Especialmente, los miembros de una clase de instancia genérico que se deriva de otra clase genérica (no crea una instancia) quedan expuestos como Java.Lang.Object. Por ejemplo, [Android.Content.Intent.GetParcelableExtra](https://developer.xamarin.com/api/member/Android.Content.Intent.GetParcelableExtra/p/System.String/) método devuelve Java.Lang.Object. Esto es debido a los genéricos borrados de Java.
Tenemos algunas clases que no son aplicables a esta limitación, pero se ajustan manualmente.


## <a name="related-links"></a>Vínculos relacionados

- [Contenedores que se pueden llamar de Android](~/android/platform/java-integration/android-callable-wrappers.md)
- [Trabajo con JNI](~/android/platform/java-integration/working-with-jni.md)
- [ExportAttribute](https://developer.xamarin.com/api/type/Java.Interop.ExportAttribute/)
- [SuperString](https://developer.xamarin.com/api/property/Java.Interop.ExportAttribute.SuperArgumentsString/)
- [RegisterAttribute](https://developer.xamarin.com/api/type/Android.Runtime.RegisterAttribute/)
