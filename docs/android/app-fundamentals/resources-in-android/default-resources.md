---
title: Recursos predeterminados
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/13/2018
ms.openlocfilehash: 7375121375cce3927e495dba33fe140f936ad0bd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73025070"
---
# <a name="default-resources"></a>Recursos predeterminados

Los recursos predeterminados son elementos que no son específicos de un determinado dispositivo o factor de forma y, por lo tanto, son la opción predeterminada del sistema operativo Android si no se pueden encontrar más recursos específicos. Como tal, son el tipo más común de recurso que se va a crear. Están organizados en subdirectorios del directorio de **recursos** según su tipo de recurso:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Archivos de recursos predeterminados](default-resources-images/01-resource-files-vs.png)

En la imagen anterior, el proyecto tiene valores predeterminados para los recursos, diseños y valores que se van a dibujar (archivos XML que contienen valores simples).

A continuación se proporciona una lista completa de los tipos de recursos:

- **animación &ndash; archivos** XML que describen animaciones de propiedades.
   Las animaciones de propiedades se introdujeron en el nivel de API 11 (Android 3,0) y proporcionan la animación de propiedades en un objeto. Las animaciones de propiedades son una forma más flexible y eficaz de describir animaciones en cualquier tipo de objeto.

- **anim** &ndash; archivos XML que describen animaciones de *interpolación* . Las animaciones de interpolación son una serie de instrucciones de animación para realizar transformaciones en el contenido de un objeto de vista, o por ejemplo, la rotación de una imagen o el aumento del tamaño del texto. Las animaciones de interpolación están limitadas solo a objetos de vista.

- **color** &ndash; archivos XML que describen una lista de Estados de los colores. Para comprender las listas de Estados de color, considere la posibilidad de usar un widget de interfaz de usuario, como un botón.
   Puede tener distintos Estados, como presionar o deshabilitar, y el botón puede cambiar de color con cada cambio de estado. La lista se expresa en una lista de Estados.

- los **recursos &ndash;** dibujables son un concepto general para los gráficos que se pueden compilar en la aplicación y a los que se tiene acceso mediante llamadas API o a los que hacen referencia otros recursos XML.
   Algunos ejemplos de drawables son los archivos de mapa de bits (. png,. gif,. jpg), mapas de bits redimensionables especiales conocidos como [nueve revisiones](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), listas de Estados, formas genéricas definidas en XML, etc.

- **diseño** &ndash; archivos XML que describen un diseño de la interfaz de usuario, como una actividad o una fila de una lista.

- **menú** &ndash; archivos XML que describen los menús de la aplicación, como *menús de opciones*, *menús contextuales*y *submenús*. Para obtener un ejemplo de menús, vea la [demostración del menú emergente](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) o el ejemplo de [controles estándar](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) .

- **&ndash; archivos** arbitrarios que se guardan en formato binario y sin formato. Estos archivos se compilan en una aplicación Android en formato binario.

- **valores** &ndash; archivos XML que contienen valores simples. Un archivo XML en el directorio Values no define un solo recurso, sino que puede definir varios recursos. Por ejemplo, un archivo XML puede contener una lista de valores de cadena, mientras que otro archivo XML puede contener una lista de valores de color.

- **xml** &ndash; archivos XML que son similares en función de los archivos de configuración de .net. Se trata de XML arbitrario que la aplicación puede leer en tiempo de ejecución.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

![Archivos de recursos predeterminados](default-resources-images/01-resource-files-xs.png)

En la imagen anterior, el proyecto tiene valores predeterminados para los recursos, diseños y valores que se van a dibujar (archivos XML que contienen valores simples).

A continuación se proporciona una lista completa de los tipos de recursos:

- **animación &ndash; archivos** XML que describen animaciones de propiedades.
   Las animaciones de propiedades se introdujeron en el nivel de API 11 (Android 3,0) y proporcionan la animación de propiedades en un objeto. Las animaciones de propiedades son una forma más flexible y eficaz de describir animaciones en cualquier tipo de objeto.

- **anim** &ndash; archivos XML que describen animaciones de *interpolación* . Las animaciones de interpolación son una serie de instrucciones de animación para realizar transformaciones en el contenido de un objeto de vista, o por ejemplo, la rotación de una imagen o el aumento del tamaño del texto. Las animaciones de interpolación están limitadas solo a objetos de vista.

- **color** &ndash; archivos XML que describen una lista de Estados de los colores. Para comprender las listas de Estados de color, considere la posibilidad de usar un widget de interfaz de usuario, como un botón.
   Puede tener distintos Estados, como presionar o deshabilitar, y el botón puede cambiar de color con cada cambio de estado. La lista se expresa en una lista de Estados.

- **fuente** &ndash; a partir del nivel de API 26, es posible incrustar fuentes como un recurso en una aplicación Android. La biblioteca de soporte técnico 26 recargará las fuentes en el nivel de API 14. La incrustación de fuentes permite a las aplicaciones cargar fuentes personalizadas directamente desde diseños XML sin necesidad de importarlas como recursos antes del uso.

- los recursos **mip** &ndash; que se pueden dibujar son un concepto general para los gráficos que se pueden compilar en la aplicación y a los que se tiene acceso a través de llamadas API o a los que hacen referencia otros recursos XML.
   Algunos ejemplos de drawables son los archivos de mapa de bits (. png,. gif,. jpg), mapas de bits redimensionables especiales conocidos como [nueve revisiones](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), listas de Estados, formas genéricas definidas en XML, etc.

- **diseño** &ndash; archivos XML que describen un diseño de la interfaz de usuario, como una actividad o una fila de una lista.

- **menú** &ndash; archivos XML que describen los menús de la aplicación, como *menús de opciones*, *menús contextuales*y *submenús*. Para obtener un ejemplo de menús, vea la [demostración del menú emergente](https://docs.microsoft.com/samples/xamarin/monodroid-samples/popupmenudemo) o el ejemplo de [controles estándar](https://docs.microsoft.com/samples/xamarin/mobile-samples/standardcontrols/) .

- **&ndash; archivos** arbitrarios que se guardan en formato binario y sin formato. Estos archivos se compilan en una aplicación Android en formato binario.

- **valores** &ndash; archivos XML que contienen valores simples. Un archivo XML en el directorio Values no define un solo recurso, sino que puede definir varios recursos. Por ejemplo, un archivo XML puede contener una lista de valores de cadena, mientras que otro archivo XML puede contener una lista de valores de color.

- **xml** &ndash; archivos XML que son similares en función de los archivos de configuración de .net. Se trata de XML arbitrario que la aplicación puede leer en tiempo de ejecución.

-----
