---
title: 'Xamarin.Essentials: Solución de problemas'
description: En este documento se describe cómo solucionar los problemas encontrados al desarrollar con la biblioteca Xamarin.Essentials.
ms.assetid: 2E474FAF-F841-4E3C-B815-F7ABD8EE3361
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.openlocfilehash: a9546923c9a17aade58d830ea2504406ad09d7d7
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870084"
---
# <a name="xamarinessentials-troubleshooting"></a>Xamarin.Essentials: Solución de problemas

## <a name="error-version-conflict-detected-for-xamarinandroidsupportcompat"></a>Error: Se detectó un conflicto de versión para Xamarin.Android.Support.Compat

El error siguiente se puede producir al actualizar paquetes NuGet (o agregar un paquete) nuevo con un proyecto de Xamarin.Forms que usa Xamarin.Essentials:

```error
NU1107: Version conflict detected for Xamarin.Android.Support.Compat. Reference the package directly from the project to resolve this issue. 
 MyApp -> Xamarin.Essentials 1.1.0 -> Xamarin.Android.Support.CustomTabs 28.0.0.1 -> Xamarin.Android.Support.Compat (= 28.0.0.1) 
 MyApp -> Xamarin.Forms 3.1.0.583944 -> Xamarin.Android.Support.v4 25.4.0.2 -> Xamarin.Android.Support.Compat (= 25.4.0.2).
```

El problema es que no coinciden las dependencias de los dos paquetes NuGet. Esto se puede resolver agregando de forma manual una versión específica de la dependencia (en este caso **Xamarin.Android.Support.Compat**) que puede admitir los dos.

Para ello, agregue de forma manual el paquete NuGet que es el origen del conflicto y use la lista **Versión** para seleccionar una versión específica. Actualmente, la versión 28.0.0.1 del paquete NuGet Xamarin.Android.Support.Compat & Xamarin.Android.Support.Core.Util resolverá este error.

Consulte [esta entrada de blog](https://redth.codes/how-to-fix-the-dreaded-version-conflict-nuget-error-in-your-xamarin-android-projects/) para obtener más información y ver un vídeo sobre cómo resolver el problema.

Si surge algún problema o encuentra un error, notifíquelo en el [repositorio de Xamarin.Essentials de GitHub](https://github.com/xamarin/Essentials).
