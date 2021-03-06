---
title: Información general de Unified API
description: La Unified API de Xamarin permite compartir código entre Mac e iOS y admite aplicaciones de 32 y 64 bits con el mismo binario.
ms.prod: xamarin
ms.assetid: 5F0CEC18-5EF6-4A99-9DCF-1A3B57EA157C
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: e683170b048be5ab5cc39fa8560c82916ead5d50
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/09/2020
ms.locfileid: "84570939"
---
# <a name="unified-api-overview"></a>Información general de Unified API

La Unified API de Xamarin permite compartir código entre Mac e iOS y admite aplicaciones de 32 y 64 bits con el mismo binario. El Unified API se utiliza de forma predeterminada en los nuevos proyectos de Xamarin. iOS y Xamarin. Mac.

> [!IMPORTANT]
> La Classic API de Xamarin, que precedía a la Unified API, ha quedado en desuso.
>
> - La última versión de Xamarin. iOS para admitir el Classic API (MonoTouch. dll) era Xamarin. iOS 9,10.
> - Xamarin. Mac sigue admitiendo el Classic API, pero ya no se actualiza. Puesto que está en desuso, los desarrolladores deben trasladar sus aplicaciones a la Unified API.

## <a name="updating-classic-api-based-apps"></a>Actualización de aplicaciones basadas en Classic API

Siga las instrucciones pertinentes para su plataforma:

- [Actualización de aplicaciones existentes](updating-apps.md)
- [Actualización de aplicaciones iOS existentes](updating-ios-apps.md)
- [Actualización de aplicaciones Mac existentes](updating-mac-apps.md)
- [Actualización de aplicaciones Xamarin.Forms existentes](updating-xamarin-forms-apps.md)
- [Migración de un enlace a Unified API](update-binding.md)

## <a name="tips-for-updating-code-to-the-unified-api"></a>[Sugerencias para actualizar el código a Unified API](updating-tips.md)

Independientemente de las aplicaciones que migre, consulte [estas sugerencias](updating-tips.md) para ayudarle a actualizar correctamente a la Unified API.

## <a name="library-split"></a>División de biblioteca

A partir de este punto, nuestras API aparecerán de dos maneras:

- **Classic API:** Limitado a 32 bits (solo) y expuesto en los `monotouch.dll` `XamMac.dll` ensamblados y.
- **Unified API:** Admite el desarrollo de 32 y 64 bits con una única API disponible en `Xamarin.iOS.dll` los `Xamarin.Mac.dll` ensamblados y.

Esto significa que para los desarrolladores empresariales (no dirigidos a la tienda de aplicaciones), puede seguir usando las API clásicas existentes, ya que mantendremos mantenerlas de forma indefinida o puede actualizar a las nuevas API.

<a name="namespace-changes"></a>

## <a name="namespace-changes"></a>Cambios de espacio de nombres

Para reducir la fricción de compartir el código entre nuestros productos de iOS y Mac, vamos a cambiar los espacios de nombres de las API de los productos.

Estamos quitando el prefijo "MonoTouch" de nuestro producto de iOS y "MonoMac" de nuestro producto Mac en los tipos de datos.

Esto hace que sea más sencillo compartir el código entre las plataformas Mac e iOS sin tener que recurrir a la compilación condicional y reducirá el ruido en la parte superior de los archivos de código fuente.

- **Classic API:** Los espacios de nombres usan `MonoTouch.` o `MonoMac.` prefijo.
- **Unified API:** Sin prefijo de espacio de nombres

## <a name="runtime-defaults"></a>Valores predeterminados en tiempo de ejecución

De forma predeterminada, el Unified API usa el recolector de elementos no utilizados **SGen** y el nuevo sistema de [recuento de referencias](~/ios/internals/newrefcount.md) para realizar el seguimiento de la propiedad del objeto. Esta misma característica se ha migrado a Xamarin. Mac.

