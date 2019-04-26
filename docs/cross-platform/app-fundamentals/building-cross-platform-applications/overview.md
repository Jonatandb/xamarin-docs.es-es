---
title: Creación de introducción a las aplicaciones de plataforma
description: Este documento proporciona información general de la creación de aplicaciones multiplataforma. Describe el valor de C#, patrones, como MVC y MVVM y las interfaces de usuario nativas de diseño.
ms.prod: xamarin
ms.assetid: E442EEFB-FA9C-40E9-9668-5A3F915C8400
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 1eb308e0095c29d8ab0d0bdf1f74b807fd2ab97f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61275633"
---
# <a name="building-cross-platform-applications-overview"></a>Creación de introducción a las aplicaciones de plataforma

Esta guía presentan la plataforma Xamarin y cómo diseñar una aplicación multiplataforma para maximizar la reutilización del código y ofrecer una experiencia nativa de alta calidad en todas las principales plataformas móviles: iOS, Android y Windows Phone.

El enfoque usado en este documento es generalmente se aplica a las aplicaciones de productividad y las aplicaciones de juegos, sin embargo, el foco está en la productividad y la utilidad (aplicaciones que no sean juego). Consulte la [Introducción a MonoGame documento](~/graphics-games/monogame/introduction/index.md) o visite [Visual Studio Tools para Unity](https://docs.microsoft.com/visualstudio/cross-platform/visual-studio-tools-for-unity) para obtener instrucciones de desarrollo de juegos multiplataforma.

La frase "escribir-ejecutar una vez, en todas partes" se usa a menudo que ensalce las virtudes de una sola base de código que se ejecuta sin modificaciones en varias plataformas. Mientras tiene la ventaja de reutilización del código, ese enfoque conduce generalmente a las aplicaciones que tienen un conjunto de características mínimo común denominador y una interfaz de usuario de aspecto genérico que no se ajusta bien en cualquiera de las plataformas de destino.

Xamarin no es simplemente una "write-ejecutar una vez, en todas partes" plataforma, como uno de sus fortalezas es la capacidad de implementar las interfaces de usuario nativas específicamente para cada plataforma. Sin embargo, con un diseño concienzudo todavía es posible compartir la mayoría del usuario que no son código de interfaz y obtener lo mejor de ambos mundos: escriba el código de lógica empresarial y de almacenamiento de datos de una sola vez y presentar las interfaces de usuario nativas en cada plataforma. Este documento describe un enfoque de arquitectura general para lograr este objetivo.

Este es un resumen de los puntos clave para crear aplicaciones multiplataforma de Xamarin:

-   **Use C#**  -escribir sus aplicaciones C#. Código existente escrito en C# se puede portar a iOS y Android mediante Xamarin muy fácilmente y obviamente se utiliza en aplicaciones de Windows.
-   **Usar patrones de diseño MVC o MVVM** -desarrollar la interfaz de usuario de la aplicación con el patrón de controlador, vista o modelo. Diseñar su aplicación mediante un enfoque de modelo/vista/controlador o un enfoque de Model, View y ViewModel donde hay una separación clara entre el "modelo" y el resto. Determinar qué partes de la aplicación se utiliza elementos de interfaz de usuario nativa de cada plataforma (iOS, Android, Windows, Mac) y utilícelo como guía para dividir su aplicación en dos componentes: "Core" y "Interfaz de usuario".
-   **Crear interfaces de usuario nativas** -cada aplicación específica del sistema operativo proporciona una capa de interfaz de usuario diferente (implementado en C# herramientas de diseño con la Ayuda de la interfaz de usuario nativa):

1.  En iOS, utilice las APIs UIKit para crear aplicaciones de aspecto nativo y, opcionalmente, utilizando el Diseñador de iOS de Xamarin para crear visualmente la interfaz de usuario.
1.  En Android, use Android.Views para crear aplicaciones de aspecto nativo, que aprovecha el Diseñador de interfaz de usuario de Xamarin.
1.  En Windows va a usar XAML para la capa de presentación, creado en el Diseñador de interfaz de usuario de Visual Studio o Blend.
1.  En Mac, usará los guiones gráficos para la capa de presentación, que se creó en Xcode.

Proyectos de Xamarin.Forms se admiten en todas las plataformas y le permiten que crear interfaces de usuario que se pueden compartir entre plataformas mediante XAML de Xamarin.Forms. 

La cantidad de reutilización del código dependerá en gran medida de cuánto código se mantiene en el núcleo compartido y la cantidad de código es la interfaz de usuario específica. El código principal es todo lo que no interactúa directamente con el usuario, sino que proporciona servicios para las partes de la aplicación que se recopilan y muestran esta información.

Para aumentar la cantidad de reutilización del código, puede adoptar los componentes de multiplataforma que proporcionan servicios comunes en todos estos sistemas, como:

1.   [SQLite-net](https://www.nuget.org/packages/sqlite-net-pcl/) para el almacenamiento local de SQL,
1.   [Xamarin Plugins](https://xamarin.com/plugins) para tener acceso a funcionalidades específicas del dispositivo como cámara, contactos y la ubicación geográfica,
1.   [Paquetes de NuGet](https://nuget.org) que son compatibles con los proyectos de Xamarin como [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/),
1.  Uso de características de .NET framework para las redes, servicios web, E/S y mucho más.


Algunos de estos componentes se implementan en el *Tasky* caso práctico.

 <a name="Separate_Reusable_Code_into_a_Core_Library" />


## <a name="separate-reusable-code-into-a-core-library"></a>Separar código reutilizable en una biblioteca principal

Siguiendo el principio de separación de responsabilidad por la arquitectura de aplicaciones de la creación de capas y, a continuación, mover la funcionalidad básica que es independiente de la plataforma en una biblioteca reutilizable principal, puede maximizar el compartir código entre plataformas, como la siguiente ilustración se muestra:

 ![](overview-images/layers2.png "Siguiendo el principio de separación de responsabilidad por la arquitectura de aplicaciones de la creación de capas y, a continuación, mover la funcionalidad básica que es independiente de la plataforma en una biblioteca reutilizable principal, puede maximizar el compartir código entre plataformas")

 <a name="Case_Studies" />


## <a name="case-studies"></a>Casos prácticos

Hay un caso práctico que acompaña a este documento: *Tasky Pro*. Cada caso práctico trata la implementación de los conceptos descritos en este documento en un ejemplo del mundo real. El código es código abierto y está disponible en [github](https://github.com/xamarin/mobile-samples/).
