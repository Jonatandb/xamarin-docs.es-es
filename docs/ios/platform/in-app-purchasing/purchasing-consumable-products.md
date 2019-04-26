---
title: Compra de productos consumibles en Xamarin.iOS
description: Este documento describe los productos consumibles en Xamarin.iOS. Productos consumibles son fragmentos de uso simple de funcionalidad como moneda en juego.
ms.prod: xamarin
ms.assetid: E0CB4A0F-C3FA-3933-58A7-13246971D677
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: b55465a700974e0ce5ceb8893d96311d920e04ae
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61366708"
---
# <a name="purchasing-consumable-products-in-xamarinios"></a>Compra de productos consumibles en Xamarin.iOS

Productos consumibles son las más fáciles de implementar, ya que no hay ningún requisito 'restore'. Son útiles para productos como moneda en juego o una parte de uso simple de funcionalidad. Los usuarios pueden volver a adquirir nuevo productos consumibles over y over.

## <a name="built-in-product-delivery"></a>Entrega de productos integrados

El código de ejemplo que acompaña a este documento muestra productos integrados: el Id. de producto están codificados en la aplicación porque están estrechamente acopladas en el código que desbloquea la característica después de pago. El proceso de compra se puede visualizar similar al siguiente:   
   
[![La visualización de proceso de compra](purchasing-consumable-products-images/image26.png)](purchasing-consumable-products-images/image26.png#lightbox)     
   
 El flujo de trabajo básico es:   
   
 1. La aplicación agrega un `SKPayment` a la cola. Si es necesario el usuario se le solicitará su identificador de Apple y se le pedirá que confirme el pago.   
   
 2. StoreKit envía la solicitud al servidor para su procesamiento.   
   
 3. Cuando se completa la transacción, el servidor responde con una confirmación de transacción.   
   
 4. El `SKPaymentTransactionObserver` subclase recibe la notificación y lo procesa.   
   
 5. La aplicación permite que el producto (mediante la actualización `NSUserDefaults` o algún otro mecanismo) y, a continuación, llama a del StoreKit `FinishTransaction`.

Hay otro tipo de flujo de trabajo – *Server-Delivered productos* , es decir tratan más adelante en el documento (vea la sección *comprobación de recepción y productos Server-Delivered*).

## <a name="consumable-products-example"></a>Ejemplo de productos consumible

El [InAppPurchaseSample código](https://developer.xamarin.com/samples/monotouch/StoreKit/) contiene un proyecto denominado *consumibles* que implementa un basic 'en el juego currency' (denominado "juguetear créditos"). El ejemplo muestra cómo implementar dos productos de compra en la aplicación para permitir al usuario comprar como muchos "créditos monkey" como deseen: en una aplicación real también habría cierto nivel de gasto de ellos.   
   
   
   
 La aplicación se muestra en estas capturas de pantalla: cada compra más "créditos monkey" agrega al saldo del usuario:   
   
   
   
 [![Cada compra más créditos de monkey agrega al saldo de los usuarios](purchasing-consumable-products-images/image27.png)](purchasing-consumable-products-images/image27.png#lightbox)   
   
   
   
 Las interacciones entre las clases personalizadas, StoreKit y Store de la aplicación tendrá este aspecto:   
   
   
   
 [![Las interacciones entre las clases personalizadas, StoreKit y Store de la aplicación](purchasing-consumable-products-images/image28.png)](purchasing-consumable-products-images/image28.png#lightbox)

&nbsp;

### <a name="viewcontroller-methods"></a>ViewController métodos

Además de las propiedades y métodos necesarios para recuperar información del producto, el controlador de vista requiere los observadores adicional de notificación escuchar las notificaciones relacionadas con la compra. Estos son solo `NSObjects` que va a registrar y quitar en `ViewWillAppear` y `ViewWillDisappear` respectivamente.

```csharp
NSObject succeededObserver, failedObserver;
```

El constructor también creará la `SKProductsRequestDelegate` subclase ( `InAppPurchaseManager`) que a su vez crea y registra el `SKPaymentTransactionObserver` ( `CustomPaymentObserver`).   
   
   
   
 Es la primera parte del procesamiento de una transacción de compra en la aplicación controlar el presionar el botón cuando el usuario desea comprar algo, como se muestra en el siguiente código de la aplicación de ejemplo:

```csharp
buy5Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy5ProductId);
};
buy10Button.TouchUpInside += (sender, e) => {
   iap.PurchaseProduct (Buy10ProductId);
};
```

   
   
 La segunda parte de la interfaz de usuario está controlando la notificación de que la transacción se ha realizado correctamente, en este caso mediante la actualización del saldo mostrado:

```csharp
priceObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionSucceededNotification,
(notification) => {
   balanceLabel.Text = CreditManager.Balance() + " monkey credits";
});
```

La parte final de la interfaz de usuario muestra un mensaje si una transacción se cancela por algún motivo. En el código de ejemplo simplemente se escribe un mensaje en la ventana de salida:

```csharp
failedObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerTransactionFailedNotification,
(notification) => {
   Console.WriteLine ("Transaction Failed");
});
```

Además de estos métodos en el controlador de vista, una transacción de compra de productos consumibles también requiere código en el `SKProductsRequestDelegate` y `SKPaymentTransactionObserver`.

### <a name="inapppurchasemanager-methods"></a>Métodos InAppPurchaseManager

El código de ejemplo implementa un número de compra de los métodos relacionados en la clase InAppPurchaseManager, incluido el `PurchaseProduct` método que crea un `SKPayment` de instancia y lo agrega a la cola para su procesamiento:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKPayment payment = SKPayment.PaymentWithProduct (appStoreProductId);
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

Agregando el pago a la cola es una operación asincrónica. La aplicación recupera el control mientras StoreKit procesa la transacción y lo envía a los servidores de Apple. En este punto es que iOS comprobará el usuario inicia sesión en el Store de la aplicación y se le pida el identificador de Apple y la contraseña si es necesario.   
   
   
   
 Suponiendo que el usuario correctamente, se autentica con el Store de la aplicación y acepta la transacción, el `SKPaymentTransactionObserver` recibirá la respuesta del StoreKit y llame al método siguiente para completar la transacción y terminarla.

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   // Register the purchase, so it is remembered for next time
   PhotoFilterManager.Purchase(productId);
   FinishTransaction(transaction, true);
}
```

El último paso consiste en asegurarse de que notifique a StoreKit que ha cumplido correctamente la transacción, mediante una llamada a `FinishTransaction`:

```csharp
public void FinishTransaction(SKPaymentTransaction transaction, bool wasSuccessful)
{
   // remove the transaction from the payment queue.
   SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);  // THIS IS IMPORTANT - LET'S APPLE KNOW WE'RE DONE !!!!
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {transaction},new NSObject[] {new NSString("transaction")});
       if (wasSuccessful) {
           // send out a notification that we've finished the transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionSucceededNotification, this, userInfo);
       } else {
           // send out a notification for the failed transaction
           NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerTransactionFailedNotification, this, userInfo);
       }
   }
}
```

Una vez que se entrega el producto, `SKPaymentQueue.DefaultQueue.FinishTransaction` debe llamarse para suprimir la transacción de la cola de pago.

### <a name="skpaymenttransactionobserver-custompaymentobserver-methods"></a>Métodos SKPaymentTransactionObserver (CustomPaymentObserver)

Las llamadas de StoreKit el `UpdatedTransactions` método cuando se recibe una respuesta de los servidores de Apple y pasa una matriz de `SKPaymentTransaction` objetos inspeccionar el código. El método recorre cada transacción y realiza una función diferente según el estado de transacción (como se muestra aquí):

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
              theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
              theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

El `CompleteTransaction` método se ha tratado anteriormente en esta sección: guarda los detalles de compra para `NSUserDefaults`, finaliza la transacción con StoreKit y, por último, notifica a la interfaz de usuario para la actualización.

### <a name="purchasing-multiple-products"></a>Comprar varios productos

Si tiene sentido en la aplicación para comprar varios productos, utilice el `SKMutablePayment` clase y establezca el campo Cantidad:

```csharp
public void PurchaseProduct(string appStoreProductId)
{
   SKMutablePayment payment = SKMutablePayment.PaymentWithProduct (appStoreProductId);
   payment.Quantity = 4; // hardcoded as an example
   SKPaymentQueue.DefaultQueue.AddPayment (payment);
}
```

El código que controla la transacción completada también debe consultar la propiedad de la cantidad para completar correctamente la compra:

```csharp
public void CompleteTransaction (SKPaymentTransaction transaction)
{
   var productId = transaction.Payment.ProductIdentifier;
   var qty = transaction.Payment.Quantity;
   if (productId == ConsumableViewController.Buy5ProductId)
       CreditManager.Add(5 * qty);
   else if (productId == ConsumableViewController.Buy10ProductId)
       CreditManager.Add(10 * qty);
   else
       Console.WriteLine ("Shouldn't happen, there are only two products");
   FinishTransaction(transaction, true);
}
```

Cuando un usuario adquiere varias cantidades, la alerta de confirmación StoreKit reflejará la cantidad, el precio por unidad y el precio total que se le cobrará, como se muestra en la captura de pantalla siguiente:

[![Confirmar una compra.](purchasing-consumable-products-images/image30.png)](purchasing-consumable-products-images/image30.png#lightbox)

## <a name="handling-network-outages"></a>Control de interrupciones de red

Adquisición de la aplicación requiere una conexión de red operativa para StoreKit para comunicarse con servidores de Apple. Si una conexión de red no está disponible, a continuación, en la aplicación de compras no estará disponible.

### <a name="product-requests"></a>Solicitudes de producto

Si la red no está disponible al realizar una `SKProductRequest`, el `RequestFailed` método de la `SKProductsRequestDelegate` subclase ( `InAppPurchaseManager`) se llamará, tal como se muestra a continuación:

```csharp
public override void RequestFailed (SKRequest request, NSError error)
{
   using (var pool = new NSAutoreleasePool()) {
       NSDictionary userInfo = NSDictionary.FromObjectsAndKeys(new NSObject[] {error},new NSObject[] {new NSString("error")});
       // send out a notification for the failed transaction
       NSNotificationCenter.DefaultCenter.PostNotificationName (InAppPurchaseManagerRequestFailedNotification, this, userInfo);
   }
}
```

ViewController, a continuación, realiza escuchas para la notificación y muestra un mensaje en los botones de la compra:

```csharp
requestObserver = NSNotificationCenter.DefaultCenter.AddObserver (InAppPurchaseManager.InAppPurchaseManagerRequestFailedNotification,
(notification) => {
   Console.WriteLine ("Request Failed");
   buy5Button.SetTitle ("Network down?", UIControlState.Disabled);
   buy10Button.SetTitle ("Network down?", UIControlState.Disabled);
});
```

Dado que una conexión de red puede ser transitoria en dispositivos móviles, las aplicaciones pueden desea supervisar el estado de la red mediante el marco de trabajo SystemConfiguration y volver a intentar cuando hay disponible una conexión de red. Consulte de Apple o la que lo utiliza.

### <a name="purchase-transactions"></a>Transacciones de compra

La cola de pago StoreKit almacenará y compra directa solicita si es posible, por lo que el efecto de una interrupción de red variará en función de cuando la red durante el proceso de compra.   
   
   
   
 Si se produce un error durante una transacción, el `SKPaymentTransactionObserver` subclase ( `CustomPaymentObserver`) tendrá el `UpdatedTransactions` método llamado y el `SKPaymentTransaction` clase será el estado de error.

```csharp
public override void UpdatedTransactions (SKPaymentQueue queue, SKPaymentTransaction[] transactions)
{
   foreach (SKPaymentTransaction transaction in transactions)
   {
       switch (transaction.TransactionState)
       {
           case SKPaymentTransactionState.Purchased:
               theManager.CompleteTransaction(transaction);
               break;
           case SKPaymentTransactionState.Failed:
               theManager.FailedTransaction(transaction);
               break;
           default:
               break;
       }
   }
}
```

El `FailedTransaction` método detecta si el error fue debido a la cancelación del usuario, como se muestra aquí:

```csharp
public void FailedTransaction (SKPaymentTransaction transaction)
{
   //SKErrorPaymentCancelled == 2
   if (transaction.Error.Code == 2) // user cancelled
       Console.WriteLine("User CANCELLED FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   else // error!
       Console.WriteLine("FailedTransaction Code=" + transaction.Error.Code + " " + transaction.Error.LocalizedDescription);
   FinishTransaction(transaction,false);
}
```

Incluso si se produce un error en una transacción, el `FinishTransaction` método debe llamarse para suprimir la transacción de la cola de pago:

```csharp
SKPaymentQueue.DefaultQueue.FinishTransaction(transaction);
```

El código de ejemplo, a continuación, envía una notificación para que ViewController pueda mostrar un mensaje. Las aplicaciones no deben mostrar adicional del mensaje si el usuario canceló la transacción. Otros códigos de error que pueden producirse incluyen:

```csharp
FailedTransaction Code=0 Cannot connect to iTunes Store
FailedTransaction Code=5002 An unknown error has occurred
FailedTransaction Code=5020 Forget Your Password?
Applications may detect and respond to specific error codes, or handle them in the same way.
```

## <a name="handling-restrictions"></a>Las restricciones del control

El **configuración > General > restricciones** característica de iOS permite a los usuarios bloquear determinadas características de su dispositivo.   
   
   
   
 Puede consultar si se permite al usuario para realizar compras en la aplicación a través de la `SKPaymentQueue.CanMakePayments` método. Si se devuelve false, a continuación, el usuario no puede acceder a compras en la aplicación. StoreKit mostrará automáticamente un mensaje de error al usuario si se intenta realizar una compra. Al comprobar este valor la aplicación en su lugar, puede ocultar los botones de la compra o realizar alguna otra acción para ayudar al usuario.   
   
   
   
 En el `InAppPurchaseManager.cs` archivo la `CanMakePayments` método ajusta la función StoreKit similar al siguiente:

```csharp
public bool CanMakePayments()
{
   return SKPaymentQueue.CanMakePayments;
}
```

Para probar este método, use el **restricciones** característica de iOS para deshabilitar **compras de la aplicación**:   
   
   
   
 [![Use la característica de restricciones de iOS para deshabilitar las compras de la aplicación](purchasing-consumable-products-images/image31.png)](purchasing-consumable-products-images/image31.png#lightbox)   
   
   
   
 Este código de ejemplo de `ConsumableViewController` reacciona a `CanMakePayments` devuelve el valor false al mostrar **AppStore deshabilitado** texto en los botones deshabilitados.

```csharp
// only if we can make payments, request the prices
if (iap.CanMakePayments()) {
   // now go get prices, if we don't have them already
   if (!pricesLoaded)
       iap.RequestProductData(products); // async request via StoreKit -> App Store
} else {
   // can't make payments (purchases turned off in Settings?)
   // the buttons are disabled by default, and only enabled when prices are retrieved
   buy5Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
   buy10Button.SetTitle ("AppStore disabled", UIControlState.Disabled);
}
```

La aplicación aspecto cuando la **compras de la aplicación** característica está restringida, se deshabilitan los botones de la compra.   
   
   
   
 [![La aplicación este aspecto cuando las compras en la aplicación está función restringida la compra se deshabilitan los botones](purchasing-consumable-products-images/image32.png)](purchasing-consumable-products-images/image32.png#lightbox)   
   
   
   

Todavía puede ser la información del producto solicitada cuando `CanMakePayments` es false, por lo que la aplicación todavía puede recuperar y mostrar los precios. Esto significa que si eliminamos el `CanMakePayments` comprobación desde el código de los botones de compra sería estar activa, pero cuando se intenta realizar una compra, el usuario verá un mensaje que **compras de la aplicación no se permiten** (generados por StoreKit Cuando la cola de pago se tiene acceso):   
   
   
   
 [![No se permiten las compras de la aplicación](purchasing-consumable-products-images/image33.png)](purchasing-consumable-products-images/image33.png#lightbox)   
   
   
   
 Aplicaciones del mundo real pueden adoptar un enfoque diferente para controlar la restricción, como ocultar los botones por completo y quizás ofrece un mensaje más detallado de la alerta que StoreKit muestra automáticamente.

