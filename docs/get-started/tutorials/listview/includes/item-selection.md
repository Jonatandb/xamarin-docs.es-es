---
ms.openlocfilehash: c33db7de63a585cb6fb47aa1145aa724c9eb790f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61037569"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. En **MainPage.xaml**, modifique la declaración [`ListView`](xref:Xamarin.Forms.ListView) para que establezca controladores para los eventos [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) y [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped):

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Este código establece el evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) en un controlador de eventos denominado `OnListViewItemSelected`, y el evento [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), en un controlador de eventos llamado `OnListViewItemTapped`. Ambos controladores de eventos se crearán en el paso siguiente.

1. En el **Explorador de soluciones**, en el proyecto **ListViewTutorial**, expanda **MainPage.xaml** y haga doble clic en **MainPage.xaml.cs** para abrirlo. Después, en **MainPage.xaml.cs**, agregue los controladores de eventos `OnListViewItemSelected` y `OnListViewItemTapped` a la clase:

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Cuando se pulsa un elemento de [`ListView`](xref:Xamarin.Forms.ListView), se desencadenan los eventos [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) y [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), que ejecutarán los métodos `OnListViewItemSelected` y `OnListItemTapped`, respectivamente. El argumento `sender` en ambos métodos es el objeto `ListView` responsable de desencadenar el evento y puede usarse para acceder al objeto `ListView`. El argumento [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) en el método `OnListViewItemSelected` proporciona el elemento seleccionado y el argumento [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) en el método `OnListViewItemTapped` proporciona el elemento pulsado.

    > [!IMPORTANT]
    > El evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) solo se desencadena cuando se selecciona un nuevo elemento en [`ListView`](xref:Xamarin.Forms.ListView). Por lo tanto, al pulsar dos veces el mismo elemento se desencadenarán dos eventos [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), pero solo un evento `ItemSelected`.

1. En la barra de herramientas de Visual Studio, pulse el botón **Iniciar** (el botón triangular similar a un botón de reproducción) para iniciar la aplicación en Android Emulator o en el simulador remoto de iOS elegido:

    [![Captura de pantalla de un elemento ListView que responde a la pulsación y selección de elementos en iOS y Android](../images/item-selection.png "Selección de elementos de ListView")](../images/item-selection-large.png#lightbox "Selección de elementos de ListView")

    Establezca puntos de interrupción en los dos controladores de eventos y pulse los elementos de [`ListView`](xref:Xamarin.Forms.ListView). Tenga en cuenta que el evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) solo se desencadena cuando se selecciona un nuevo elemento en [`ListView`](xref:Xamarin.Forms.ListView), mientras que el evento [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) se desencadena cada vez que se pulsa un elemento.

    Para obtener más información sobre la pulsación y selección de elementos, consulte [Selección y pulsaciones](~/xamarin-forms/user-interface/listview/interactivity.md#selection--taps) en la guía [Interactividad de ListView](~/xamarin-forms/user-interface/listview/interactivity.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio para Mac](#tab/vsmac)

1. En **MainPage.xaml**, modifique la declaración [`ListView`](xref:Xamarin.Forms.ListView) para que establezca controladores para los eventos [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) y [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped):

    ```xaml
    <ListView ItemsSource="{Binding Monkeys}"
              ItemSelected="OnListViewItemSelected"
              ItemTapped="OnListViewItemTapped" />
    ```

    Este código establece el evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) en un controlador de eventos denominado `OnListViewItemSelected`, y el evento [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), en un controlador de eventos llamado `OnListViewItemTapped`. Ambos controladores de eventos se crearán en el paso siguiente.

1. En el **Panel de solución**, en el proyecto **ListViewTutorial**, expanda **MainPage.xaml** y haga doble clic en **MainPage.xaml.cs** para abrirlo. Después, en **MainPage.xaml.cs**, agregue los controladores de eventos `OnListViewItemSelected` y `OnListViewItemTapped` a la clase:

    ```csharp
    void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs e)
    {
        Monkey selectedItem = e.SelectedItem as Monkey;
    }

    void OnListViewItemTapped(object sender, ItemTappedEventArgs e)
    {
        Monkey tappedItem = e.Item as Monkey;
    }
    ```

    Cuando se pulsa un elemento de [`ListView`](xref:Xamarin.Forms.ListView), se desencadenan los eventos [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) y [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), que ejecutarán los métodos `OnListViewItemSelected` y `OnListItemTapped`, respectivamente. El argumento `sender` en ambos métodos es el objeto `ListView` responsable de desencadenar el evento y puede usarse para acceder al objeto `ListView`. El argumento [`SelectedItemChangedEventArgs`](xref:Xamarin.Forms.SelectedItemChangedEventArgs) en el método `OnListViewItemSelected` proporciona el elemento seleccionado y el argumento [`ItemTappedEventArgs`](xref:Xamarin.Forms.ItemTappedEventArgs) en el método `OnListViewItemTapped` proporciona el elemento pulsado.

    > [!IMPORTANT]
    > El evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) solo se desencadena cuando se selecciona un nuevo elemento en [`ListView`](xref:Xamarin.Forms.ListView). Por lo tanto, al pulsar dos veces el mismo elemento se desencadenarán dos eventos [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped), pero solo un evento `ItemSelected`.

1. En la barra de herramientas de Visual Studio para Mac, pulse el botón **Iniciar** (el botón triangular similar a un botón de reproducción) para iniciar la aplicación en Android Emulator o en el simulador de iOS elegido:

    [![Captura de pantalla de un elemento ListView que responde a la pulsación y selección de elementos en iOS y Android](../images/item-selection.png "Selección de elementos de ListView")](../images/item-selection-large.png#lightbox "Selección de elementos de ListView")

    Establezca puntos de interrupción en los dos controladores de eventos y pulse los elementos de [`ListView`](xref:Xamarin.Forms.ListView). Tenga en cuenta que el evento [`ItemSelected`](xref:Xamarin.Forms.ListView.ItemSelected) solo se desencadena cuando se selecciona un nuevo elemento en [`ListView`](xref:Xamarin.Forms.ListView), mientras que el evento [`ItemTapped`](xref:Xamarin.Forms.ListView.ItemTapped) se desencadena cada vez que se pulsa un elemento.

    Para obtener más información sobre la pulsación y selección de elementos, consulte [Selección y pulsaciones](~/xamarin-forms/user-interface/listview/interactivity.md#selection--taps) en la guía [Interactividad de ListView](~/xamarin-forms/user-interface/listview/interactivity.md).
