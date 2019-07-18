---
title: 'Xamarin.Essentials: Conectividad'
description: La clase Connectivity en Xamarin.Essentials permite supervisar los cambios en las condiciones de red del dispositivo, revisar el acceso actual a la red y cómo está conectado actualmente.
ms.assetid: E1B1F152-B1D5-4227-965E-C0AEBF528F49
author: jamesmontemagno
ms.author: jamont
ms.date: 01/08/2019
ms.custom: video
ms.openlocfilehash: c70510f7b47f93c6119532b6a1c06f6c2e9e56ea
ms.sourcegitcommit: afe9d93373d66eb45d82cabefca83b5733969634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855760"
---
# <a name="xamarinessentials-connectivity"></a>Xamarin.Essentials: Conectividad

La clase **Connectivity** permite supervisar los cambios en las condiciones de red del dispositivo, revisar el acceso actual a la red y cómo está conectado actualmente.

## <a name="get-started"></a>Primeros pasos

[!include[](~/essentials/includes/get-started.md)]

Para acceder a la funcionalidad **Connectivity**, se requiere la siguiente configuración específica para la plataforma.

# <a name="androidtabandroid"></a>[Android](#tab/android)

El permiso `AccessNetworkState` es necesario y se debe configurar en el proyecto Android. Se puede agregar de las siguientes maneras:

Abra el archivo **AssemblyInfo.cs** de la carpeta **Propiedades** y agregue lo siguiente:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.AccessNetworkState)]
```

O BIEN, actualice el manifiesto de Android:

Abra el archivo **AndroidManifest.xml** de la carpeta **Propiedades** y agregue lo siguiente dentro del nodo **manifest**.

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

O haga clic con el botón derecho en el proyecto de Android y abra las propiedades del proyecto. En **Manifiesto de Android**, busque el área **Permisos requeridos:** y compruebe el permiso **Access Network State** (Estado de red de acceso). Esto actualizará automáticamente el archivo **AndroidManifest.xml**.

# <a name="iostabios"></a>[iOS](#tab/ios)

No se requiere configuración adicional.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

No se requiere configuración adicional.

-----

## <a name="using-connectivity"></a>Uso de Connectivity

Agregue una referencia a Xamarin.Essentials en su clase:

```csharp
using Xamarin.Essentials;
```

Compruebe el acceso de red actual:

```csharp
var current = Connectivity.NetworkAccess;

if (current == NetworkAccess.Internet)
{
    // Connection to internet is available
}
```

[Acceso a la red](xref:Xamarin.Essentials.NetworkAccess) cae en estas categorías:

* **Internet**: acceso a Internet y local.
* **ConstrainedInternet**: acceso limitado a Internet. Indica la conectividad cautiva del portal, donde se proporciona acceso local a un portal web, pero el acceso a Internet requiere que se proporcionen credenciales específicas a través de un portal.
* **Local**: solo acceso a la red local.
* **None**: sin conectividad disponible.
* **Unknown**: no se puede determinar la conectividad de Internet.

Puede comprobar qué tipo de [perfil de conexión](xref:Xamarin.Essentials.ConnectionProfile) el dispositivo usa de manera activa:

```csharp
var profiles = Connectivity.ConnectionProfiles;
if (profiles.Contains(ConnectionProfile.WiFi))
{
    // Active Wi-Fi connection.
}
```

Cada vez que el perfil de conexión o el acceso a la red cambia, puede recibir un evento cuando se desencadena:

```csharp
public class ConnectivityTest
{
    public ConnectivityTest()
    {
        // Register for connectivity changes, be sure to unsubscribe when finished
        Connectivity.ConnectivityChanged += Connectivity_ConnectivityChanged;
    }

    void Connectivity_ConnectivityChanged(object sender, ConnectivityChangedEventArgs e)
    {
        var access = e.NetworkAccess;
        var profiles = e.ConnectionProfiles;
    }
}
```

## <a name="limitations"></a>Limitaciones

Es importante tener en cuenta que puede que `NetworkAccess` informe `Internet`, pero no hay disponible acceso total a la Web. Debido a cómo funciona la conectividad en cada plataforma, solo puede garantizar que hay disponible una conexión. Por ejemplo, es posible que el dispositivo esté conectado a una red Wi-Fi, pero el enrutador no está conectado a Internet. En esta instancia, puede que se indique que hay Internet, pero no hay disponible ninguna conexión activa.

## <a name="api"></a>API

* [Código fuente de Connectivity](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Connectivity)
* [Documentación de API de Connectivity](xref:Xamarin.Essentials.Connectivity)

## <a name="related-video"></a>Vídeo relacionado

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Connectivity-Essential-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
