---
title: Clases base y controles nativos del representador
description: Todos los controles de Xamarin.Forms tienen un representador que los acompaña para cada plataforma y que crea una instancia de un control nativo. En este artículo se enumeran las clases de representador y control nativo que implementan cada página, diseño, vista y celda de Xamarin.Forms.
ms.prod: xamarin
ms.assetid: A8909AE3-ED0E-4D24-BF96-B49E732E3B93
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/19/2018
ms.openlocfilehash: cdca5294ea12bf8907ea5f6242efea00f384e77e
ms.sourcegitcommit: 5d4e6677224971e2bc0268f405d192d0358c74b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58329343"
---
# <a name="renderer-base-classes-and-native-controls"></a>Clases base y controles nativos del representador

_Todos los controles de Xamarin.Forms tienen un representador que los acompaña para cada plataforma y que crea una instancia de un control nativo. En este artículo se enumeran las clases de representador y control nativo que implementan cada página, diseño, vista y celda de Xamarin.Forms._

A excepción de la clase `MapRenderer`, los representadores específicos de la plataforma se pueden encontrar en los espacios de nombres siguientes:

- **iOS**: Xamarin.Forms.Platform.iOS
- **Android**: Xamarin.Forms.Platform.Android
- **Android (AppCompat)**: Xamarin.Forms.Platform.Android.AppCompat
- **Plataforma universal de Windows (UWP)**: Xamarin.Forms.Platform.UWP

La clase `MapRenderer` se puede encontrar en los espacios de nombres siguientes:

- **iOS**: Xamarin.Forms.Maps.iOS
- **Android**: Xamarin.Forms.Maps.Android
- **Plataforma universal de Windows (UWP)**: Xamarin.Forms.Maps.UWP

## <a name="pages"></a>Páginas

En la tabla siguiente se enumeran las clases de representador y control nativo que implementan cada tipo [Page](~/xamarin-forms/user-interface/controls/pages.md) de Xamarin.Forms:

