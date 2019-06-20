---
title: Introducción a la distribución de aplicaciones Xamarin.iOS
description: Este documento contiene información general sobre las técnicas de distribución que están disponibles para las aplicaciones de Xamarin.iOS y actúa como un punto de partida a documentos más detallados sobre el tema.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 1bd61474daedd8bd330c0370948d6dee6f8ea50e
ms.sourcegitcommit: 215b507b2e5a44bb023abc2c804c824b1a6190d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67194954"
---
# <a name="xamarinios-app-distribution-overview"></a>Introducción a la distribución de aplicaciones Xamarin.iOS

_Este documento contiene información general sobre las técnicas de distribución que están disponibles para las aplicaciones Xamarin.iOS y actúa como un punto de partida a documentos más detallados sobre el tema._

Una vez que se ha desarrollado una aplicación de Xamarin.iOS, el siguiente paso del ciclo de vida de desarrollo de software es distribuirla a los usuarios, como se muestra en la sección destacada del siguiente diagrama:


[![](images/publishingdiagram.png "Una vez que se ha desarrollado la aplicación iOS, el siguiente paso consiste en distribuirla a los usuarios, tal y como se muestra en la sección resaltada de este diagrama.")](images/publishingdiagram.png#lightbox)


Apple proporciona los siguientes métodos para distribuir una aplicación de iOS, que son compatibles con Xamarin.iOS:

1. [**La App Store**](#App_Store_Distribution)
2. [**Interna (Enterprise)**](#In-House_Distribution)
2. [**Ad Hoc**](#Ad_Hoc_Distribution)

Todos estos escenarios requieren que las aplicaciones se aprovisionen mediante el correspondiente *perfil de aprovisionamiento*. Los perfiles de aprovisionamiento son archivos que contienen información de firma de código, así como la identidad de la aplicación y el mecanismo de distribución previsto. También contienen información sobre en qué dispositivos se puede implementar la aplicación para la distribución que no se realice a través del App Store.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>Distribución a través del App Store

> [!IMPORTANT]
> Apple [ha comunicado](https://developer.apple.com/ios/submit/) que, a partir de marzo de 2019, las aplicaciones y actualizaciones que se envíen al App Store deberán haberse compilado con el SDK de iOS 12.1 o posterior, incluido en Xcode 10.1 y versiones posteriores.
> Las aplicaciones también deberán admitir los tamaños de pantalla del iPhone XS y el iPad Pro de 12.9".

Se trata de la forma principal mediante la que se distribuyen las aplicaciones de iOS a los consumidores en dispositivos iOS. Todas las aplicaciones que se envían a la App Store requieren la aprobación de Apple.

Las aplicaciones se envían a la App Store a través de un portal llamado *iTunes Connect*. La guía [Configurar la aplicación en iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md) proporciona más información sobre cómo configurar y usar este portal para preparar una aplicación de Xamarin.iOS para su publicación en el App Store.

Es importante tener en cuenta que solo los desarrolladores que pertenecen al **Programa para desarrolladores de Apple** tienen acceso a iTunes Connect. Los miembros del **Programa para desarrolladores empresariales de Apple** no tienen acceso.

Para obtener más información, visite la guía [Distribución a través del App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md).

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>Distribución interna

A veces denominada *Distribución empresarial*, la distribución interna permite a los miembros del **Programa para desarrolladores empresariales de Apple** distribuir aplicaciones internamente a otros miembros de la misma organización. La distribución interna tiene las ventajas de no requerir una revisión de la App Store y no tener ningún límite en el número de dispositivos en los que se puede instalar una aplicación. Sin embargo, es importante tener en cuenta que los miembros del **Programa para desarrolladores empresariales de Apple** **no** tienen acceso a iTunes Connect y, por lo tanto, el licenciatario es responsable de distribuir la aplicación.

Para obtener más información sobre cómo configurar y cómo distribuir una aplicación de forma interna, consulte la [Guía de distribución interna](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Distribución ad hoc

Los usuarios pueden probar las aplicaciones de Xamarin.iOS a través de la distribución ad hoc, que está disponible tanto en el **Programa para desarrolladores de Apple** y el **Programa para desarrolladores empresariales de Apple**, y permite realizar la prueba en un máximo de 100 dispositivos iOS. El mejor caso de uso para la distribución ad hoc es la distribución dentro de una empresa cuando iTunes Connect no es una opción.

Para obtener más información sobre cómo configurar y cómo distribuir una aplicación de forma interna, consulte la [Guía de distribución ad hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="summary"></a>Resumen

Este artículo le da una breve introducción a los mecanismos de distribución que están disponibles para las aplicaciones de Xamarin.iOS. Introduce la iTunes App Store, la implementación interna y ad hoc, y proporciona vínculos a información más detallada.

## <a name="related-links"></a>Vínculos relacionados

- [Distribución a través del App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Configuración de una aplicación en iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Publicación en App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Distribución interna](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Distribución ad hoc](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Archivo iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Compatibilidad con IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Solución de problemas](~/ios/deploy-test/troubleshooting.md)
