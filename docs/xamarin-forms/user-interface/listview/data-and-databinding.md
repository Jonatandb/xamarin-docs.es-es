---
title: Orígenes de datos de ListView
description: En este artículo se explica cómo rellenar el ListView de Xamarin.Forms con datos y cómo usar el enlace de datos con un ListView.
ms.prod: xamarin
ms.assetid: B5571660-1E82-4379-95C3-0725288CF5D9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/30/2018
ms.openlocfilehash: e53f6dce47dd7db60267d21c8d816ece554dc46c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61320017"
---
# <a name="listview-data-sources"></a>Orígenes de datos de ListView

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)

Un [ `ListView` ](xref:Xamarin.Forms.ListView) se utiliza para mostrar listas de datos. Aprenderemos acerca de cómo rellenar un ListView con datos y cómo podemos enlazar al elemento seleccionado.

## <a name="itemssource"></a>ItemsSource

Un [ `ListView` ](xref:Xamarin.Forms.ListView) se rellena con datos mediante el [ `ItemsSource` ](xref:Xamarin.Forms.ItemsView`1.ItemsSource) propiedad, que puede aceptar cualquier colección que implementa `IEnumerable`. La manera más sencilla para rellenar un `ListView` implica el uso de una matriz de cadenas:

```xaml
<ListView>
      <ListView.ItemsSource>
          <x:Array Type="{x:Type x:String}">
            <x:String>mono</x:String>
            <x:String>monodroid</x:String>
            <x:String>monotouch</x:String>
            <x:String>monorail</x:String>
            <x:String>monodevelop</x:String>
            <x:String>monotone</x:String>
            <x:String>monopoly</x:String>
            <x:String>monomodal</x:String>
            <x:String>mononucleosis</x:String>
          </x:Array>
      </ListView.ItemsSource>
</ListView>
```

El código de C# equivalente es:

```csharp
var listView = new ListView();
listView.ItemsSource = new string[]
{
  "mono",
  "monodroid",
  "monotouch",
  "monorail",
  "monodevelop",
  "monotone",
  "monopoly",
  "monomodal",
  "mononucleosis"
};

//monochrome will not appear in the list because it was added
//after the list was populated.
listView.ItemsSource.Add("monochrome");
```

![](data-and-databinding-images/itemssource-simple.png "ListView Mostrar lista de cadenas")

El enfoque anterior se rellenará la `ListView` con una lista de cadenas. De forma predeterminada, `ListView` llamará `ToString` y mostrar el resultado en un `TextCell` para cada fila. Para personalizar cómo se muestran los datos, vea [aspecto de la celda](~/xamarin-forms/user-interface/listview/customizing-cell-appearance.md).

Dado que `ItemsSource` se ha enviado a una matriz, no se actualizará el contenido como los cambios de lista o matriz subyacentes. Si desea que el ListView para actualizarse automáticamente a medida que los elementos se agregan, quitan y puede cambiar en la lista subyacente, deberá usar un `ObservableCollection`. [`ObservableCollection`](xref:System.Collections.ObjectModel.ObservableCollection`1) se define en `System.Collections.ObjectModel` y es exactamente igual que `List`, excepto en que se puede notificar `ListView` de los cambios:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
listView.ItemsSource = employees;

//Mr. Mono will be added to the ListView because it uses an ObservableCollection
employees.Add(new Employee(){ DisplayName="Mr. Mono"});
```

<a name="Data_Binding" />

## <a name="data-binding"></a>Enlace de datos
Enlace de datos es la "adherencia" que enlaza las propiedades de un objeto de interfaz de usuario a las propiedades de un objeto CLR, como una clase en ViewModel. Enlace de datos es útil porque simplifica el desarrollo de interfaces de usuario mediante la sustitución mucha aburrido código reutilizable.

Enlace de datos funciona manteniendo los objetos sincronizados a medida que cambian sus valores enlazados. En lugar de tener que escribir controladores de eventos para cada vez que cambia el valor de un control, establecer el enlace y habilitar el enlace en ViewModel.

Para obtener más información sobre el enlace de datos, vea [conceptos básicos del enlace de datos](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md) que es la cuarta parte de la [serie de artículos de conceptos básicos de XAML de Xamarin.Forms](~/xamarin-forms/xaml/xaml-basics/index.md).

### <a name="binding-cells"></a>Enlazar celdas
Propiedades de celdas (y elementos secundarios de las celdas) se pueden enlazar a propiedades de objetos en el `ItemsSource`. Por ejemplo, podría usarse un ListView para presentar una lista de empleados.

La clase employee:

```csharp
public class Employee
{
    public string DisplayName {get; set;}
}
```

`ObservableCollection<Employee>` se crea y se establece como el `ListView`del `ItemsSource`:

```csharp
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();
public EmployeeListPage()
{
  //defined in XAML to follow
  EmployeeView.ItemsSource = employees;
  ...
}
```

La lista se rellena con datos:

```csharp
public EmployeeListPage()
{
  ...
  employees.Add(new Employee{ DisplayName="Rob Finnerty"});
  employees.Add(new Employee{ DisplayName="Bill Wrestler"});
  employees.Add(new Employee{ DisplayName="Dr. Geri-Beth Hooper"});
  employees.Add(new Employee{ DisplayName="Dr. Keith Joyce-Purdy"});
  employees.Add(new Employee{ DisplayName="Sheri Spruce"});
  employees.Add(new Employee{ DisplayName="Burt Indybrick"});
}
```

El siguiente fragmento muestra un `ListView` enlazado a una lista de empleados:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
xmlns:constants="clr-namespace:XamarinFormsSample;assembly=XamarinFormsXamlSample"
x:Class="XamarinFormsXamlSample.Views.EmployeeListPage"
Title="Employee List">
  <ListView x:Name="EmployeeView">
    <ListView.ItemTemplate>
      <DataTemplate>
        <TextCell Text="{Binding DisplayName}" />
      </DataTemplate>
    </ListView.ItemTemplate>
  </ListView>
</ContentPage>
```

Tenga en cuenta que el enlace es el programa de instalación en el código por motivos de simplicidad, aunque podría haber enlazado en XAML.

El bit de XAML anterior define un `ContentPage` que contiene un `ListView`. El origen de datos de la `ListView` se establece a través de la `ItemsSource` atributo. El diseño de cada fila de la `ItemsSource`se define dentro el `ListView.ItemTemplate` elemento.

Éste es el resultado:

![](data-and-databinding-images/bound-data.png "ListView con enlace de datos")

### <a name="binding-selecteditem"></a>Enlace SelectedItem

A menudo conveniente enlazar con el elemento seleccionado de un `ListView`, en lugar de usar un controlador de eventos para responder a cambios. Para hacer esto en XAML, enlazar la `SelectedItem` propiedad:

```xaml
<ListView x:Name="listView"
 SelectedItem="{Binding Source={x:Reference SomeLabel},
 Path=Text}">
 …
</ListView>
```

Suponiendo que `listView`del `ItemsSource` es una lista de cadenas, `SomeLabel` tendrán su propiedad text enlazada a la `SelectedItem`.

## <a name="related-links"></a>Vínculos relacionados

- [Enlaces bidireccionales (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ListView/SwitchEntryTwoBinding)
