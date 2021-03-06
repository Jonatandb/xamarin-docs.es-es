---
title: 'Xamarin.Essentials: Preferencias'
description: En este documento se describe la clase Preferences de Xamarin.Essentials, que guarda las preferencias de la aplicación en un almacén de clave y valor. Se describe cómo usar la clase y los tipos de datos que se pueden almacenar.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 01/15/2019
ms.custom: video
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: acc0c48776c7a91e9e5a060928564bc6e0c1d775
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801821"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: Preferencias

La clase **Preferences** ayuda a almacenar las preferencias de la aplicación en un almacén de clave y valor.

## <a name="get-started"></a>Primeros pasos

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-preferences"></a>Uso de Preferences

Agregue una referencia a Xamarin.Essentials en la clase:

```csharp
using Xamarin.Essentials;
```

Para guardar un valor para una _clave_ determinada en las preferencias:

```csharp
Preferences.Set("my_key", "my_value");
```

Para recuperar un valor de las preferencias o un valor predeterminado si no se establece:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Para comprobar si una clave _determinada_ existe en las preferencias:

```csharp
bool hasKey = Preferences.ContainsKey("my_key");
```

Para quitar la _clave_ de las preferencias:

```csharp
Preferences.Remove("my_key");
```

Para quitar todas las preferencias:

```csharp
Preferences.Clear();
```

Además de estos métodos, cada una toma un valor `sharedName` opcional que se puede usar para crear contenedores adicionales para la preferencia. Lea los detalles de implementación de la plataforma a continuación.

## <a name="supported-data-types"></a>Tipos de datos admitidos

En **Preferences** se admiten los tipos de datos siguientes:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="integrate-with-system-settings"></a>Integración con la configuración del sistema

Las preferencias se almacenan de forma nativa, lo que permite integrar la configuración en la configuración nativa del sistema. Siga la documentación y los ejemplos de la plataforma para integrarlos con ella:

* Apple: [Implementación de un lote de configuración de iOS](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/UserDefaults/Preferences/Preferences.html)
* [Ejemplo de preferencias de la aplicación de iOS](https://docs.microsoft.com/samples/xamarin/ios-samples/appprefs/)
* [Configuración de watchOS](https://developer.xamarin.com/guides/ios/watch/working-with/settings/)
* Android: [Introducción a las pantallas de configuración](https://developer.android.com/guide/topics/ui/settings.html)

## <a name="implementation-details"></a>Detalles de implementación

Los valores de `DateTime` se almacenan en un formato binario de 64 bits (entero largo) mediante dos métodos definidos por la clase `DateTime`: El método [`ToBinary`](xref:System.DateTime.ToBinary) se usa para codificar el valor `DateTime`, mientras que el método [`FromBinary`](xref:System.DateTime.FromBinary(System.Int64)) descodifica el valor. Vea la documentación de estos métodos para obtener los ajustes que se podrían realizar en los valores descodificados cuando se almacena un valor `DateTime` que no es de hora universal coordinada (UTC).

## <a name="platform-implementation-specifics"></a>Detalles de implementación de la plataforma

# <a name="android"></a>[Android](#tab/android)

Todos los datos se almacenan en [Preferencias compartidas](https://developer.android.com/training/data-storage/shared-preferences.html). Si no se especifica ningún `sharedName`, se usan las preferencias compartidas predeterminadas; en caso contrario, el nombre se usa para obtener una preferencia compartida **privada** con el nombre especificado.

# <a name="ios"></a>[iOS](#tab/ios)

[NSUserDefaults](https://docs.microsoft.com/xamarin/ios/app-fundamentals/user-defaults) se usa para almacenar valores en dispositivos iOS. Si no se especifica ningún `sharedName`, se usa `StandardUserDefaults`; en caso contrario, el nombre se usa para crear un `NSUserDefaults` con el nombre especificado que se usa para el `NSUserDefaultsType.SuiteName`.

# <a name="uwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) se usa para almacenar los valores en el dispositivo. Si no se especifica ningún `sharedName`, se usa `LocalSettings`; en caso contrario, el nombre se usa para crear un contenedor dentro de `LocalSettings`.

`LocalSettings` también tiene la siguiente restricción de que el nombre de cada valor puede tener una longitud máxima de 255 caracteres. Cada valor puede tener un tamaño máximo de 8 KB y cada valor compuesto puede tener un tamaño máximo de 64 KB.

--------------

## <a name="persistence"></a>Persistencia

Al desinstalar la aplicación se quitarán todas las _Preferencias_. Hay una excepción, para las aplicaciones que se destinan y se ejecutan en Android 6.0 (nivel de API 23) o versiones posteriores en las que se usa [__Copia de seguridad automática__](https://developer.android.com/guide/topics/data/autobackup). Esta característica está activada de forma predeterminada y conserva los datos de aplicación, incluidas las __preferencias compartidas__, que son las que usa la API **Preferences**. Se puede deshabilitar si se sigue la [documentación](https://developer.android.com/guide/topics/data/autobackup) de Google.

## <a name="limitations"></a>Limitaciones

Cuando se almacena una cadena, esta API está pensada para almacenar pequeñas cantidades de texto.  El rendimiento puede ser inferior si se intenta usar para almacenar grandes cantidades de texto.

## <a name="api"></a>API

- [Código fuente de Preferences](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Preferences)
- [Documentación de API para Preferences](xref:Xamarin.Essentials.Preferences)

## <a name="related-video"></a>Vídeo relacionado

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Preferences-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
