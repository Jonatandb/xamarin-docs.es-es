---
title: Convertidores de unidades de Xamarin.Essentials
description: La clase UnitConverters de Xamarin.Essentials proporciona varios convertidores de unidades para ayudar a los desarrolladores que usan Xamarin.Essentials.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 332762a83796fef24278c6685a29f7d31a10dadc
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801860"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials: convertidores de unidades

La clase **UnitConverters** proporciona varios convertidores de unidades para ayudar a los desarrolladores que usan Xamarin.Essentials.

## <a name="get-started"></a>Primeros pasos

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Uso de convertidores de unidades

Agregue una referencia a Xamarin.Essentials en la clase:

```csharp
using Xamarin.Essentials;
```

Todos los convertidores de unidades están disponibles mediante el uso de la clase estática `UnitConverters` de Xamarin.Essentials. Por ejemplo, puede convertir fácilmente grados Fahrenheit a Celsius.

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

Esta es una lista de las conversiones disponibles:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToMeters
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- DegreesPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToDegreesPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToPascals
- HectopascalsToKilopascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToKilometers
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [Código fuente de los convertidores de unidades](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Documentación sobre la API de los convertidores de unidades](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>Vídeo relacionado

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
