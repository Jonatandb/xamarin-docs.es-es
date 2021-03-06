---
title: ¿Cómo puedo copiar archivos de salida de IPA en la carpeta de entrega de TFS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: B0F1E09E-7315-45BA-B7FF-44D2063EE19C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 4493b1a0d06e2f44ee9a11a250395f058baa0548
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031021"
---
# <a name="how-can-i-copy-ipa-output-files-to-the-tfs-drop-folder"></a>¿Cómo puedo copiar archivos de salida de IPA en la carpeta de entrega de TFS?

Abra el archivo `.csproj` del proyecto de aplicación de iOS en un editor de texto y, a continuación, agregue las siguientes líneas al final (inmediatamente antes de la etiqueta de cierre de `</Project>`):

```xml
<PropertyGroup>
    <CreateIpaDependsOn>
        $(CreateIpaDependsOn);
        CopyIpa
    </CreateIpaDependsOn>
</PropertyGroup>

<Target Name="CopyIpa"
    Condition="'$(OutputType)' == 'Exe'
        And '$(ComputedPlatform)' == 'iPhone'
        And '$(BuildIpa)' == 'true'
        And '$(TF_BUILD)' == 'true'">
    <Copy
        SourceFiles="$(OutputPath)$(IpaPackageName)"
        DestinationFolder="$(TF_BUILD_BINARIESDIRECTORY)"
    />
</Target>
```

## <a name="notes"></a>Notas

- Esta es la misma técnica general que se describe en ¿puedo [cambiar la ruta de acceso de salida del archivo IPA?](~/ios/troubleshooting/questions/ipa-output-path.md). Los dos puntos importantes son establecer `$(TF_BUILD_BINARIESDIRECTORY)` como la carpeta de destino y agregar una condición adicional para que `CopyIpa` solo se ejecute para las compilaciones de TFS.

- Para obtener una descripción de `TF_BUILD_BINARIESDIRECTORY` consulte [variables de compilación predefinidas](https://docs.microsoft.com/azure/devops/pipelines/build/variables).

## <a name="additional-references"></a>Referencias adicionales

- [Documentación sobre la instalación de TFS para su uso con Xamarin](https://docs.microsoft.com/azure/devops/repos/tfvc/overview)
- [Tarea de compilación de Azure DevOps: Xamarin. Android](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-android)
- [Tarea de compilación de Azure DevOps: Xamarin. iOS](https://docs.microsoft.com/azure/devops/pipelines/tasks/build/xamarin-ios)

### <a name="next-steps"></a>Pasos siguientes

En este documento se describe el comportamiento actual de Xamarin 3.11.666 para Visual Studio y Xamarin. iOS 8.10.3 en el host de compilación de Mac. Para obtener más ayuda, para ponerse en contacto con nosotros o si este problema permanece incluso después de usar la información anterior, consulte [¿Qué opciones de soporte técnico están disponibles para Xamarin?](~/cross-platform/troubleshooting/support-options.md) para obtener información sobre las opciones de contacto, sugerencias y cómo archivar un nuevo error si es necesario .
