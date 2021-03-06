---
title: Creación de interfaces de usuario con el diseñador de iOS
description: En este documento se describe cómo usar el Xamarin Designer para iOS para compilar la interfaz de usuario de una aplicación con guiones gráficos y archivos. Xib. Proporciona vínculos a documentos que describen la disponibilidad de la herramienta, su funcionalidad básica, los controles que se deben diseñar y proporcionan tutoriales de su uso.
ms.prod: xamarin
ms.assetid: E35EFB69-EBBA-40E3-ADBE-CB8016F17127
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 05/31/2018
ms.openlocfilehash: 157e16da2c524029c29e767cd6b3e5eb550a2389
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021762"
---
# <a name="building-user-interfaces-with-the-ios-designer"></a>Creación de interfaces de usuario con el diseñador de iOS

_El Xamarin Designer para iOS es un diseñador visual para los formatos de guion gráfico y Interface Builder de iOS que está totalmente integrado con Visual Studio para Mac y Visual Studio. IOS Designer mantiene la compatibilidad total con los formatos Storyboard y. Xib, de modo que los archivos se pueden editar en Visual Studio para Mac o Visual Studio además de los Interface Builder de Xcode. Además, el Xamarin Designer para iOS admite características avanzadas, como los controles personalizados que se representan en tiempo de diseño en el editor._

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

[![iOS Designer en Visual Studio para Mac](images/designer-vsmac-sml.png "El diseñador de iOS")](images/designer-vsmac.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![iOS Designer en Visual Studio](images/designer-vs.png "El diseñador de iOS")](images/designer-vs.png#lightbox)

-----

## <a name="availability"></a>Disponibilidad

El Xamarin Designer para iOS está disponible en Visual Studio para Mac y en Visual Studio 2017 en Windows.

En estas guías se supone que está familiarizado con el contenido que se incluye en las [guías de introducción de Xamarin. iOS](~/ios/get-started/index.md).

## <a name="ios-designer-basicsintroductionmd"></a>[Conceptos básicos de iOS Designer](introduction.md)

En esta guía se tratan las características de Xamarin iOS Designer. Trata los aspectos básicos del diseñador, que muestran cómo usar el diseñador para diseñar controles visualmente y cómo editar propiedades.

## <a name="designable-controls-overviewios-designable-controls-overviewmd"></a>[Información general sobre los controles que se van a diseñar](ios-designable-controls-overview.md)

Esta guía se centra en los controles personalizados, en cómo se crean y en qué requisitos deben cumplirse para que se representen en la superficie de diseño. Además, muestra cómo depurar problemas comunes que pueden producirse al utilizar controles que se pueden diseñar.

## <a name="walkthrough---using-custom-controls-with-ios-designerios-designable-controls-walkthroughmd"></a>[Tutorial: uso de controles personalizados con el diseñador de iOS](ios-designable-controls-walkthrough.md)

En este artículo se proporciona un tutorial paso a paso que muestra cómo crear un control personalizado y usarlo en el diseñador de iOS. Muestra cómo hacer que un control esté disponible en el cuadro de herramientas del diseñador para que se pueda arrastrar o colocar en una vista. Además, muestra cómo implementar un control para que se represente correctamente en tiempo de diseño y tiempo de ejecución, así como para crear propiedades que se pueden establecer en tiempo de diseño.

## <a name="auto-layout-with-the-xamarin-ios-designerdesigner-auto-layoutmd"></a>[Diseño automático con Xamarin iOS Designer](designer-auto-layout.md)

En esta guía se presenta el diseño automático de iOS y el nuevo flujo de trabajo de restricciones disponible en el diseñador de iOS.