Esto soluciona varios problemas a los que se enfrentan los desarrolladores con el sistema antiguo y que también facilitan la administración de la [memoria](~/cross-platform/deploy-test/memory-perf-best-practices.md).

Tenga en cuenta que es posible habilitar el nuevo Refcount incluso para el Classic API, pero el valor predeterminado es conservador y no requiere que los usuarios realicen cambios. Con el Unified API, se ha tomado la oportunidad de cambiar el valor predeterminado y ofrecer a los desarrolladores todas las mejoras a la vez que refactorizan y vuelven a probar su código.

## <a name="api-changes"></a>Cambios en la API

En el Unified API se eliminan los métodos en desuso y hay algunos casos en los que se produjeron errores tipográficos en los nombres de API cuando estaban enlazados a los espacios de nombres de MonoMac y MonoTouch originales en las API clásicas. Estas instancias se han corregido en las nuevas API unificadas y deberán actualizarse en las aplicaciones de componente, iOS y Mac. Esta es una lista de las más comunes en las que puede encontrarse:

|Nombre del método Classic API|Nombre del método Unified API|
|--- |--- |
|`UINavigationController.PushViewControllerAnimated()`|`UINavigationController.PushViewController()`|
|`UINavigationController.PopViewControllerAnimated()`|`UINavigationController.PopViewController()`|
|`CGContext.SetRGBFillColor()`|`CGContext.SetFillColor()`|
|`NetworkReachability.SetCallback()`|`NetworkReachability.SetNotification()`|
|`CGContext.SetShadowWithColor`|`CGContext.SetShadow`|
|`UIView.StringSize`|`UIKit.UIStringDrawing.StringSize`|

Para obtener una lista completa de los cambios que se han realizado al cambiar del modelo clásico al Unified API, consulte la documentación sobre las diferencias de la [API clásica (MonoTouch. dll) frente a la unificada (Xamarin. iOS. dll)](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) .

## <a name="updating-to-unified"></a>Actualización a Unified

Varias API antiguas o interrumpidas en el modelo **clásico** no están disponibles en la API **unificada** . Puede ser más fácil corregir las `CS0616` advertencias antes de iniciar la actualización (manual o automatizada), ya que tendrá el `[Obsolete]` mensaje de atributo (parte de la advertencia) para guiarle a la API correcta.

Tenga en cuenta que estamos publicando una [*diferencia*](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md) de los cambios clásicos de la API unificada de vs que se pueden usar antes o después de las actualizaciones del proyecto. La corrección de las llamadas obsoletas en clásico suele ser un ahorro de tiempo (menos búsquedas de documentación).

Siga estas instrucciones para [actualizar aplicaciones iOS existentes](~/cross-platform/macios/unified/updating-ios-apps.md)o [aplicaciones Mac](~/cross-platform/macios/unified/updating-mac-apps.md) a la Unified API.
Revise el resto de esta página y [estas sugerencias](~/cross-platform/macios/unified/updating-tips.md) para obtener más información sobre cómo migrar el código.

### <a name="nuget"></a>NuGet

Los paquetes NuGet que anteriormente admitía Xamarin. iOS a través de la Classic API publicaron sus ensamblados mediante el moniker de plataforma **Monotouch10** .

El Unified API introduce un nuevo identificador de plataforma para los paquetes compatibles: **Xamarin. iOS10**. Los paquetes de NuGet existentes deberán actualizarse para agregar compatibilidad con esta plataforma, compilando en el Unified API.

> [!IMPORTANT]
> Si tiene un error con el formato _"el error 3 no puede incluir ' MonoTouch. dll ' y ' Xamarin. iOS. dll ' en el mismo Xamarin. se hace referencia de forma explícita al proyecto de iOS: ' Xamarin. iOS. dll ', mientras que ' XXX, version = 0.0.000, Culture = neutral, PublicKeyToken = null ' ' hace referencia a ' NoTouch. dll_ ' después de convertir la aplicación en las API unificadas. por lo general, se debe a que hay un componente o un paquete NuGet en el proyecto que no se ha actualizado Unified API Deberá quitar el componente o NuGet existente, actualizar a una versión que admita las API unificadas y realizar una compilación limpia.

