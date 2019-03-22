---
title: Análisis detallado de inicio rápido de Xamarin.Forms
description: En este artículo se examinan los aspectos fundamentales del desarrollo de aplicaciones con Xamarin.Forms. Los temas que se han tratado incluyen la anatomía de una aplicación de Xamarin.Forms, la arquitectura y los elementos fundamentales, y la interfaz de usuario.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 7B2340A1-6883-41D8-860C-0BB6C4E0C316
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/27/2018
ms.openlocfilehash: 8674ef47867acf3bca4d05fd6628a58e2f9ad90e
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329369"
---
# <a name="xamarinforms-quickstart-deep-dive"></a>Análisis detallado de inicio rápido de Xamarin.Forms

En el [inicio rápido de Xamarin.Forms](~/get-started/index.yml), se creó la aplicación de notas. En este artículo se revisa lo que se ha compilado para comprender los aspectos fundamentales del funcionamiento de las aplicaciones de Xamarin.Forms.

::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Introducción a Visual Studio

Visual Studio organiza el código en *soluciones* y *proyectos*. Una solución es un contenedor que puede incluir uno o varios proyectos. Un proyecto puede ser una aplicación, una biblioteca auxiliar o una aplicación de prueba, entre otros. La aplicación de Notes consta de una solución que contiene cuatro proyectos, como se muestra en la captura de pantalla siguiente:

![](deepdive-images/vs/solution.png "Explorador de soluciones de Visual Studio")

Los proyectos son:

- Notas: este proyecto es el proyecto de biblioteca .NET Standard que incluye todo el código compartido y la interfaz de usuario compartida.
- Notes.Android: este proyecto contiene código específico de Android y es el punto de entrada para la aplicación Android.
- Notes.iOS: este proyecto contiene código específico de iOS y es el punto de entrada para la aplicación de iOS.
- Notes.UWP: este proyecto incluye el código específico de plataforma Universal de Windows (UWP) y es el punto de entrada para la aplicación de UWP.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomía de una aplicación de Xamarin.Forms

Captura de pantalla siguiente muestra el contenido del proyecto de biblioteca estándar de .NET de notas en Visual Studio:

![](deepdive-images/vs/net-standard-project.png "Contenido del proyecto de .NET Standard de Phoneword")

El proyecto tiene un nodo **Dependencias** que contiene los nodos **NuGet** y **SDK**:

- **NuGet** &ndash; el Xamarin.Forms y paquetes de NuGet de sqlite-net-pcl que se han agregado al proyecto.
- **SDK**: el metapaquete `NETStandard.Library` que hace referencia al conjunto completo de paquetes NuGet que definen .NET Standard.

::: zone-end
::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Introducción a Visual Studio para Mac

[Visual Studio para Mac](/visualstudio/mac/) sigue la práctica de Visual Studio consistente en organizar el código en *soluciones* y *proyectos*. Una solución es un contenedor que puede incluir uno o varios proyectos. Un proyecto puede ser una aplicación, una biblioteca auxiliar o una aplicación de prueba, entre otros. La aplicación de Notes consta de una solución que contiene tres proyectos, como se muestra en la captura de pantalla siguiente:

![](deepdive-images/vsmac/solution.png "Panel de solución de Visual Studio para Mac")

Los proyectos son:

- Notas: este proyecto es el proyecto de biblioteca .NET Standard que incluye todo el código compartido y la interfaz de usuario compartida.
- Notes.Android: este proyecto contiene código específico de Android y es el punto de entrada para aplicaciones de Android.
- Notes.iOS: este proyecto contiene código específico de iOS y es el punto de entrada para las aplicaciones de iOS.

## <a name="anatomy-of-a-xamarinforms-application"></a>Anatomía de una aplicación de Xamarin.Forms

Captura de pantalla siguiente muestra el contenido del proyecto de biblioteca estándar de .NET de notas en Visual Studio para Mac:

![](deepdive-images/vsmac/net-standard-project.png "Contenido del proyecto de la biblioteca de .NET Standard de Phoneword")

El proyecto tiene un nodo **Dependencias** que contiene los nodos **NuGet** y **SDK**:

- **NuGet** &ndash; el Xamarin.Forms y paquetes de NuGet de sqlite-net-pcl que se han agregado al proyecto.
- **SDK**: el metapaquete `NETStandard.Library` que hace referencia al conjunto completo de paquetes NuGet que definen .NET Standard.

::: zone-end

El proyecto también consta de varios archivos:

