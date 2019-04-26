---
title: Descripción del ejemplo
description: En este tema se ofrece un tutorial de la aplicación de ejemplo de Xamarin.Forms que muestra cómo comunicar con los servicios web diferentes. Aunque cada servicio web utiliza una aplicación de ejemplo independiente, todos son funcionalmente similares y comparten clases comunes.
ms.prod: xamarin
ms.assetid: A3FEB262-0D79-42E6-8F8B-A565618C490B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/28/2017
ms.openlocfilehash: bfd7330fa1eee5f80a9043341d9760058d99d48b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61321767"
---
# <a name="understanding-the-sample"></a>Descripción del ejemplo

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)

_En este tema se ofrece un tutorial de la aplicación de ejemplo de Xamarin.Forms que muestra cómo comunicar con los servicios web diferentes. Aunque cada servicio web utiliza una aplicación de ejemplo independiente, todos son funcionalmente similares y comparten clases comunes._

La aplicación de lista de tareas pendientes de ejemplo que se describe a continuación se usa para demostrar cómo obtener acceso a diferentes tipos de back-ends de web service con Xamarin.Forms. Proporciona funcionalidad para:

- Ver una lista de tareas.
- Agregar, editar y eliminar tareas.
- Establece el estado de una tarea a 'listo'.
- Diga a los campos de nombre y las notas de la tarea.

En todos los casos, las tareas se almacenan en un back-end que se accede a través de un servicio web.

Cuando se inicia la aplicación, se muestra una página que enumera las tareas que se recuperan desde el servicio web y permite al usuario crear una nueva tarea. Al hacer clic en una tarea desplaza a la aplicación a una segunda página, donde la tarea puede se puede editar, se guardan, eliminan y habla. A continuación se muestra la aplicación final:

![](walkthrough-images/app-example-1.png "Aplicación de tareas pendientes - la primera página")
![](walkthrough-images/app-example-2.png "aplicación de tareas pendientes - segunda página")

Cada tema de esta guía proporciona un vínculo de descarga a un *diferentes* versión de la aplicación que se muestra un tipo específico de back-end de web service. Descargue el código de ejemplo correspondiente en la página de cada estilo de servicio web.

## <a name="understanding-the-application-anatomy"></a>Descripción de la Anatomía de la aplicación

El proyecto PCL para cada aplicación de ejemplo consta de tres carpetas principales:

|Carpeta|Propósito|
|--- |--- |
|Datos|Contiene las clases e interfaces utilizadas para administrar elementos de datos y comunicarse con el servicio web. Como mínimo, esto incluye la `TodoItemManager` (clase), que se expone a través de una propiedad en el `App` clase para invocar las operaciones del servicio web.|
|Modelos|Contiene las clases de modelo de datos para la aplicación. Como mínimo, esto incluye la `TodoItem` (clase), que modela un único elemento de datos usados por la aplicación. La carpeta puede incluir también las clases adicionales que se usa para modelar los datos de usuario.|
|Vistas|Contiene las páginas de la aplicación. Esto suele estar compuesto por el `TodoListPage` y `TodoItemPage` clases y las clases adicionales que se usa para realizar la autenticación.|

El proyecto PCL para cada aplicación también consta de un número de archivos importantes:

|Archivo|Propósito|
|--- |--- |
|Constants.cs|El `Constants` (clase), que especifica las constantes utilizadas por la aplicación para comunicarse con el servicio web. Estas constantes requieren actualización para tener acceso a su personal de back-end servicio creado en un proveedor.|
|ITextToSpeech.cs|El `ITextToSpeech` interfaz, que especifica que el `Speak` método debe proporcionarse mediante cualquier clase de implementación.|
|Todo.cs|El `App` clase que es responsable de crear instancias tanto la primera página que se mostrará la aplicación en cada plataforma, y el `TodoItemManager` clase que se utiliza para invocar las operaciones del servicio web.|

### <a name="viewing-pages"></a>Visualización de páginas

