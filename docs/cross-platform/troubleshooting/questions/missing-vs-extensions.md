---
title: Faltan extensiones de Visual Studio después de la instalación
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 066d36a3-e553-48d6-8769-c972274d7641
author: davidortinau
ms.author: daortin
ms.date: 03/20/2017
ms.openlocfilehash: 976d0882c5875c1d3e1c8f0ea1732de08df8e07f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996180"
---
# <a name="missing-visual-studio-extensions-after-installation"></a>Faltan extensiones de Visual Studio después de la instalación

## <a name="error-message-this-project-is-incompatible-with-the-current-edition-of-visual-studio"></a>Mensaje de error: este proyecto no es compatible con la edición actual de Visual Studio

Asegúrese de que está instalado Visual Studio 2017 (Community, Professional o Enterprise) o una versión más reciente.

Vea también los [requisitos de Windows](~/cross-platform/get-started/requirements.md#windows-requirements).

## <a name="possible-fix-1-change-the-installation-to-make-sure-the-visual-studio-extensions-are-installed"></a>Posible corrección 1: cambiar la instalación para asegurarse de que las extensiones de Visual Studio están instaladas

En determinadas situaciones, el instalador de Xamarin podría desactivar automáticamente las opciones de instalación de las extensiones de Visual Studio. Si esta es la causa del problema, instale las extensiones de Visual Studio que faltan mediante el comando **Cambiar** del instalador. Por ejemplo, para instalar las extensiones de Visual Studio 2013:

1. Abra el Panel de control **Programas y características** de Windows.

2. Haga clic con el botón derecho en la entrada **Xamarin** y seleccione **Cambiar**.

3. Haga clic en **Siguiente** y, después, en **Cambiar**.

4. Asegúrese de que la opción **Xamarin for Visual Studio 2013** esté establecida en Install:

    ![Habilitar Xamarin para la opción de instalación de Visual Studio 2013](missing-vs-extensions-images/installer.png)

5. Continúe con el resto del Asistente para la instalación.

## <a name="possible-fix-2-ask-visual-studio-to-set-up-the-extensions-again"></a>Corrección posible 2: solicitar a Visual Studio que vuelva a configurar las extensiones

1. Compruebe si las extensiones de Xamarin se han copiado en la carpeta de extensiones de Visual Studio:

    `C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\Extensions\Xamarin\Xamarin\3.1.228.0`

    Si las extensiones están instaladas correctamente (para la versión 3.1.228), habrá 60 elementos en la carpeta:

    ![Lista del contenido de la carpeta Xamarin\3.1.228.0 en el explorador](missing-vs-extensions-images/folder.png)

2. Después de confirmar que el contenido de la carpeta es correcto, indíquele a Visual Studio que intente configurar las extensiones de nuevo:

    `"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv.exe" /setup`

## <a name="possible-fix-3-try-a-fresh-reinstall-of-xamarin"></a>Corrección posible 3: intentar instalar Xamarin de nuevo

1. Desde el Panel de control de Windows, desinstale cualquiera de los siguientes elementos que aparezcan:

    * Xamarin

    * Xamarin para Windows

    * Xamarin.Android

    * Xamarin.iOS

    * Xamarin para Visual Studio

2. En el explorador, elimine los archivos restantes de las carpetas de extensión de Visual Studio de Xamarin (todas las versiones, incluidos los archivos de **programa** y **los archivos de programa (x86)**):

    `C:\Program Files*\Microsoft Visual Studio 1*.0\Common7\IDE\Extensions\Xamarin`

3. Compruebe también si el directorio "VirtualStore" contiene copias "superpuestas" de alguno de los directorios de extensiones:

    `%LOCALAPPDATA%\VirtualStore`

4. Abra el Editor del Registro (regedit).

5. Busque esta clave:

    _HKEY \_ local \_ MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\SharedDlls_

6. Busque y elimine las entradas que coincidan con este patrón:

    _C:\Archivos de programa \* \Microsoft Visual Studio 1 \* . 0 \ Common7\IDE\Extensions\Xamarin_

7. Busque esta clave:

    `HKEY\_CURRENT\_USER\Software\Microsoft\VisualStudio\1\*.0\ExtensionManager\PendingDeletions`

8. Elimine las entradas que parezca que puedan estar relacionadas con Xamarin. Por ejemplo, esta es la que solía producir problemas en versiones anteriores de Xamarin:

    _Mono.VisualStudio.Shell,1.0_

9. Reinicie.

10. Vuelva a instalar la versión estable actual de Xamarin desde [VisualStudio.com](https://visualstudio.com/xamarin).

## <a name="possible-fix-4-repair-visual-studio-installation"></a>Corrección posible 4: reparar la instalación de Visual Studio

1. Abra el Panel de control **Programas y características** de Windows.

2. Haga clic con el botón derecho en la entrada pertinente de Microsoft Visual Studio y seleccione **Cambiar**.

3. Haga clic en el botón **Reparar** del cuadro de diálogo de Visual Studio que se abre.