- **Data\NoteDatabase.cs** : esta clase contiene código para crear la base de datos, leen datos, escribir datos en él y eliminar datos.
- **Models\Note.cs** : esta clase define un `Note` modelo cuyas instancias almacenan datos sobre todas las notas de la aplicación.
- **App.xaml**: el marcado XAML para la clase `App`, que define un diccionario de recursos para la aplicación.
- **App.xaml.cs**: el código subyacente para la clase `App`, que es el responsable de crear instancias de la primera página que se mostrarán mediante la aplicación en cada plataforma, y para controlar los eventos del ciclo de vida de la aplicación.
- **NotesPage.xaml** – marcado el XAML para el `NotesPage` (clase), que define la interfaz de usuario para la página que aparece cuando se inicia la aplicación.
- **NotesPage.xaml.cs** : el código subyacente para el `NotesPage` (clase), que contiene la lógica de negocios que se ejecuta cuando el usuario interactúa con la página.
- **NoteEntryPage.xaml** – marcado el XAML para el `NoteEntryPage` (clase), que define la interfaz de usuario para la página que aparece cuando el usuario escribe una nota.
- **NoteEntryPage.xaml.cs** : el código subyacente para el `NoteEntryPage` (clase), que contiene la lógica de negocios que se ejecuta cuando el usuario interactúa con la página.