### <a name="the-road-to-64-bits"></a>El camino a 64 bits

Para obtener información general sobre la compatibilidad con aplicaciones de 32 y 64 bits, así como sobre los marcos de trabajo, consulte las consideraciones de la [plataforma de bits 32 y 64](~/cross-platform/macios/32-and-64/index.md).

 <a name="new-data-types"></a>

#### <a name="new-data-types"></a>Nuevos tipos de datos

En el núcleo de la diferencia, las API de Mac e iOS usan tipos de datos específicos de la arquitectura que siempre son de 32 bits en plataformas de 32 bits y de 64 bits en plataformas de 64 bits.

Por ejemplo, Objective-C asigna el `NSInteger` tipo de datos a `int32_t` en sistemas de 32 bits y a `int64_t` en sistemas de 64 bits.

Para que coincida con este comportamiento, en nuestro Unified API vamos a reemplazar los usos anteriores de `int` (que en .net se definen como siempre `System.Int32` ) a un nuevo tipo de datos: `System.nint` .  Puede pensar en "n" como "Native", por lo que el tipo de entero nativo de la plataforma.

Estamos introduciendo `nint` `nuint` y `nfloat` proporcionando también tipos de datos creados sobre ellos cuando sea necesario.

Para obtener más información sobre estos cambios de tipos de datos, vea el documento [tipos nativos](~/cross-platform/macios/nativetypes.md) .

### <a name="how-to-detect-the-architecture-of-ios-apps"></a>Cómo detectar la arquitectura de las aplicaciones de iOS

Podría haber situaciones en las que la aplicación necesita saber si se está ejecutando en un sistema iOS de 32 bits o de 64 bits. El siguiente código se puede usar para comprobar la arquitectura:

```csharp
if (IntPtr.Size == 4) {
    Console.WriteLine ("32-bit App");
} else if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit App");
}
```

<a name="deprecated-apis"></a>

### <a name="arrays-and-systemcollectionsgeneric"></a>Matrices y System. Collections. Generic

Dado que los indizadores de C# esperan un tipo de `int` , tendrá que convertir explícitamente `nint` los valores a `int` para tener acceso a los elementos de una colección o matriz. Por ejemplo:

```csharp
public List<string> Names = new List<string>();
...

public string GetName(nint index) {
    return Names[(int)index];
}

```

Este es el comportamiento esperado, ya que la conversión de `int` a `nint` es una pérdida en 64 bits, no se realiza una conversión implícita.

### <a name="converting-datetime-to-nsdate"></a>Convertir DateTime en NSDate

Cuando se usan las API unificadas, `DateTime` `NSDate` ya no se realiza la conversión implícita de los valores de a. Estos valores se deben convertir explícitamente de un tipo a otro. Los siguientes métodos de extensión se pueden usar para automatizar este proceso:

```csharp
public static DateTime NSDateToDateTime(this NSDate date)
{
    // NSDate has a wider range than DateTime, so clip
    // the converted date to DateTime.Min|MaxValue.
    double secs = date.SecondsSinceReferenceDate;
    if (secs < -63113904000)
        return DateTime.MinValue;
    if (secs > 252423993599)
        return DateTime.MaxValue;
    return (DateTime) date;
}

public static NSDate DateTimeToNSDate(this DateTime date)
{
    if (date.Kind == DateTimeKind.Unspecified)
        date = DateTime.SpecifyKind (date, /* DateTimeKind.Local or DateTimeKind.Utc, this depends on each app */)
    return (NSDate) date;
}

```

<a name="deprecated-typos"></a>

### <a name="deprecated-apis-and-typos"></a>API y errores tipográficos en desuso

Dentro de la API clásica de Xamarin. iOS (MonoTouch. dll) el `[Obsolete]` atributo se usaba de dos maneras diferentes:

