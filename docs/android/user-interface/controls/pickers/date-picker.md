---
title: Selector de fecha
description: Seleccione las fechas del calendario con el DatePickerDialog y DialogFragment
ms.prod: xamarin
ms.assetid: F2BCD8D4-8957-EA53-C5A8-6BB603ADB47B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/22/2018
ms.openlocfilehash: 9f82317f6041de3952d11b391afffafe6fbd8761
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61153540"
---
# <a name="date-picker"></a>Selector de fecha

## <a name="overview"></a>Información general

Hay ocasiones cuando un usuario debe introducir datos en una aplicación de Android. Para ayudar con esto, proporciona el marco de trabajo Android el [ `DatePicker` ](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/) widget y [ `DatePickerDialog` ](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/) . El `DatePicker` permite a los usuarios seleccionar el año, mes y día en una interfaz coherente entre dispositivos y aplicaciones. El `DatePickerDialog` es una clase auxiliar que encapsula el `DatePicker` en un cuadro de diálogo.

Aplicaciones modernas de Android deben mostrar el `DatePickerDialog` en un [ `DialogFragment` ](https://developer.xamarin.com/api/type/Android.App.DialogFragment/). Esto permitirá que una aplicación mostrar el selector de fechas como un cuadro de diálogo emergente o incrustado en una actividad. Además, el `DialogFragment` va a administrar el ciclo de vida y la presentación del cuadro de diálogo, lo que reduce la cantidad de código que se debe implementar.

Esta guía demuestra cómo usar el `DatePickerDialog`, ajustados en un `DialogFragment`. La aplicación de ejemplo mostrará el `DatePickerDialog` como un cuadro de diálogo modal cuando el usuario hace clic en un botón en una actividad. Cuando se establece la fecha por el usuario, un `TextView` se actualizará con la fecha en que se ha seleccionado.

[![Botón de captura de pantalla de elegir fecha seguido por el cuadro de diálogo de selector de fecha](date-picker-images/image-01-sml.png)](date-picker-images/image-01.png#lightbox)

## <a name="requirements"></a>Requisitos

La aplicación de ejemplo en esta guía tiene como destino Android 4.1 (nivel de API
16) o superior, pero se puede aplicar a Android 3.0 (API nivel 11 o superior). Es posible compatibilidad con versiones anteriores de Android con la adición de la v4 Android Support Library al proyecto y algunos cambios de código.

## <a name="using-the-datepicker"></a>Uso de DatePicker

En este ejemplo se extenderá `DialogFragment`. La subclase hospedará y mostrar un `DatePickerDialog`:

![Cuadro de diálogo de primer plano del selector de fecha](date-picker-images/image-02.png)

Cuando el usuario selecciona una fecha y hace clic en el **Aceptar** botón, el `DatePickerDialog` llamará al método [ `IOnDateSetListener.OnDateSet` ](https://developer.xamarin.com/api/member/Android.App.DatePickerDialog+IOnDateSetListener.OnDateSet/p/Android.Widget.DatePicker/System.Int32/System.Int32/System.Int32/).
Esta interfaz se implementa mediante el hospedaje `DialogFragment`. Si el usuario hace clic en el **cancelar** botón, cuadro de diálogo y fragmento, se cerrará a sí mismos.

Hay varias maneras el `DialogFragment` puede devolver la fecha seleccionada a la actividad de hospedaje:

1. **Invocar un método o establecer una propiedad** &ndash; la actividad puede proporcionar una propiedad o método específicamente para establecer este valor.

2. **Generar un evento** &ndash; el `DialogFragment` puede definir un evento que se genera cuando `OnDateSet` se invoca.

3. **Use un `Action`**  &ndash; el `DialogFragment` puede invocar un `Action<DateTime>` para mostrar la fecha en la actividad. La actividad proporcionará la `Action<DateTime` al crear una instancia el `DialogFragment`. En este ejemplo usará la tercera técnica y requerir que proporcione la actividad un `Action<DateTime>` a la `DialogFragment`.



### <a name="extending-dialogfragment"></a>Extender DialogFragment

El primer paso para mostrar un `DatePickerDialog` es poner en subclase `DialogFragment` y hacer que implemente la `IOnDateSetListener` interfaz:

```csharp
public class DatePickerFragment : DialogFragment, 
                                  DatePickerDialog.IOnDateSetListener
{
    // TAG can be any string of your choice.
    public static readonly string TAG = "X:" + typeof (DatePickerFragment).Name.ToUpper();
    
    // Initialize this value to prevent NullReferenceExceptions.
    Action<DateTime> _dateSelectedHandler = delegate { };
    
    public static DatePickerFragment NewInstance(Action<DateTime> onDateSelected)
    {
        DatePickerFragment frag = new DatePickerFragment();
        frag._dateSelectedHandler = onDateSelected;
        return frag;
    }
    
    public override Dialog OnCreateDialog(Bundle savedInstanceState)
    {
        DateTime currently = DateTime.Now;
        DatePickerDialog dialog = new DatePickerDialog(Activity, 
                                                       this, 
                                                       currently.Year, 
                                                       currently.Month - 1,
                                                       currently.Day);
        return dialog;
    }
    
    public void OnDateSet(DatePicker view, int year, int monthOfYear, int dayOfMonth)
    {
        // Note: monthOfYear is a value between 0 and 11, not 1 and 12!
        DateTime selectedDate = new DateTime(year, monthOfYear + 1, dayOfMonth);
        Log.Debug(TAG, selectedDate.ToLongDateString());
        _dateSelectedHandler(selectedDate);
    }
}
```

El `NewInstance` método se invoca para crear una nueva instancia `DatePickerFragment`. Este método toma un `Action<DateTime>` que se invocará cuando el usuario hace clic en el **Aceptar** situado en la `DatePickerDialog`.

Cuando el fragmento que se va a mostrarse, Android llamará al método `OnCreateDialog`. Este método creará una nueva `DatePickerDialog` objeto e inicializarlo con la fecha actual y el objeto de devolución de llamada (que es la instancia actual de la `DatePickerFragment`).


> [!NOTE]
> Tenga en cuenta que el valor del mes cuando `IOnDateSetListener.OnDateSet` se invoca en el intervalo de 0 a 11 y no de 1 a 12. Será el día del mes en el intervalo de 1 a 31 (dependiendo de qué se seleccionó el mes).



### <a name="showing-the-datepickerfragment"></a>Mostrando el DatePickerFragment

Ahora que el `DialogFragment` ha sido implementado, en esta sección examinará cómo usar el fragmento en una actividad. En la aplicación de ejemplo que acompaña a esta guía, creará una instancia de la actividad del `DialogFragment` utilizando el `NewInstance` método de fábrica y, después, mostrar invocar `DialogFragment.Show`. Como parte de la creación de instancias la `DialogFragment`, la actividad pasa un `Action<DateTime>`, que mostrará la fecha en un `TextView` hospedada por la actividad:

```csharp
[Activity(Label = "@string/app_name", MainLauncher = true, Icon = "@drawable/icon")]
public class MainActivity : Activity
{
    TextView _dateDisplay;
    Button _dateSelectButton;

    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);

        _dateDisplay = FindViewById<TextView>(Resource.Id.date_display);
        _dateSelectButton = FindViewById<Button>(Resource.Id.date_select_button);
        _dateSelectButton.Click += DateSelect_OnClick;
    }

    void DateSelect_OnClick(object sender, EventArgs eventArgs)
    {
        DatePickerFragment frag = DatePickerFragment.NewInstance(delegate(DateTime time)
                                                                 {
                                                                     _dateDisplay.Text = time.ToLongDateString();
                                                                 });
        frag.Show(FragmentManager, DatePickerFragment.TAG);
    }
}
```


## <a name="summary"></a>Resumen

En este ejemplo se describe cómo mostrar un `DatePicker` widget como un cuadro de diálogo modal de menú emergente como parte de una actividad de Android. Se proporciona una implementación de ejemplo DialogFragment y tratan el `IOnDateSetListener` interfaz. En este ejemplo también muestra cómo el DialogFragment puede interactuar con el host de actividad para mostrar la fecha seleccionada.


## <a name="related-links"></a>Vínculos relacionados

- [DialogFragment](https://developer.xamarin.com/api/type/Android.App.DialogFragment/)
- [DatePicker](https://developer.xamarin.com/api/type/Android.Widget.DatePicker/)
- [DatePickerDialog](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog/)
- [DatePickerDialog.IOnDateSetListener](https://developer.xamarin.com/api/type/Android.App.DatePickerDialog+IOnDateSetListener/)
- [Seleccione una fecha](https://github.com/xamarin/recipes/tree/master/Recipes/android/controls/datepicker/select_a_date)