Para obtener más información sobre la anatomía de una aplicación de Xamarin.iOS, consulte [Anatomía de una aplicación de Xamarin.iOS](~/ios/get-started/hello-ios/hello-ios-deepdive.md#anatomy-of-a-xamarinios-application). Para obtener más información sobre la anatomía de una aplicación de Xamarin.Android, consulte [Anatomía de una aplicación de Xamarin.Android](~/android/get-started/hello-android/hello-android-deepdive.md#anatomy).

## <a name="architecture-and-application-fundamentals"></a>Aspectos básicos de arquitectura y la aplicación

Una aplicación de Xamarin.Forms tiene la misma arquitectura que una aplicación multiplataforma tradicional. El código compartido normalmente se coloca en una biblioteca de .NET Standard, y las aplicaciones específicas de la plataforma consumen el código compartido. El siguiente diagrama muestra una visión general de esta relación para la aplicación de notas:

::: zone pivot="windows"

![](deepdive-images/vs/architecture.png "Notas de la arquitectura")

::: zone-end
::: zone pivot="macos"

![](deepdive-images/vsmac/architecture.png "Notas de la arquitectura")

::: zone-end

Para maximizar la reutilización del código de inicio, las aplicaciones de Xamarin.Forms tienen una clase única denominada `App` que es responsable de crear instancias de la primera página que mostrará la aplicación en cada plataforma, como se muestra en el siguiente ejemplo de código:

```csharp
using Xamarin.Forms;
using Xamarin.Forms.Xaml;

[assembly: XamlCompilation(XamlCompilationOptions.Compile)]
namespace Notes
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new NavigationPage(new NotesPage());
        }
        ...
    }
}
```

Este código establece la `MainPage` propiedad de la `App` clase a un [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) instancia cuyo contenido es un `NotesPage` instancia. Además, el atributo [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) se activa en el compilador XAML, de manera que XAML se compile directamente en lenguaje intermedio. Para obtener más información, consulte [Compilación XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="launching-the-application-on-each-platform"></a>Iniciando la aplicación en cada plataforma

### <a name="ios"></a>iOS

Para iniciar la página de Xamarin.Forms inicial en iOS, el proyecto Notes.iOS define la `AppDelegate` clase que hereda de la `FormsApplicationDelegate` clase:

```csharp
namespace Notes.iOS
{
    [Register("AppDelegate")]
    public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
    {
        public override bool FinishedLaunching(UIApplication app, NSDictionary options)
        {
            global::Xamarin.Forms.Forms.Init();
            LoadApplication(new App());
            return base.FinishedLaunching(app, options);
        }
    }
}
```

El reemplazo `FinishedLaunching` inicializa el marco de Xamarin.Forms mediante una llamada al método `Init`. Esto provoca que la implementación específica de iOS de Xamarin.Forms se cargue en la aplicación antes de que se establezca el controlador de vista raíz mediante la llamada al método `LoadApplication`.

### <a name="android"></a>Android

Para iniciar la página inicial de Xamarin.Forms en Android, el proyecto Notes.Android incluye código que crea un `Activity` con el `MainLauncher` atributo con la actividad se hereda de la `FormsAppCompatActivity` clase:

```csharp
namespace Notes.Droid
{
    [Activity(Label = "Notes",
              Icon = "@mipmap/icon",
              Theme = "@style/MainTheme",
              MainLauncher = true,
              ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        protected override void OnCreate(Bundle savedInstanceState)
        {
            TabLayoutResource = Resource.Layout.Tabbar;
            ToolbarResource = Resource.Layout.Toolbar;

            base.OnCreate(savedInstanceState);
            global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
            LoadApplication(new App());
        }
    }
}
```

El reemplazo `OnCreate` inicializa el marco de Xamarin.Forms mediante una llamada al método `Init`. Esto provoca que la implementación específica de Android de Xamarin.Forms se cargue en la aplicación antes de que lo haga la aplicación de Xamarin.Forms.

::: zone pivot="windows"

### <a name="universal-windows-platform"></a>Plataforma universal de Windows

En las aplicaciones de plataforma universal de Windows (UWP), el método `Init` que inicializa el marco de Xamarin.Forms se invoca desde la clase `App`:

```csharp
Xamarin.Forms.Forms.Init (e);

if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
{
  ...
}
```

Esto provoca que la implementación específica de UWP de Xamarin.Forms se cargue en la aplicación. La página de Xamarin.Forms inicial se inicia por la `MainPage` clase:

```csharp
namespace Notes.UWP
{
    public sealed partial class MainPage
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.LoadApplication(new Notes.App());
        }
    }
}
```

La aplicación de Xamarin.Forms se carga con el método `LoadApplication`.

> [!NOTE]
> Aplicaciones de la plataforma de Windows universales pueden ser compilada con Xamarin.Forms, pero solo mediante Visual Studio en Windows.

::: zone-end

## <a name="user-interface"></a>Interfaz de usuario

Hay cuatro grupos principales de control usados para crear la interfaz de usuario de una aplicación de Xamarin.Forms:

1. **Páginas**: las páginas de Xamarin.Forms representan pantallas de aplicaciones móviles multiplataforma. Usa la aplicación de notas de la [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) clase para mostrar pantallas únicas. Para obtener más información sobre las páginas, consulte [Xamarin.Forms Pages](~/xamarin-forms/user-interface/controls/pages.md) (Páginas de Xamarin.Forms).
1. **Vistas**: las vistas de Xamarin.Forms son los controles que se muestran en la interfaz de usuario, como etiquetas, botones y cuadros de entrada de texto. Usa la aplicación finalizada de notas el [ `ListView` ](xref:Xamarin.Forms.ListView), [ `Editor` ](xref:Xamarin.Forms.Editor), y [ `Button` ](xref:Xamarin.Forms.Button) vistas. Para obtener más información sobre las vistas, consulte [Xamarin.Forms Views](~/xamarin-forms/user-interface/controls/views.md) (Vistas de Xamarin.Forms).
1. **Diseños**: los diseños de Xamarin.Forms son contenedores que se usan para crear vistas en estructuras lógicas. Usa la aplicación de notas de la [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) clase para organizar las vistas en una pila vertical y el [ `Grid` ](xref:Xamarin.Forms.Grid) clase para organizar botones horizontalmente. Para obtener más información sobre los diseños, consulte [Xamarin.Forms Layouts](~/xamarin-forms/user-interface/controls/layouts.md) (Diseños de Xamarin.Forms).
1. **Celdas**: las celdas de Xamarin.Forms son elementos especializados que se usan para los elementos de una lista, y describen cómo debe dibujarse cada elemento de una lista. Usa la aplicación de notas de la [ `TextCell` ](xref:Xamarin.Forms.TextCell) para mostrar los dos elementos para cada fila en la lista. Para obtener más información sobre las celdas, consulte [Xamarin.Forms Cells](~/xamarin-forms/user-interface/controls/cells.md) (Celdas de Xamarin.Forms).

En tiempo de ejecución, cada control se asignará a su equivalente nativo, que es lo que se representará.

### <a name="layout"></a>Diseño

Usa la aplicación de notas de la [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) para simplificar el desarrollo de aplicaciones multiplataforma al organizar automáticamente las vistas en la pantalla, independientemente del tamaño de pantalla. Cada elemento secundario se coloca uno detrás del otro, ya sea horizontal o verticalmente, en el orden en el que se ha agregado. La cantidad de espacio que usará la clase `StackLayout` depende de cómo se establezcan las propiedades [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) y [`VerticalOptions`](xref:Xamarin.Forms.View.HorizontalOptions), pero `StackLayout` intentará usar toda la pantalla de forma predeterminada.

El código XAML siguiente muestra un ejemplo del uso de un [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) al diseño del `NoteEntryPage`:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    ...    
    <StackLayout Margin="{StaticResource PageMargin}">
        <Editor Placeholder="Enter your note"
                Text="{Binding Text}"
                HeightRequest="100" />
        <Grid>
            ...
        </Grid>
    </StackLayout>    
</ContentPage>
```

De forma predeterminada el [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) asume una orientación vertical. Sin embargo, se puede cambiar a una orientación horizontal estableciendo el [ `StackLayout.Orientation` ](xref:Xamarin.Forms.StackLayout.Orientation) propiedad a la [ `StackOrientation.Horizontal` ](xref:Xamarin.Forms.StackOrientation.Horizontal) miembro de enumeración.

> [!NOTE]
> Se puede establecer el tamaño de las vistas a través de la `HeightRequest` y `WidthRequest` propiedades.

Para obtener más información sobre la clase [`StackLayout`](xref:Xamarin.Forms.StackLayout), consulte [StackLayout](~/xamarin-forms/user-interface/layouts/stack-layout.md).

### <a name="responding-to-user-interaction"></a>Responder a la interacción del usuario

Un objeto que se ha definido en XAML puede desencadenar un evento que se controla en el archivo de código subyacente. El siguiente ejemplo de código muestra la `OnSaveButtonClicked` método en el código subyacente para el `NoteEntryPage` (clase), que se ejecuta en respuesta a la [ `Clicked` ](xref:Xamarin.Forms.Button.Clicked) evento y activar el *guardar* botón .

```csharp
async void OnSaveButtonClicked(object sender, EventArgs e)
{
    var note = (Note)BindingContext;
    note.Date = DateTime.UtcNow;
    await App.Database.SaveNoteAsync(note);
    await Navigation.PopAsync();
}
```

El `OnSaveButtonClicked` método guarda la nota en la base de datos y vuelve a la página anterior.

> [!NOTE]
> El archivo de código subyacente de una clase XAML puede tener acceso a un objeto que se ha definido en XAML con el nombre asignado a él con el atributo `x:Name`. El valor que se ha asignado a este atributo tiene las mismas reglas que las variables de C#, ya que debe comenzar con una letra o guion bajo y no contener espacios incrustados.

Botón el cableado de la operación de guardar el `OnSaveButtonClicked` método se produce en el marcado XAML para el `NoteEntryPage` clase:

```xaml
<Button Text="Save"
        Clicked="OnSaveButtonClicked" />
```

### <a name="lists"></a>Listas

El [ `ListView` ](xref:Xamarin.Forms.ListView) es responsable de mostrar una colección de elementos verticalmente en una lista. Cada elemento en el `ListView` se incluirá en una sola celda.

El siguiente ejemplo de código muestra la [ `ListView` ](xref:Xamarin.Forms.ListView) desde el `NotesPage`:

```xaml
<ListView x:Name="listView"
          Margin="{StaticResource PageMargin}"
          ItemSelected="OnListViewItemSelected">
    <ListView.ItemTemplate>
        <DataTemplate>
            <TextCell Text="{Binding Text}"
                      Detail="{Binding Date}" />
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

El diseño de cada fila de la [ `ListView` ](xref:Xamarin.Forms.ListView) se define dentro el [ `ListView.ItemTemplate` ](xref:Xamarin.Forms.ItemsView`1.ItemTemplate) elemento y la usa para mostrar las notas que se recuperan mediante la aplicación de enlace de datos. El [ `ListView.ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) propiedad está establecida en el origen de datos en `NotesPage.xaml.cs`:

```csharp
protected override async void OnAppearing()
{
    base.OnAppearing();

    listView.ItemsSource = await App.Database.GetNotesAsync();
}
```    

Este código rellena la [ `ListView` ](xref:Xamarin.Forms.ListView) con las notas que se almacenan en la base de datos.

Cuando se selecciona una fila en la [ `ListView` ](xref:Xamarin.Forms.ListView), [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) desencadena el evento. Un controlador de eventos, llamado `OnListViewItemSelected`, se ejecuta cuando se activa el evento:

```csharp
async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        ...
    }
}
```

El [ `ItemSelected` ](xref:Xamarin.Forms.ListView.ItemSelected) evento puede tener acceso al objeto que se asoció a la celda a través de la [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) propiedad.

Para obtener más información sobre la [ `ListView` ](xref:Xamarin.Forms.ListView) de clases, vea [ListView](~/xamarin-forms/user-interface/listview/index.md).

## <a name="navigation"></a>Navegación

Xamarin.Forms proporciona una serie de experiencias de navegación de páginas diferente, en función del tipo de [`Page`](xref:Xamarin.Forms.Page) que se use. Para [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) navegación instancias puede ser jerárquica, o no modales. Para obtener información sobre la navegación modal, consulte [páginas modales de Xamarin.Forms](~/xamarin-forms/app-fundamentals/navigation/modal.md).

> [!NOTE]
> Las clases [`CarouselPage`](xref:Xamarin.Forms.CarouselPage), [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) y [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) proporcionan experiencias de navegación alternativas. Para obtener más información, consulte [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md) (Navegación).

En la navegación jerárquica, el [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) clase se utiliza para navegar a través de una pila de [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) objetos hacia delante y hacia atrás, según sea necesario. La clase implementa la navegación como una pila de objetos [`Page`](xref:Xamarin.Forms.Page) en la que el último en entrar es el primero en salir (LIFO). Para pasar de una página a otra, una aplicación insertará una nueva página en la pila de navegación, donde se convertirá en la página activa. Para volver a la página anterior, la aplicación mostrará la página actual de la pila de navegación y la nueva página de nivel superior se convertirá en la página activa.

La clase `NavigationPage` también agregará una barra de navegación en la parte superior de la página que muestra un título y un botón **Atrás** adecuado para la plataforma para volver a la página anterior.

La primera página que se agrega a una pila de navegación se conoce como el *raíz* página de la aplicación y en el ejemplo de código siguiente muestra cómo se consigue en la aplicación de notas:

```csharp
public App ()
{
    ...
    MainPage = new NavigationPage (new NotesPage ());
}
```

Todas las instancias [`ContentPage`](xref:Xamarin.Forms.ContentPage) tienen una propiedad [`Navigation`](xref:Xamarin.Forms.NavigableElement.Navigation) que expone métodos para modificar la pila de la página. Solo se deberían invocar estos métodos si la aplicación incluye una [`NavigationPage`](xref:Xamarin.Forms.NavigationPage). Para navegar a la `NoteEntryPage`, es necesario invocar el método [`PushAsync`](xref:Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)) como se muestra en el ejemplo de código siguiente:

```csharp
await Navigation.PushAsync(new NoteEntryPage());
```

Esto hace que el nuevo `NoteEntryPage` objeto se inserte en la pila de navegación, donde se convertirá en la página activa.

La página activa se puede extraer de la pila de navegación. Para ello, pulse el botón *Atrás* del dispositivo, independientemente de si se trata de un botón físico en el dispositivo o de un botón en la pantalla. Para volver mediante programación a la página original, el objeto `NoteEntryPage` debe invocar el método [`PopAsync`](xref:Xamarin.Forms.NavigationPage.PopAsync), como se muestra en el ejemplo de código siguiente:

```csharp
await Navigation.PopAsync();
```

Para obtener más información sobre la navegación jerárquica, consulte [Hierarchical Navigation](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md) (Navegación jerárquica).

## <a name="data-binding"></a>Enlace de datos

El enlace de datos se usa para simplificar la forma en que una aplicación de Xamarin.Forms muestra e interactúa con sus datos. Establece una conexión entre la interfaz de usuario y la aplicación subyacente. La clase [`BindableObject`](xref:Xamarin.Forms.BindableObject) contiene gran parte de la infraestructura para admitir el enlace de datos.

El enlace de datos conecta dos objetos, denominados *origen* y *destino*. El objeto de *origen* proporciona los datos. El objeto de *destino* usa (y, a menudo, muestra) los datos del objeto de origen. Por ejemplo, un [ `Editor` ](xref:Xamarin.Forms.Editor) (*destino* objeto) normalmente enlazará su [ `Text` ](xref:Xamarin.Forms.Editor.Text) propiedad para un público `string` propiedad en un *origen* objeto. En el diagrama siguiente se muestra la relación de enlace:

![](deepdive-images/data-binding.png "Enlace de datos")

El principal beneficio del enlace de datos es que ya no tiene que preocuparse de sincronizar los datos entre las vistas y el origen de datos. Los cambios en el objeto de *origen* se insertan automáticamente en el objeto de *destino* en segundo plano por medio del marco de enlace, mientras que los cambios en el objeto de destino pueden insertarse de manera opcional en el objeto de *origen*.

Establecer datos de enlace es un proceso de dos pasos:

- La propiedad [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) del objeto de *destino* se debe establecer en el de *origen*.
- Es necesario establecer un enlace entre el *destino* y el *origen*. En XAML, esto se consigue mediante la extensión de marcado [`Binding`](xref:Xamarin.Forms.Xaml.BindingExtension).

En la aplicación de notas, el destino de enlace es el [ `Editor` ](xref:Xamarin.Forms.Editor) que muestra una nota, mientras que el `Note` instancia establecida como el [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de `NoteEntryPage` es el enlace código fuente.

El `BindingContext` de la `NoteEntryPage` se establece durante la navegación en páginas, como se muestra en el ejemplo de código siguiente:

```csharp
async void OnNoteAddedClicked(object sender, EventArgs e)
{
    await Navigation.PushAsync(new NoteEntryPage
    {
        BindingContext = new Note()
    });
}

async void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
{
    if (e.SelectedItem != null)
    {
        await Navigation.PushAsync(new NoteEntryPage
        {
            BindingContext = e.SelectedItem as Note
        });
    }
}
```

En el `OnNoteAddedClicked` método, que se ejecuta cuando se agrega una nota nueva a la aplicación, el [ `BindingContext` ](xref:Xamarin.Forms.BindableObject.BindingContext) de `NoteEntryPage` se establece en un nuevo `Note` instancia. En el `OnListViewItemSelected` método, que se ejecuta cuando se selecciona una nota existente en el [ `ListView` ](xref:Xamarin.Forms.ListView), el `BindingContext` de la `NoteEntryPage` está establecido en seleccionado `Note` instancia, que se accede mediante el [ `e.SelectedItem` ](xref:Xamarin.Forms.SelectedItemChangedEventArgs.SelectedItem) propiedad.

> [!IMPORTANT]
> Aunque la propiedad [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) de cada objeto de *destino* se puede establecer de manera individual, no es necesario hacerlo. `BindingContext` es una propiedad especial que heredan todos sus elementos secundarios. Por lo tanto, cuando el `BindingContext` en el [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) está establecido en un `Note` instancia, todos los elementos secundarios de la `ContentPage` tienen el mismo `BindingContext`y se puede enlazar a propiedades públicas de la `Note`objeto.

El [ `Editor` ](xref:Xamarin.Forms.Editor) en `NoteEntryPage` , a continuación, se enlaza a la `Text` propiedad de la `Note` objeto:

```xaml
<Editor Placeholder="Enter your note"
        Text="{Binding Text}"
        ... />
```

Se establece un enlace entre la propiedad [`Editor.Text`](xref:Xamarin.Forms.Editor.Text) y la propiedad `Text` del objeto de *origen*. Los cambios realizados en el `Editor` se propagarán automáticamente a la `Note` objeto. De forma similar, si se realizan cambios en el `Note.Text` propiedad, el motor de enlace de Xamarin.Forms también actualizará el contenido de la `Editor`. Esto se conoce como *enlace bidireccional*.

Para obtener más información sobre el enlace de datos, vea [Enlace de datos de Xamarin.Forms](~/xamarin-forms/app-fundamentals/data-binding/index.md).

## <a name="styling"></a>Aplicación de estilos

A menudo, las aplicaciones de Xamarin.Forms contienen varios elementos visuales que tienen una apariencia idéntica. Establecer la apariencia de cada elemento visual puede ser repetitiva y propensas a errores. En su lugar, pueden crearse estilos que definen la apariencia y, a continuación, se aplica a los elementos visuales necesarios.

El [ `Style` ](xref:Xamarin.Forms.Style) clase agrupa una colección de valores de propiedad en un objeto que, a continuación, se puede aplicar a varias instancias del elemento visual. Los estilos se almacenan en un [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), ya sea en el nivel de aplicación, el nivel de página o el nivel de vista. Elegir dónde se puede definir un `Style` impactos en el que se puede usar:

- [`Style`](xref:Xamarin.Forms.Style) las instancias definidas en el nivel de aplicación se pueden aplicar a lo largo de la aplicación.
- [`Style`](xref:Xamarin.Forms.Style) las instancias definidas en el nivel de página se pueden aplicar a la página y a sus elementos secundarios.
- [`Style`](xref:Xamarin.Forms.Style) las instancias definidas en el nivel de vista se pueden aplicar a la vista y a sus elementos secundarios.

> [!IMPORTANT]
> Los estilos que se usan en toda la aplicación se almacenan en el diccionario de recursos de la aplicación para evitar la duplicación. Sin embargo, XAML que es específico de una página no debería incluirse en el diccionario de recursos de la aplicación, como los recursos, a continuación, se analizarán al iniciarse la aplicación en lugar de cuando se solicite una página.

Cada [ `Style` ](xref:Xamarin.Forms.Style) instancia contiene una colección de uno o varios [ `Setter` ](xref:Xamarin.Forms.Setter) objetos, con cada `Setter` tener un [ `Property` ](xref:Xamarin.Forms.Setter.Property) y un [`Value`](xref:Xamarin.Forms.Setter.Value). El `Property` es el nombre de la propiedad enlazable del elemento que se aplica el estilo, y el `Value` es el valor que se aplica a la propiedad. En el ejemplo de código siguiente se muestra un estilo de `NoteEntryPage`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.NoteEntryPage"
             Title="Note Entry">
    <ContentPage.Resources>
        <!-- Implicit styles -->
        <Style TargetType="{x:Type Editor}">
            <Setter Property="BackgroundColor"
                    Value="{StaticResource AppBackgroundColor}" />
        </Style>
        ...
    </ContentPage.Resources>
    ...
</ContentPage>
```

Este estilo se aplica a cualquier [ `Editor` ](xref:Xamarin.Forms.Editor) instancias de la página.

Al crear un [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) propiedad siempre es necesaria.

> [!NOTE]
> Aplicar un estilo a una aplicación de Xamarin.Forms tradicionalmente se logra mediante el uso de estilos XAML. Sin embargo, Xamarin.Forms también admite los elementos de estilo visual con hojas de estilos en cascada (CSS). Para obtener más información, consulte [aplicaciones estilo Xamarin.Forms con hojas de estilos en cascada (CSS)](~/xamarin-forms/user-interface/styles/css/index.md).

Para obtener más información sobre los estilos XAML, vea [aplicar estilos a las aplicaciones de Xamarin.Forms con estilos XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

### <a name="providing-platform-specific-styles"></a>Proporcionar estilos específicos de la plataforma

El `OnPlatform` las extensiones de marcado le permiten personalizar la apariencia de la interfaz de usuario en forma de acuerdo con la plataforma:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Notes.App">
    <Application.Resources>
        ...
        <Color x:Key="iOSNavigationBarColor">WhiteSmoke</Color>
        <Color x:Key="AndroidNavigationBarColor">#2196F3</Color>
        <Color x:Key="iOSNavigationBarTextColor">Black</Color>
        <Color x:Key="AndroidNavigationBarTextColor">White</Color>

        <Style TargetType="{x:Type NavigationPage}">
            <Setter Property="BarBackgroundColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarColor},
                                       Android={StaticResource AndroidNavigationBarColor}}" />
             <Setter Property="BarTextColor"
                    Value="{OnPlatform iOS={StaticResource iOSNavigationBarTextColor},
                                       Android={StaticResource AndroidNavigationBarTextColor}}" />           
        </Style>
        ...
    </Application.Resources>
</Application>
```

Esto [ `Style` ](xref:Xamarin.Forms.Style) establece diferentes [ `Color` ](xref:Xamarin.Forms.Color) valores para el [ `BarBackgroundColor` ](xref:Xamarin.Forms.NavigationPage.BarBackgroundColor) y [ `BarTextColor` ](xref:Xamarin.Forms.NavigationPage.BarTextColor) propiedades de [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage), según la plataforma que se va a usar.

Para más información sobre las extensiones de marcado, vea [Extensiones de marcado para el lenguaje XAML](~/xamarin-forms/xaml/markup-extensions/index.md). Para obtener información sobre la `OnPlatform` extensión de marcado, consulte [OnPlatform Markup Extension](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform-markup-extension).

## <a name="testing-and-deployment"></a>Prueba e implementación

Tanto Visual Studio para Mac como Visual Studio ofrecen numerosas opciones para probar e implementar una aplicación. Depurar aplicaciones es una parte común del ciclo de vida del desarrollo de la aplicación y ayuda a diagnosticar problemas de código. Para obtener más información, consulte [Set a Breakpoint (Establecer un punto de interrupción)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [Step Through Code (Recorrer el código paso a paso)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) y [Output Information to the Log Window (Información de salida para la ventana Registro)](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

Los simuladores son un buen lugar para comenzar a implementar y probar una aplicación, y cuentan con una funcionalidad que resulta útil a la hora de probar las aplicaciones. Sin embargo, los usuarios no usarán la aplicación final en un simulador, por lo que las aplicaciones deben probarse en dispositivos reales desde el primer momento y con frecuencia. Para obtener más información sobre el aprovisionamiento de dispositivos de iOS, consulte [Aprovisionamiento de dispositivos](~/ios/get-started/installation/device-provisioning/index.md). Para obtener más información sobre el aprovisionamiento de dispositivos de Android, consulte [Configurar el dispositivo para el desarrollo](~/android/get-started/installation/set-up-device-for-development.md).

## <a name="next-steps"></a>Pasos siguientes

Este análisis detallado de ha examinado los aspectos básicos del desarrollo de aplicaciones con Xamarin.Forms. Se recomienda que, como paso siguiente, lea sobre las funcionalidades que se indican a continuación:

- Existen cuatro grupos de control principales que se usan para crear la interfaz de usuario de una aplicación de Xamarin.Forms. Para obtener más información, vea [Referencia de controles](~/xamarin-forms/user-interface/controls/index.md).
- El enlace de datos es la técnica que consiste en vincular las propiedades de dos objetos para que los cambios en una propiedad se reflejen automáticamente en la otra propiedad. Para obtener más información, vea [Enlace de datos](~/xamarin-forms/app-fundamentals/data-binding/index.md).
- Xamarin.Forms proporciona una serie de experiencias de navegación de páginas diferente, en función del tipo de página que se use. Para obtener más información, consulte [Navigation](~/xamarin-forms/app-fundamentals/navigation/index.md) (Navegación).
- Los estilos permiten reducir el uso de marcado repetitivo y conseguir que la apariencia de las aplicaciones pueda cambiarse con mayor facilidad. Para obtener más información, vea [Aplicación de estilos a aplicaciones de Xamarin.Forms](~/xamarin-forms/user-interface/styles/index.md).
- Las extensiones de marcado XAML extienden las funciones avanzadas y la flexibilidad de XAML al permitir establecer atributos de elementos desde orígenes distintos de cadenas de texto literales. Para obtener más información, vea [Extensiones de marcado XAML](~/xamarin-forms/xaml/markup-extensions/index.md).
- Las plantillas de datos permiten definir la presentación de los datos en las vistas admitidas. Para obtener más información, consulte [Data Templates](~/xamarin-forms/app-fundamentals/templates/data-templates/index.md) (Plantillas de datos).
- Cada página, diseño y control se representan de forma diferente en cada plataforma mediante una clase `Renderer` que, a su vez, crea un control nativo, lo organiza en la pantalla y agrega el comportamiento especificado al código compartido. Los desarrolladores pueden implementar sus propias clases `Renderer` personalizadas para personalizar la apariencia o el comportamiento de un control. Para obtener más información, consulte [Custom Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) (Representadores personalizados).
- Los efectos también permiten personalizar los controles nativos de cada plataforma. Los efectos se crean en proyectos específicos de la plataforma mediante la creación de subclases de la clase [`PlatformEffect`](xref:Xamarin.Forms.PlatformEffect`2). Para usarlos, se adjuntan a un control adecuado de Xamarin.Forms. Para obtener más información, consulte [Effects](~/xamarin-forms/app-fundamentals/effects/index.md) (Efectos).
- El código compartido puede tener acceso a la funcionalidad nativa mediante la clase [`DependencyService`](xref:Xamarin.Forms.DependencyService). Para obtener más información, consulte [Accessing Native Features with DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md) (Acceso a características nativas con DependencyService).

También es recomendable el libro de Charles Petzold titulado [_Creating Mobile Apps with Xamarin.Forms_](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md) (Creación de aplicaciones móviles con Xamarin.Forms) para obtener más información sobre Xamarin.Forms. El libro está disponible como un archivo PDF o en una variedad de formatos de libro electrónico.

## <a name="related-links"></a>Vínculos relacionados

- [Lenguaje de marcado de aplicaciones eXtensible (XAML)](~/xamarin-forms/xaml/index.md)
- [Enlace de datos](~/xamarin-forms/app-fundamentals/data-binding/index.md)
- [Controls Reference](~/xamarin-forms/user-interface/controls/index.md) (Referencia de controles)
- [Extensiones de marcado XAML](~/xamarin-forms/xaml/markup-extensions/index.md)
- [Xamarin.Forms Samples](https://developer.xamarin.com/samples/xamarin-forms/all/) (Ejemplos de Xamarin.Forms)
- [Getting Started Samples](https://developer.xamarin.com/samples/xamarin-forms/GetStarted/) (Ejemplos de introducción)
- [Referencia de la API de Xamarin.Forms](xref:Xamarin.Forms)
- [Aprendizaje autoguiado gratuito (vídeo)](https://university.xamarin.com/self-guided/)