- **API de iOS desusada:** Esto se debe a que Apple le sugiere que deje de usar una API porque se ha sustituido por otra más reciente. El Classic API sigue siendo preciso y a menudo es necesario (si admite la versión anterior de iOS).
 Dicha API (y el `[Obsolete]` atributo) se incluyen en los nuevos ensamblados de Xamarin. iOS.
- **API incorrecta** Algunas API tenían errores tipográficos en sus nombres.

En el caso de los ensamblados originales (MonoTouch. dll y XamMac. dll), mantuvimos el código anterior disponible por compatibilidad, pero se han quitado de los ensamblados de Unified API (Xamarin. iOS. dll y Xamarin. Mac).

<a name="NSObject_ctor"></a>

### <a name="nsobject-subclasses-ctorintptr"></a>Subclases de NSObject. ctor (IntPtr)

Cada `NSObject` subclase tiene un constructor que acepta un `IntPtr` . Así es como se pueden crear instancias de una nueva instancia administrada a partir de un identificador de ObjC nativo.

En el clásico, este era un `public` constructor. Sin embargo, era fácil utilizar el uso incorrecto de esta característica en el código de usuario, por ejemplo, la creación de varias instancias administradas para una sola instancia de ObjC *o* la creación de una instancia administrada que no tuviera el estado administrado esperado (para las subclases).

Para evitar este tipo de problemas `IntPtr` , los constructores están ahora `protected` en la API **unificada** , para que solo se usen para la creación de subclases. Esto garantizará que la API correcta o segura se usa para crear una instancia administrada a partir de identificadores, es decir,

```csharp
var label = Runtime.GetNSObject<UILabel> (handle);
```

Esta API devolverá una instancia administrada existente (si ya existe) o creará una nueva (si es necesario). Ya está disponible en la API clásica y la unificada.

Tenga en cuenta que `.ctor(NSObjectFlag)` ahora también es, `protected` pero este no se usaba rara vez fuera de la subclase.

<a name="NSAction"></a>

### <a name="nsaction-replaced-with-action"></a>NSAction reemplazado por acción

Con las API unificadas, se ha `NSAction` quitado en favor de .net estándar `Action` . Esta es una gran mejora porque `Action` es un tipo .net común, mientras que `NSAction` era específico de Xamarin. iOS. Ambos hacen exactamente lo mismo, pero eran tipos distintos e incompatibles y dieron lugar a que se tuviera que escribir más código para lograr el mismo resultado.

