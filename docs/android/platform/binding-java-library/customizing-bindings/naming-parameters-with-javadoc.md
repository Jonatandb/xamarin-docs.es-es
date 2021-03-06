---
title: Asignación de nombres de parámetros con Javadoc
description: En este artículo se explica cómo recuperar nombres de parámetros en un proyecto de enlace de Java mediante el uso del Javadoc generado a partir del proyecto de Java.
ms.prod: xamarin
ms.assetid: 59E8EF16-1322-486A-BB16-353804B77356
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/20/2017
ms.openlocfilehash: bcfc5778ed4e5486d188f4eefbd32811792b44e5
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2020
ms.locfileid: "85852977"
---
# <a name="naming-parameters-with-javadoc"></a>Asignación de nombres de parámetros con Javadoc

> [!IMPORTANT]
> Estamos investigando el uso de enlaces personalizados en la plataforma Xamarin. Realice [**esta encuesta**](https://www.surveymonkey.com/r/KKBHNLT) para informar de esfuerzos de desarrollo futuros.

_En este artículo se explica cómo recuperar nombres de parámetros en un proyecto de enlace de Java mediante el uso del Javadoc generado a partir del proyecto de Java._

## <a name="overview"></a>Información general

Al enlazar una biblioteca de Java existente, se pierden algunos metadatos sobre la API enlazada. En concreto, los nombres de los parámetros para los métodos. Los nombres de parámetros se muestran como `p0`, `p1`, etc. Esto se debe a que los archivos `.class` de Java no conservan los nombres de parámetro que se usaron en el código fuente de Java. 

Un proyecto de enlace de Java de Xamarin.Android puede proporcionar nombres de parámetro si tiene acceso al código HTML de Javadoc de la biblioteca original. 

## <a name="integrating-javadoc-html-into-a-java-binding-project"></a>Integración del código HTML de Javadoc en un proyecto de enlace de Java

La integración del código HTML de Javadoc en un proyecto de enlace de Java es un proceso manual que consta de los pasos siguientes: 

1. Descargar Javadoc para la biblioteca
2. Edite el archivo `.csproj` y agregue una propiedad `<JavaDocPaths>`:
3. Limpiar y recompilar el proyecto

Una vez hecho esto, los nombres de parámetro originales de Java deberían estar presentes en las API enlazadas mediante un proyecto de enlace de Java. 

> [!NOTE]
> Existe una gran varianza en la salida de JavaDoc. La cadena de herramientas del enlace de .JAR no admite todas las permutaciones posibles y, por lo tanto, puede que algunos parámetros no tengan un nombre correcto.

## <a name="summary"></a>Resumen

En este artículo se explica cómo usar Javadoc en un proyecto de enlace de Java para proporcionar nombres de parámetro de significado para las API enlazadas. 