|Página|Representador|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ContentPage`](xref:Xamarin.Forms.ContentPage)|[PageRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/contentpage.md)|UIViewController|ViewGroup||FrameworkElement|
|[`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage)|PhoneMasterDetailRenderer (iOS: teléfono), TabletMasterDetailPageRenderer (iOS: tableta), MasterDetailRenderer (Android), MasterDetailPageRenderer (Android AppCompat), MasterDetailPageRenderer (UWP)|UIViewController (teléfono), UISplitViewController (tableta)|DrawerLayout (v4)|DrawerLayout (v4)|FrameworkElement (Control personalizado)|
|[`NavigationPage`](xref:Xamarin.Forms.NavigationPage)|NavigationRenderer (iOS y Android), NavigationPageRenderer (AppCompat Android), NavigationPageRenderer (UWP)|UIToolbar|ViewGroup|ViewGroup|FrameworkElement (Control personalizado)|
|[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)|TabbedRenderer (iOS y Android), TabbedPageRenderer (Android AppCompat), TabbedPageRenderer (UWP)|UIView|ViewPager|ViewPager|FrameworkElement (Pivot)|
|[`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)|PageRenderer|UIViewController|ViewGroup||FrameworkElement|
|[`CarouselPage`](xref:Xamarin.Forms.CarouselPage)|CarouselPageRenderer|UIScrollView|ViewPager|ViewPager|FrameworkElement (FlipView)|

## <a name="layouts"></a>Diseños

En la tabla siguiente se enumeran las clases de representador y control nativo que implementan cada tipo [Layout](~/xamarin-forms/user-interface/controls/layouts.md) de Xamarin.Forms:

|Diseño|Representador|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`ContentView`](xref:Xamarin.Forms.ContentView)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`FlexLayout`](xref:Xamarin.Forms.FlexLayout)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`Frame`](xref:Xamarin.Forms.Frame)|FrameRenderer|UIView|ViewGroup|Borde|
|[`ScrollView`](xref:Xamarin.Forms.ScrollView)|ScrollViewRenderer|UIScrollView|ScrollView|ScrollViewer|
|[`TemplatedView`](xref:Xamarin.Forms.TemplatedView)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`AbsoluteLayout`](xref:Xamarin.Forms.AbsoluteLayout)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`Grid`](xref:Xamarin.Forms.Grid)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`RelativeLayout`](xref:Xamarin.Forms.RelativeLayout)|ViewRenderer|UIView|Ver|FrameworkElement|
|[`StackLayout`](xref:Xamarin.Forms.StackLayout)|ViewRenderer|UIView|Ver|FrameworkElement|

## <a name="views"></a>Vistas

En la tabla siguiente se enumeran las clases de representador y control nativo que implementan cada tipo [View](~/xamarin-forms/user-interface/controls/views.md) de Xamarin.Forms:

|Vistas|Representador|iOS|Android|Android (AppCompat)|UWP|
|--- |--- |--- |--- |--- |--- |
|[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)|ActivityIndicatorRenderer|UIActivityIndicator|ProgressBar||ProgressBar|
|[`BoxView`](xref:Xamarin.Forms.BoxView)|BoxRenderer (iOS y Android), BoxViewRenderer (UWP)|UIView|ViewGroup||Rectángulo|
|[`Button`](xref:Xamarin.Forms.Button)|ButtonRenderer|UIButton|Botón|AppCompatButton|Botón|
|`CollectionView`|CollectionViewRenderer|UICollectionView||RecyclerView||
|[`DatePicker`](xref:Xamarin.Forms.DatePicker)|DatePickerRenderer|UITextField|EditText||DatePicker|
|[`Editor`](xref:Xamarin.Forms.Editor)|EditorRenderer|UITextView|EditText||TextBox|
|[`Entry`](xref:Xamarin.Forms.Entry)|[EntryRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/entry.md)|UITextField|EditText||TextBox|
|[`Image`](xref:Xamarin.Forms.Image)|ImageRenderer|UIImageView|ImageView||Imagen|
|[`ImageButton`](xref:Xamarin.Forms.ImageButton)|ImageButtonRenderer|UIButton||AppCompatImageButton|Botón|
|[`Label`](xref:Xamarin.Forms.Label)|LabelRenderer|UILabel|TextView||TextBlock|
|[`ListView`](xref:Xamarin.Forms.ListView)|[ListViewRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/listview.md)|UITableView|ListView||ListView|
|[`Map`](xref:Xamarin.Forms.Maps.Map)|[MapRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)|MKMapView|MapView||MapControl|
|[`Picker`](xref:Xamarin.Forms.Picker)|PickerRenderer|UITextField|EditText|EditText|ComboBox|
|[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)|ProgressBarRenderer|UIProgressView|ProgressBar||ProgressBar|
|[`SearchBar`](xref:Xamarin.Forms.SearchBar)|SearchBarRenderer|UISearchBar|SearchView||AutoSuggestBox|
|[`Slider`](xref:Xamarin.Forms.Slider)|SliderRenderer|UISlider|SeekBar||Slider|
|[`Stepper`](xref:Xamarin.Forms.Stepper)|StepperRenderer|UIStepper|LinearLayout||Control|
|[`Switch`](xref:Xamarin.Forms.Switch)|SwitchRenderer|UISwitch|Modificador|SwitchCompat|ToggleSwitch|
|[`TableView`](xref:Xamarin.Forms.TableView)|TableViewRenderer|UITableView|ListView||ListView|
|[`TimePicker`](xref:Xamarin.Forms.TimePicker)|TimePickerRenderer|UITextField|EditText||TimePicker|
|[`WebView`](xref:Xamarin.Forms.WebView)|WebViewRenderer|UIWebView|WebView||WebView|

## <a name="cells"></a>Celdas

En la tabla siguiente se enumeran las clases de representador y control nativo que implementan cada tipo [Cell](~/xamarin-forms/user-interface/controls/cells.md) de Xamarin.Forms:

|Celdas|Representador|iOS|Android|UWP|
|--- |--- |--- |--- |--- |
|[`EntryCell`](xref:Xamarin.Forms.EntryCell)|EntryCellRenderer|UITableViewCell con UITextField|LinearLayout con TextView y EditText|DataTemplate con un control TextBox|
|[`SwitchCell`](xref:Xamarin.Forms.SwitchCell)|SwitchCellRenderer|UITableViewCell con UISwitch|Modificador|DataTemplate con un elemento Grid que contiene controles TextBlock y ToggleSwitch|
|[`TextCell`](xref:Xamarin.Forms.TextCell)|TextCellRenderer|UITableViewCell|LinearLayout con dos objetos TextView|DataTemplate con un elemento StackPanel que contiene dos elementos TextBlock|
|[`ImageCell`](xref:Xamarin.Forms.ImageCell)|ImageCellRenderer|UITableViewCell con UIImage|LinearLayout con dos objetos TextView y un objeto ImageView|DataTemplate con un elemento Grid que contiene un control Image y dos TextBlock|
|[`ViewCell`](xref:Xamarin.Forms.ViewCell)|[ViewCellRenderer](~/xamarin-forms/app-fundamentals/custom-renderer/viewcell.md)|UITableViewCell|Ver|DataTemplate con un elemento ContentPresenter|

## <a name="summary"></a>Resumen

En este artículo se han enumerado las clases de representador y control nativo que implementan cada página, diseño, vista y celda de Xamarin.Forms. Todos los controles de Xamarin.Forms tienen un representador que los acompaña para cada plataforma y que crea una instancia de un control nativo.

## <a name="related-links"></a>Vínculos relacionados

- [Representadores personalizados (vídeo de Xamarin University)](https://developer.xamarin.com/videos/cross-platform/xamarinforms-custom-renderers/)
