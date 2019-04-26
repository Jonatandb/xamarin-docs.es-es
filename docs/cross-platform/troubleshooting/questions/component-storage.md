---
title: ¿Dónde se almacenan los componentes en mi equipo?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5EBB49EE-39E5-428B-866F-9FC1BB215B31
author: asb3993
ms.author: amburns
ms.date: 05/08/2018
ms.openlocfilehash: 4152c8ef7eeba3748d9244e27e48f3f9a2c0019b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61359110"
---
# <a name="where-are-the-components-stored-on-my-machine"></a>¿Dónde se almacenan los componentes en mi equipo?

Siempre que se instala un componente de Xamarin en un proyecto de aplicación, obtiene lo coloca en dos lugares:

1. En una carpeta de componentes en el nivel raíz de la carpeta de soluciones. Si quita el componente de todos los proyectos de la solución, se quitarán de esta carpeta también.

2. También se almacena una copia en las siguientes ubicaciones:
    - Windows: `%LocalAppData%\Xamarin\Cache\Components`
    - Mac: `~/Library/Caches/Xamarin/Components`

Así que para quitar completamente un componente del sistema, puede eliminarlo de los proyectos y soluciones y de la carpeta de caché anterior.
