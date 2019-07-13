---
title: Instalación de Xamarin en Visual Studio de 2019
description: Este documento describe cómo instalar Xamarin en Visual Studio de 2019. Se explican los requisitos, el proceso de instalación y la comprobación de la instalación.
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
author: conceptdev
ms.author: crdun
ms.date: 08/28/2018
ms.openlocfilehash: 92ddbfb48131bdaf8ba12cef86e09e4c575200e9
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865615"
---
# <a name="installing-xamarin-in-visual-studio-2019"></a>Instalación de Xamarin en Visual Studio de 2019

<a name="requirements" />

Compruebe los [requisitos del sistema](~/cross-platform/get-started/requirements.md) antes de empezar.

## <a name="installation"></a>Instalación

[!include[](~/cross-platform/includes/install-xamarin-windows.md)]

En Visual Studio 2019, compruebe que Xamarin está instalado, haga clic en el **ayuda** menú. Si está instalado, debería ver el elemento de menú **Xamarin**, como se muestra en esta captura de pantalla:

![Elemento de menú de Xamarin en el menú Ayuda](windows-images/12-xamarin-menu-item.png "Elemento de menú de Xamarin en el menú Ayuda")

Puede hacer clic en **Ayuda > Acerca de Microsoft Visual Studio** y desplazarse por la lista de productos instalados para ver si Xamarin está instalado:

![Pantalla de productos instalados de Visual Studio de 2019](windows-images/13-xamarin-is-installed.png "pantalla de productos instalados de Visual Studio de 2019")

Para obtener más información sobre cómo encontrar la información de la versión, consulte [Where can I find my version information and logs?](~/cross-platform/troubleshooting/questions/version-logs.md) (¿Dónde puedo encontrar los registros y la información de la versión?).

## <a name="next-steps"></a>Pasos siguientes

Instalación de Xamarin en Visual Studio de 2019 permite empezar a escribir código para sus aplicaciones, pero requieren configuración adicional para crear e implementar aplicaciones en el simulador, el emulador y el dispositivo. Consulte las siguientes guías para completar la instalación y empezar a compilar aplicaciones multiplataforma.

### <a name="ios"></a>iOS

Para obtener información detallada, consulte la guía de [instalación de Xamarin.iOS en Windows](~/ios/get-started/installation/windows/index.md). 

1. [Instalación de Visual Studio para Mac](https://docs.microsoft.com/visualstudio/mac/installation)
2. [Conexión de Visual Studio con el host de compilación del equipo Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md)
3. [Configuración del desarrollador de iOS](~/ios/get-started/installation/device-provisioning/index.md): necesaria para ejecutar la aplicación en el dispositivo
4. [Simulador remoto de iOS](~/tools/ios-simulator/index.md)
5. [Introducción a Xamarin.iOS para Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

### <a name="android"></a>Android

Para obtener información detallada, consulte la guía de [instalación de Xamarin.Android en Windows](~/android/get-started/installation/windows.md).

1. [Configuración de Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Uso del administrador de Android SDK de Xamarin](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Emulador de Android SDK](~/android/get-started/installation/android-emulator/index.md)
4. [Configurar el dispositivo para el desarrollo](~/android/get-started/installation/set-up-device-for-development.md)
