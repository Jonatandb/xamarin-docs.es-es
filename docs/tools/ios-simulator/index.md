---
title: Simulador remoto de iOS para Windows
description: El simulador remoto de iOS para Windows le permite probar las aplicaciones en un simulador de iOS que se ejecuta en Windows junto con Visual Studio 2019.
ms.prod: xamarin
ms.assetid: 63c50190-7e54-4140-a30d-1a0e577c47d7
author: lobrien
ms.author: laobri
ms.date: 04/02/2019
ms.openlocfilehash: b962390d5a5a365ada93d1778e3efb65839f41c5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61188860"
---
# <a name="remoted-ios-simulator-for-windows"></a>Simulador remoto de iOS para Windows

El simulador remoto de iOS para Windows le permite probar las aplicaciones en un simulador de iOS que se ejecuta en Windows, junto con Visual Studio 2019 y Visual Studio 2017.

[![simulador remoto de iOS que se ejecuta en Windows](images/hero-sml.png "simulador remoto de iOS que se ejecuta en Windows")](images/hero.png#lightbox)

## <a name="getting-started"></a>Introducción

El simulador remoto de iOS para Windows se instala automáticamente como parte de Xamarin en Visual Studio 2019 y Visual Studio 2017. Para ello, siga estos pasos:

1. [Emparejar Visual 2019 a un host de compilación de Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. En Visual Studio, inicie la depuración de un proyecto de iOS o tvOS. El simulador remoto de iOS para Windows aparecerá automáticamente en el equipo Windows.

Inspecione [este vídeo](deploy.md) para obtener una guía paso a paso.

## <a name="simulator-window"></a>Ventana del simulador

La barra de herramientas en la parte superior de la ventana del simulador contiene un número de botones útiles:

- **Inicio** : simula el botón Inicio de un dispositivo iOS.
- **Bloqueo** : bloquea el simulador (pasar el dedo para desbloquear).
- **Captura de pantalla** : guarda una captura de pantalla del simulador (se almacena en **Pictures\Xamarin\Simulador de iOS\\**).
- [**Configuración** ](#settings) : muestra el teclado, ubicación y otras opciones.
- [**Otras opciones** ](#other-options) : abrirá varias opciones del simulador, como rotación, gestos de agitar y Touch Id.

    [![Ejemplo de mapas en el simulador de iOS](images/maps-app-sml.png "Ejemplo de mapas en el simulador de iOS")](images/maps-app.png#lightbox)

## <a name="settings"></a>Configuración

Al hacer clic en el icono de engranaje de la barra de herramientas, se abre la ventana de **configuración**:

[![Ventana de configuración del simulador de iOS](images/settings-sml.png "Ventana de configuración del simulador de iOS")](images/settings.png#lightbox)

Estas opciones permiten habilitar el teclado de hardware, elegir una ubicación que el dispositivo debe notificar (se admiten ubicaciones estáticas y móviles), habilitar Touch ID y restablecer el contenido y la configuración del simulador.

## <a name="other-options"></a>Otras opciones

El botón de puntos suspensivos de la barra de herramientas muestra otras opciones, como la rotación, gestos de agitar y el reinicio del sistema. Estas mismas opciones pueden verse como una lista haciendo clic con el botón derecho en cualquier lugar de la ventana del simulador:

[![Configuración del simulador de iOS](images/settings-sml.png "Configuración del simulador de iOS")](images/settings.png#lightbox)

## <a name="touchscreen-support"></a>Compatibilidad con pantalla táctil

Los equipos Windows más modernos tienen pantallas táctiles. Gracias a que el simulador remoto de iOS para Windows es compatible con las interacciones táctiles, puede probar la aplicación con los mismos gestos táctiles de reducir, deslizar el dedo y con varios dedos que usa con los dispositivos iOS físicos.

De forma similar, el simulador remoto de iOS para Windows procesa la entrada de lápiz de Windows como entrada de lápiz de Apple.

## <a name="disabling-the-remoted-ios-simulator-for-windows"></a>Deshabilitar el simulador remoto de iOS para Windows

Para deshabilitar el simulador remoto de iOS para Windows, vaya a **Herramientas > Opciones > Xamarin > Configuración de iOS** y desactive la opción **Simulador Remoto en Windows**.

[![Casilla de verificación para usar el simulador](images/options-sml.png "Casilla de verificación para usar el simulador")](images/options.png#lightbox)

Con esta opción deshabilitada, el proceso de depuración abre el simulador de iOS en el host de compilación de Mac conectado.