Por ejemplo, si la aplicación de Xamarin existente incluye el código siguiente:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (new NSAction (delegate() {
    ShowDropDownAnimated (tblDataView);
}));
```

Ahora se puede reemplazar por una expresión lambda simple:

```csharp
UITapGestureRecognizer singleTap = new UITapGestureRecognizer (() => ShowDropDownAnimated(tblDataView));
```

Anteriormente, esto sería un error del compilador porque `Action` no se puede asignar a `NSAction` , pero como `UITapGestureRecognizer` ahora toma un en `Action` lugar de un `NSAction` que es válido en las API unificadas.

### <a name="custom-delegates-replaced-with-actiont"></a>Delegados personalizados reemplazados por acción\<T>

En **unificado** , algunos delegados de .net sencillos (por ejemplo, un parámetro) se reemplazaron por `Action<T>` . Por ejemplo,

```csharp
public delegate void NSNotificationHandler (NSNotification notification);
```

ahora se puede usar como `Action<NSNotification>` . Esto promueve la reutilización de código y reduce la duplicación de código dentro de Xamarin. iOS y sus propias aplicaciones.

### <a name="taskbool-replaced-with-taskbooleannserror"></a>Tarea \<bool> reemplazada por task<Boolean, NSError>>

En el modelo **clásico** , algunas API asincrónicas devuelven `Task<bool>` . Sin embargo, algunos de ellos son los que se usan cuando una `NSError` formaba parte de la firma, es decir, que `bool` ya era `true` y tenía que detectar una excepción para obtener `NSError` .

Dado que algunos errores son muy comunes y el valor devuelto no era útil, este patrón se cambió en **Unified** para devolver `Task<Tuple<Boolean,NSError>>` . Esto le permite comprobar el éxito y cualquier error que se haya producido durante la llamada asincrónica.

### <a name="nsstring-vs-string"></a>NSString frente a cadena

En algunos casos, algunas constantes tuvieron que cambiarse de `string` a `NSString` , por ejemplo,`UITableViewCell`

**Clásico**

```csharp
public virtual string ReuseIdentifier { get; }
```

**Unificado**

```csharp
public virtual NSString ReuseIdentifier { get; }
```

En general, se prefiere el `System.String` tipo .net. Sin embargo, a pesar de las directrices de Apple, algunas API nativas comparan punteros constantes (no la propia cadena) y esto solo puede funcionar cuando se exponen las constantes como `NSString` .

 <a name="protocols"></a>

### <a name="objective-c-protocols"></a>Protocolos de Objective-C

El MonoTouch original no tenía compatibilidad total con los protocolos ObjC y algunas API no óptimas se agregaron para admitir el escenario más común. Esta limitación ya no existe, pero, para mantener la compatibilidad con versiones anteriores, se mantienen varias API dentro `monotouch.dll` de y `XamMac.dll` .

Estas limitaciones se han quitado y limpiado en las API unificadas. La mayoría de los cambios tendrán el siguiente aspecto:

**Clásico**

```csharp
public virtual AVAssetResourceLoaderDelegate Delegate { get; }
```

**Unificado**

```csharp
public virtual IAVAssetResourceLoaderDelegate Delegate { get; }
```

El `I` prefijo significa que **Unified** expone una interfaz, en lugar de un tipo específico, para el protocolo ObjC. Esto facilitará los casos en los que no desea subclases del tipo específico que Xamarin. iOS proporcionó.

También permitió que algunas API sean más precisas y fáciles de usar, por ejemplo:

**Clásico**

```csharp
public virtual void SelectionDidChange (NSObject uiTextInput);
```

**Unificado**

```csharp
public virtual void SelectionDidChange (IUITextInput uiTextInput);
```

Estas API son ahora más fáciles de usar, sin hacer referencia a la documentación, y la finalización del código del IDE le proporcionará sugerencias más útiles basadas en el protocolo o la interfaz.

#### <a name="nscoding-protocol"></a>Protocolo NSCoding

Nuestro enlace original incluía un. ctor (NSCoder) para cada tipo, aunque no era compatible con el `NSCoding` Protocolo.  Un único `Encode(NSCoder)` método estaba presente en `NSObject` para codificar el objeto.
Pero este método solo funcionaría si la instancia se ajusta al protocolo NSCoding.

En el Unified API hemos corregido este problema.  Los nuevos ensamblados solo tendrán `.ctor(NSCoder)` si el tipo se ajusta a `NSCoding` . Además, estos tipos ahora tienen un `Encode(NSCoder)` método que se ajusta a la `INSCoding` interfaz.

Bajo impacto: en la mayoría de los casos, este cambio no afectará a las aplicaciones, ya que no se pueden usar los constructores antiguos, quitados.

## <a name="further-tips"></a>Más sugerencias

Los cambios adicionales que se deben tener en cuenta se enumeran en las [sugerencias para actualizar aplicaciones al Unified API](~/cross-platform/macios/unified/updating-tips.md).


## <a name="related-links"></a>Vínculos relacionados

- [Actualización de aplicaciones de iOS](updating-ios-apps.md)
- [Actualización de aplicaciones Mac](updating-mac-apps.md)
- [Actualización de aplicaciones de Xamarin. Forms](updating-xamarin-forms-apps.md)
- [Actualizar enlaces](update-binding.md)
- [Actualizar sugerencias](updating-tips.md)
- [Diferencias de Unified API clásicas](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
- [Trabajo con tipos nativos en aplicaciones multiplataforma](~/cross-platform/macios/native-types-cross-platform.md)