La mayoría de las aplicaciones de ejemplo contienen al menos dos páginas:

- **TodoListPage** : esta página muestra una lista de `TodoItem` instancias y un icono de marca si la `TodoItem.Done` propiedad es `true`. Al hacer clic en un elemento se desplaza a la `TodoItemPage`. Además, se pueden crear nuevos elementos, haga clic en el *+* símbolos.
- **TodoItemPage** : esta página muestra los detalles para el seleccionado `TodoItem`y permite que se puede editar, guardar, elimina y habla.

Además, algunas aplicaciones de ejemplo contienen páginas adicionales que se usan para administrar el proceso de autenticación de usuario.

### <a name="modeling-the-data"></a>Modelado de datos

Cada aplicación de ejemplo usa el `TodoItem` clase para modelar los datos que se muestran y se envía al servicio web para el almacenamiento. En el ejemplo de código siguiente se muestra la clase `TodoItem`:

```csharp
public class TodoItem
{
    public string ID { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
}
```

El `ID` propiedad se utiliza para identificar de forma única cada `TodoItem` de instancia y cada servicio web sirve para identificar los datos que se va a actualizar o eliminar.

### <a name="invoking-web-service-operations"></a>Invocar las operaciones del servicio Web

Se tiene acceso a las operaciones del servicio Web a través de la `TodoItemManager` clase y una instancia de la clase se pueden acceder mediante el `App.TodoManager` propiedad. La `TodoItemManager` clase proporciona los métodos siguientes para invocar las operaciones del servicio web:

- **GetTasksAsync** : este método se usa para rellenar el `ListView` control en el `TodoListPage` con el `TodoItem` instancias recuperados del servicio web.
- **SaveTaskAsync** : este método se utiliza para crear o actualizar un `TodoItem` instancia del servicio web.
- **DeleteTaskAsync** : este método se usa para eliminar un `TodoItem` instancia del servicio web.

Además, algunas aplicaciones de ejemplo contienen métodos adicionales en el `TodoItemManager` (clase), que se usan para administrar el proceso de autenticación de usuario.

En lugar de invocar las operaciones del servicio web directamente, el `TodoItemManager` métodos invocan métodos en una clase dependiente que se inserta en la `TodoItemManager` constructor. Por ejemplo, una aplicación de ejemplo inserta la `RestService` clase en el `TodoItemManager` constructor para proporcionar la implementación que usa las API de REST para acceder a los datos.

### <a name="translating-text-to-speech"></a>Traducir texto a voz

La mayoría de las aplicaciones de ejemplo contienen la funcionalidad de texto a voz (TTS) para hablar de los valores de la `TodoItem.Name` y `TodoItem.Notes` propiedades. Esto se consigue mediante la `OnSpeakActivated` controlador de eventos en el `TodoItemPage` clase, como se muestra en el ejemplo de código siguiente:

```csharp
void OnSpeakActivated (object sender, EventArgs e)
{
    var todoItem = (TodoItem)BindingContext;
    App.Speech.Speak(todoItem.Name + " " + todoItem.Notes);
}
```

Este método simplemente invoca el `Speak` método que se implementa mediante una plataforma específica `Speech` clase. Cada `Speech` la clase implementa la `ITextToSpeech` interfaz, y el código de inicio específicas de la plataforma crea una instancia de la `Speech` clase que se puede acceder mediante el `App.Speech` propiedad.

## <a name="summary"></a>Resumen

En este tema se proporciona un tutorial sobre la aplicación de ejemplo de Xamarin.Forms que se usa para demostrar cómo comunicarse con servicios web diferentes. Aunque cada servicio web utiliza una aplicación de ejemplo independiente, están basadas en la misma interfaz de usuario y la lógica empresarial como se describió anteriormente, solo el mecanismo de almacenamiento de los datos de web service es diferente.


## <a name="related-links"></a>Vínculos relacionados

- [Versión de ASMX (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoASMX)
- [Versión WCF (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoWCF)
- [Versión REST (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoREST)
- [Versión de Azure (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzure)
