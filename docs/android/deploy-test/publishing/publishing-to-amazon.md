---
title: Publicación en Amazon Appstore
ms.prod: xamarin
ms.assetid: A3E9EAC7-2968-8891-CDF2-B73FC0013EC9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 288ff00b35c369581e50b8e8777f85b7b6119590
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2020
ms.locfileid: "73021307"
---
# <a name="publishing-to-the-amazon-app-store"></a>Publicación en Amazon Appstore

Amazon Mobile App Distribution Program permite a los desarrolladores de aplicaciones móviles publicar sus aplicaciones en Amazon. Esta sección describe brevemente la Tienda Apps de Amazon para Android. 

[![Pantalla de la tienda de aplicaciones de Amazon](publishing-to-amazon-images/amazon-app-store.png)](publishing-to-amazon-images/amazon-app-store.png#lightbox)

Amazon no limita el tamaño de los APK. En cambio, si un APK supera los 30 MB, usará FTP para la distribución en lugar de Amazon Mobile App Distribution Portal.

## <a name="submitting-apps-binary-info"></a>Envío de aplicaciones: información binaria

Enviar una aplicación a la Tienda Apps de Amazon es un proceso similar al de enviar una aplicación a Google Play. Las aplicaciones distribuidas mediante Amazon requieren los siguientes recursos: 

- **Icono**: es un archivo .png de 114 x 114 con un fondo transparente. Es obligatorio.
- **Miniatura**: es una versión más grande del icono anterior. Es de 512 x 512 píxeles con un fondo transparente. Este icono también es obligatorio.
- **Capturas de pantalla**: Amazon requiere un mínimo de tres y un máximo de 10 capturas de pantalla. Las capturas de pantalla deben ser de 1024 píxeles de ancho x 600 píxeles de alto u 800 píxeles de ancho x 480 píxeles de alto. Se admiten los formatos .png y .jpg.
- **Imagen promocional**: para que una aplicación aparezca destacada en ubicaciones promocionales (como la página principal), se puede enviar de forma opcional una imagen promocional. Debe ser un archivo .png o .jpg de 1024 píxeles de ancho x 500 píxeles de alto, con orientación horizontal. No puede tener ninguna animación.
- Se pueden proporcionar actualizaciones a cinco vídeos.

## <a name="approval-process"></a>Proceso de aprobación

Una vez que se ha enviado una aplicación, pasa por un proceso de aprobación.
Amazon revisará la aplicación para asegurarse de que funciona como se detalla en la descripción del producto, no pone los datos de los clientes en riesgo y no afectará al funcionamiento del dispositivo. Una vez completado el proceso de aprobación, Amazon enviará una notificación y distribuirá la aplicación.
