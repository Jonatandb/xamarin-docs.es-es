---
title: Publicación de aplicaciones Xamarin.iOS en el App Store
description: En este documento se describe cómo configurar, compilar y publicar una aplicación Xamarin.iOS para su distribución en el App Store.
ms.prod: xamarin
ms.assetid: DFBCC0BA-D233-4DC4-8545-AFBD3768C3B9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/25/2018
ms.openlocfilehash: c81c84b8b32bdde6949918f3a31f171983007f39
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675224"
---
# <a name="publishing-xamarinios-apps-to-the-app-store"></a>Publicación de aplicaciones Xamarin.iOS en el App Store

Para publicar una aplicación en el [App Store](https://www.apple.com/ios/app-store/), en primer lugar, un desarrollador de aplicaciones debe enviarla junto con capturas de pantalla, una descripción, iconos y otra información a Apple para su revisión. Tras aprobar la aplicación, Apple la coloca en el App Store, donde los usuarios pueden comprarla e instalarla directamente desde sus dispositivos iOS.

En esta guía se describen los pasos que se deben seguir para preparar una aplicación para el App Store y enviarla a Apple para su revisión. En concreto, se describe cómo hacer lo siguiente:

> [!div class="checklist"]
> - Seguir las directrices de revisión del App Store
> - Configurar un identificador de aplicación y los derechos
> - Proporcionar un icono del App Store e iconos de la aplicación
> - Configurar un perfil de aprovisionamiento del App Store
> - Actualizar la configuración de compilación de **versión**
> - Configurar la aplicación en iTunes Connect
> - Compilar la aplicación y enviarla a Apple

> [!IMPORTANT]
> Apple [ha comunicado](https://developer.apple.com/ios/submit/) que, a partir de marzo de 2019, las aplicaciones y actualizaciones que se envíen al App Store deberán haberse compilado con el SDK de iOS 12.1 o posterior, incluido en Xcode 10.1 y versiones posteriores.
> Las aplicaciones también deberán admitir los tamaños de pantalla del iPhone XS y el iPad Pro de 12.9".

## <a name="app-store-guidelines"></a>Directrices del App Store

Antes de enviar una aplicación para su publicación en el App Store, asegúrese de que cumple los estándares definidos en el documento [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html) (Directrices de revisión del App Store) de Apple.
Al enviar una aplicación al App Store, Apple la revisa para asegurarse de que cumple estos requisitos. Si no los cumple Apple la rechazará, de modo que tendrá que solucionar los problemas citados y volver a enviarla.
Por lo tanto, es recomendable que se familiarice con las directrices lo antes posible durante el proceso de desarrollo.

Aquí se muestran un par de cosas que debe tener en cuenta a la hora de enviar una aplicación:

1. Asegúrese de que la descripción de la aplicación coincide con su funcionalidad.
2. Compruebe que la aplicación no se bloquea con un uso normal. Esto incluye el uso en cada dispositivo iOS que admita.

También puede echar un vistazo a los [recursos relacionados con el App Store](https://developer.apple.com/app-store/resources/) que proporciona Apple.

## <a name="set-up-an-app-id-and-entitlements"></a>Configurar un identificador de aplicación y los derechos

Todas las aplicaciones iOS tienen un identificador de aplicación único, que tiene asociado un conjunto de servicios de aplicación, denominados *derechos*. Los derechos permiten que las aplicaciones realicen operaciones diversas, como recibir una notificación de inserción, tener acceso a características de iOS, como HealthKit, y mucho más.

Para crear un identificador de aplicación y seleccionar los derechos necesarios, visite el [Portal para desarrolladores de Apple](https://developer.apple.com/account/) y siga estos pasos:

1. En la sección **Certificados, identificadores y perfiles**, seleccione **Identificadores > Id. de aplicación**.
2. Haga clic en el botón **+** y proporcione un **Nombre** y un **Id. de agrupación** para la nueva aplicación.
3. Desplácese hasta la parte inferior de la pantalla y seleccione cualquier **Servicio de aplicación** que necesitará su aplicación de Xamarin.iOS. Los servicios de aplicación se describen con más detalle en la guía [Trabajar con funcionalidades en Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md).
4. Haga clic en el botón **Continuar** y siga las instrucciones en pantalla para crear el nuevo identificador de aplicación.

Además de seleccionar y configurar los servicios de aplicación necesarios al definir el identificador de aplicación, debe configurar el identificador de aplicación y los derechos en el proyecto de Xamarin.iOS. Para ello, edite los archivos **Info.plist** y **Entitlements.plist**. Para más información, eche un vistazo a la guía [Trabajar con derechos en Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md), en la que se describe cómo se crea un archivo **Entitlements.plist** y qué significan las distintas configuraciones de derechos que contiene.

## <a name="include-an-app-store-icon"></a>Incluir un icono del App Store

Al enviar una aplicación a Apple, asegúrese de que incluye un catálogo de recursos que contenga un icono del App Store. Para más información sobre cómo hacerlo, vea la guía [Iconos del App Store en Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-store-icon.md).

## <a name="set-the-apps-icons-and-launch-screens"></a>Establecer los iconos de aplicaciones y las pantallas de inicio

Para que Apple ponga una aplicación iOS a disposición en el App Store, debe tener iconos y pantallas de inicio adecuados para todos los dispositivos iOS en los que se puede ejecutar. Para más información sobre cómo configurar iconos de aplicación y pantallas de inicio, lea las guías siguientes:

- [Iconos de aplicación en Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Pantallas de inicio para aplicaciones Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)

## <a name="create-and-install-an-app-store-provisioning-profile"></a>Crear e instalar un perfil de aprovisionamiento del App Store

iOS usa *perfiles de aprovisionamiento* para controlar cómo puede implementarse una compilación de aplicación determinada. Estos son archivos que contienen información sobre el certificado que se ha usado para firmar una aplicación, el identificador de aplicación y el lugar en el que puede instalarse la aplicación. Para el desarrollo y la distribución ad hoc, el perfil de aprovisionamiento también incluye la lista de dispositivos permitidos en los que se puede implementar la aplicación. En cambio, para la distribución en el App Store, solo se incluye información del identificador de aplicación y del certificado, ya que el único mecanismo para la distribución pública es el App Store.

Para crear e instalar un perfil de aprovisionamiento del App Store, siga estos pasos:

1. Inicie sesión en el [Portal para desarrolladores de Apple](https://developer.apple.com/account/).
2. En **Certificados, identificadores y perfiles**, seleccione **Perfiles de aprovisionamiento > Distribución**.
3. Haga clic en el botón **+** , seleccione **App Store** y haga clic en **Continuar**.
4. Seleccione el **Identificador de aplicación** en la lista y haga clic en **Continuar**.
5. Seleccione un certificado de firma y haga clic en **Continuar**.
6. Escriba un **Nombre de perfil** y haga clic en **Continuar** para generar el perfil.
7. Use las herramientas de [administración de cuentas de Apple](~/cross-platform/macios/apple-account-management.md) de Xamarin para descargar el perfil de aprovisionamiento recién creado en el equipo Mac. Si está trabajando con un equipo Mac, también puede descargar el perfil de aprovisionamiento directamente desde el Portal para desarrolladores de Apple y hacer doble clic en él para instalarlo.

Para obtener instrucciones detalladas, vea [Creación de un perfil de distribución](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioningprofile) y [Selección de un perfil de distribución en un proyecto de Xamarin.iOS](~/ios/deploy-test/app-distribution/app-store-distribution/index.md#selectprofile).

## <a name="update-the-release-build-configuration"></a>Actualizar la configuración de compilación de versión

Los nuevos proyectos Xamarin.iOS se establecen automáticamente en las _configuraciones de compilación_ de **depuración** y **versión**. Para configurar correctamente la compilación de **versión**, siga estos pasos:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

1. Desde el **Panel de solución**, abra **Info.plist**. Seleccione **Aprovisionamiento manual**. Guarde y cierre el archivo.
2. Haga clic con el botón derecho en el **nombre del proyecto** en el **Panel de solución**, seleccione **Opciones** y vaya a la pestaña **Compilación de iOS**.
3. Establezca **Configuración** en **Versión** y **Plataforma** en **iPhone**.
4. Para compilar con un SDK de iOS específico, selecciónelo en la lista **Versión del SDK**. En caso contrario, deje este valor en **Predeterminado**.
5. La vinculación reduce el tamaño total de la aplicación, ya que elimina el código no usado. En la mayoría de los casos, la opción **Comportamiento del enlazador** debe establecerse en el valor predeterminado **Vincular solo SDK de marco**. En algunos casos, como cuando se usan bibliotecas de terceros, puede ser necesario establecer este valor en **No vincular** para asegurarse de que no se quita el código necesario. Para obtener más información, vea la guía [Vincular aplicaciones Xamarin.iOS](~/ios/deploy-test/linker.md).
6. Active **Optimizar imágenes PNG** para reducir aún más el tamaño de la aplicación.
7. La depuración _no_ debe habilitarse porque hará que la compilación sea innecesariamente grande.
8. En el caso de iOS 11, seleccione una de las arquitecturas de dispositivo que admite **ARM64**. Para obtener más información sobre la compilación de dispositivos iOS de 64 bits, vea la sección **Habilitar compilaciones de 64 bits de aplicaciones Xamarin.iOS** de la documentación [Consideraciones sobre las plataformas de 32 y 64 bits](~/cross-platform/macios/32-and-64/index.md).
9. Tal vez le interese usar el compilador **LLVM** para compilar código más rápido y más pequeño, pero esta opción aumenta los tiempos de compilación.
10. En función de las necesidades de la aplicación, puede que también quiera ajustar el tipo de **Recolección de elementos no utilizados** que se está usando y la configuración de **Internacionalización**.

    Después de establecer las opciones descritas anteriormente, la configuración de compilación debería ser similar al siguiente:

    ![Configuración de compilación de iOS](publishing-to-the-app-store-images/build-m157.png "Configuración de compilación de iOS")

    Consulte también la guía [Mecanismos de compilación de iOS](~/ios/deploy-test/ios-build-mechanics.md), en la que se describe con más detalle la configuración de compilación.

11. Vaya a la pestaña **Firma del lote de iOS**. Si estas opciones no son editables, asegúrese de que esté seleccionado **Aprovisionamiento manual** en el archivo **Info.plist**.
12. Asegúrese de que **Configuración** esté establecido en **Versión** y **Plataforma** en **iPhone**.
13. Establezca **Identidad de firma** en **Distribución (automática)** .
14. Para **Perfil de aprovisionamiento**, seleccione el perfil de aprovisionamiento del App Store [creado anteriormente](#create-and-install-an-app-store-provisioning-profile).

    Las opciones de firma del lote del proyecto deberían parecerse a lo siguiente:

    ![Firma del lote de iOS](publishing-to-the-app-store-images/bundleSigning-m157.png "Firma del lote de iOS")

15. Haga clic en **Aceptar** para guardar los cambios en las propiedades del proyecto.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Asegúrese de que Visual Studio 2019 o Visual Studio 2017 están [emparejados con un host de compilación de Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Haga clic con el botón derecho en el **nombre del proyecto** en el **Explorador de soluciones** y luego seleccione **Propiedades**.
3. Vaya a la pestaña **Compilación de iOS** y establezca **Configuración** en **Versión** y **Plataforma** en **iPhone**.
4. Para compilar con un SDK de iOS específico, selecciónelo en la lista **Versión del SDK**. En caso contrario, deje este valor en **Predeterminado**.
5. La vinculación reduce el tamaño total de la aplicación, ya que elimina el código no usado. En la mayoría de los casos, la opción **Comportamiento del enlazador** debe establecerse en el valor predeterminado **Vincular solo SDK de marco**. En algunos casos, como cuando se usan bibliotecas de terceros, puede ser necesario establecer este valor en **No vincular** para asegurarse de que no se quita el código necesario. Para obtener más información, vea la guía [Vincular aplicaciones Xamarin.iOS](~/ios/deploy-test/linker.md).
6. Active **Optimizar imágenes PNG** para reducir aún más el tamaño de la aplicación.
7. La depuración no debe habilitarse porque hará que la compilación sea innecesariamente grande.
8. En el caso de iOS 11, seleccione una de las arquitecturas de dispositivo que admite **ARM64**. Para obtener más información sobre la compilación de dispositivos iOS de 64 bits, vea la sección **Habilitar compilaciones de 64 bits de aplicaciones Xamarin.iOS** de la documentación [Consideraciones sobre las plataformas de 32 y 64 bits](~/cross-platform/macios/32-and-64/index.md).
9. Tal vez le interese usar el compilador **LLVM** para compilar código más rápido y más pequeño, pero esta opción aumenta los tiempos de compilación.
10. En función de las necesidades de la aplicación, puede que también quiera ajustar el tipo de **Recolección de elementos no utilizados** que se está usando y la configuración de **Internacionalización**.

    Después de establecer las opciones descritas anteriormente, la configuración de compilación debería ser similar al siguiente:

    ![Configuración de compilación de iOS](publishing-to-the-app-store-images/build-w157.png "Configuración de compilación de iOS")

    Consulte también la guía [Mecanismos de compilación de iOS](~/ios/deploy-test/ios-build-mechanics.md), en la que se describe con más detalle la configuración de compilación.

11. Vaya a la pestaña **Firma del lote de iOS**. Asegúrese de que **Configuración** esté establecido en **Versión** y **Plataforma** en **iPhone** y de que la opción **Aprovisionamiento manual** está seleccionada.
12. Establezca **Identidad de firma** en **Distribución (automática)** .
13. Para **Perfil de aprovisionamiento**, seleccione el perfil de aprovisionamiento del App Store [creado anteriormente](#create-and-install-an-app-store-provisioning-profile).

    Las opciones de firma del lote del proyecto deberían parecerse a lo siguiente:

    ![Configuración de firma del lote de iOS](publishing-to-the-app-store-images/bundleSigning-w157.png "Configuración de firma del lote de iOS")

14. Vaya a la pestaña **Opciones de IPA de iOS**.
15. Asegúrese de que **Configuración** esté establecido en **Versión** y **Plataforma** en **iPhone**.
16. Active la casilla **Compilar archivo de paquete de iTunes (IPA)** . Esta configuración hará que cada compilación de **versión** (ya que es la configuración seleccionada) genere un archivo .ipa. Este archivo se puede enviar a Apple para la publicación en el App Store.

    > [!NOTE]
    > No hacen falta **metadatos de iTunes** ni **ilustraciones de iTunes** para las versiones del App Store. Para obtener más información, vea [Archivo iTunesMetadata.plist en aplicaciones Xamarin.iOS](~/ios/deploy-test/app-distribution/itunesmetadata.md) e [Ilustraciones de iTunes](~/ios/app-fundamentals/images-icons/app-icons.md#itunes-artwork).

17. Para especificar un nombre de archivo .ipa que difiere del nombre del proyecto de Xamarin.iOS, escríbalo en el campo **Nombre del paquete**.

    ![Configuración de firma del lote de iOS](publishing-to-the-app-store-images/ipaOptions-w157.png "Configuración de firma del lote de iOS")

18. Guarde la configuración de compilación y ciérrela.

-----

## <a name="configure-your-app-in-itunes-connect"></a>Configurar la aplicación en iTunes Connect

[iTunes Connect](https://itunesconnect.apple.com) es un conjunto de aplicaciones de herramientas basadas en web para administrar aplicaciones iOS en el App Store. La aplicación Xamarin.iOS debe estar correctamente configurada en iTunes Connect para que se pueda enviar a Apple para su revisión y publicación en el App Store.

Para obtener información sobre cómo hacerlo, lea la guía [Configuración de una aplicación en iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

## <a name="build-and-submit-your-app"></a>Compilar y enviar la aplicación

Con la configuración de compilación establecida correctamente y iTunes Connect a la espera de su envío, ahora puede compilar la aplicación y enviarla a Apple.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos)

1. En Visual Studio para Mac, seleccione la configuración de compilación de **versión** y un dispositivo (no un simulador) para el que se va a compilar.

    ![Selección de configuración de compilación y plataforma](publishing-to-the-app-store-images/chooseConfig-m157.png "Selección de configuración de compilación y plataforma")

2. En el menú **Compilación**, seleccione **Archive for Publishing** (Archivo para publicar).
3. Una vez creado el archivo, se mostrará la vista **Archives (Archivos)** :

    ![Vista de archivos](publishing-to-the-app-store-images/archives-m157.png "Vista de archivos")

    > [!NOTE]
    > De forma predeterminada, la vista de **Archivos** solo muestra los archivos de la solución abierta. Para ver todas las soluciones que tienen archivos, active la casilla **Show all archives** (Mostrar todos los archivos). Es recomendable conservar los archivos antiguos para que la información de depuración que incluyen se puede usar para resolver los símbolos de los informes de bloqueo, si es necesario.

4. Haga clic en **Sign and Distribute…** (Firmar y distribuir…) para abrir el Asistente para la publicación.
5. Seleccione el canal de distribución del **App Store**. Haga clic en **Siguiente**.

    ![Selección del canal de distribución](publishing-to-the-app-store-images/distChannel-m157.png "Selección del canal de distribución")

6. En la ventana **Perfil de aprovisionamiento**, seleccione su identidad de firma, la aplicación y el perfil de aprovisionamiento. Haga clic en **Siguiente**.

    ![Selección del perfil de aprovisionamiento](publishing-to-the-app-store-images/provProfileSelect-m157.png "Selección del perfil de aprovisionamiento")

7. Compruebe los detalles del paquete y haga clic en **Publicar** para guardar un paquete .ipa para la aplicación:

    ![Comprobación de los detalles de la aplicación](publishing-to-the-app-store-images/publish-m157.png "Comprobación de los detalles de la aplicación")

8. Una vez que se haya guardado el archivo .ipa, la aplicación está lista para cargarse en iTunes Connect.

    ![Listo para el envío](publishing-to-the-app-store-images/readyToGo-m157.png "Listo para el envío")

9. Haga clic en **Abrir Application Loader** e inicie sesión (tenga en cuenta que debe [crear una contraseña específica de la aplicación](https://support.apple.com/ht204397) para el identificador de Apple).

    > [!NOTE]
    > Para obtener más información sobre la herramienta, consulte la [documentación de Apple sobre Application Loader](https://help.apple.com/itc/apploader/#/apdS673accdb).

10. Seleccione **Deliver Your App** (Entregar la aplicación) y haga clic en el botón **Elegir**:

    ![Seleccione Deliver Your App (Entregar la aplicación)](publishing-to-the-app-store-images/publishvs01.png "Seleccione Deliver Your App (Entregar la aplicación)")

11. Seleccione el archivo .ipa que ha creado anteriormente y haga clic en el botón **Aceptar**.
12. Application Loader validará el archivo:

    ![Pantalla de validación](publishing-to-the-app-store-images/publishvs02.png "Pantalla de validación")

13. Haga clic en el botón **Siguiente** y la aplicación se validará en el App Store:

    ![Validación en el App Store](publishing-to-the-app-store-images/publishvs03.png "Validación en el App Store")

14. Haga clic en el botón **Enviar** para enviar la aplicación a Apple para su revisión.
15. Application Loader le informará cuando el archivo se haya cargado correctamente.

    > [!NOTE]
    > Apple puede rechazar aplicaciones con el archivo **iTunesMetadata.plist** incluido en el archivo .ipa, lo que produce un error como el siguiente:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Para encontrar una solución a este error, eche un vistazo a [esta publicación en los foros de Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Visual Studio 2017 no admite actualmente el flujo de trabajo **Archive for Publishing** (Archivo para la publicación) que se encuentra en Visual Studio para Mac.

1. Asegúrese de que Visual Studio 2019 o Visual Studio 2017 están [emparejados con un host de compilación de Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Seleccione **Versión** en la lista desplegable **Configuraciones de soluciones** de Visual Studio 2017 y **iPhone** en la lista desplegable **Plataformas de solución**.

    ![Selección de configuración de compilación y plataforma](publishing-to-the-app-store-images/chooseConfig-w157.png "Selección de configuración de compilación y plataforma")

3. Compile el proyecto. Esto crea un archivo .ipa.

    > [!NOTE]
    > En la sección [Actualizar la configuración de compilación de versión](#update-the-release-build-configuration) de este documento se han configurado las opciones de compilación de la aplicación para crear un archivo .ipa para cada compilación de **versión**.

4. Para buscar el archivo .ipa en el equipo Windows, haga clic con el botón derecho en el nombre del proyecto de Xamarin.iOS en el **Explorador de soluciones** de Visual Studio 2019 o Visual Studio 2017 y seleccione **Abrir la carpeta en el Explorador de archivos**. Después, en el **Explorador de archivos** de Windows que se acaba de abrir, vaya al subdirectorio **bin/iPhone/Release**. A menos que haya [personalizado la ubicación de salida del archivo .ipa](#customize-the-ipa-location), debería estar en este directorio.
5. Para ver el archivo .ipa en el host de compilación de Mac, haga clic con el botón derecho en el nombre del proyecto de Xamarin.iOS en el **Explorador de soluciones** de Visual Studio 2019 o Visual Studio 2017 (en Windows) y seleccione **Mostrar archivo IPA en servidor de compilación**. Se abrirá una ventana del **Finder** en el host de compilación de Mac con el archivo .ipa seleccionado.
6. En el host de compilación de Mac, abra el **cargador de aplicaciones**. En Xcode, seleccione **Xcode > Open Developer Tool (Abrir la herramienta para desarrolladores) > Application Loader (Cargador de aplicaciones)** .

    > [!NOTE]
    > Para obtener más información sobre la herramienta, consulte la [documentación de Apple sobre Application Loader](https://help.apple.com/itc/apploader/#/apdS673accdb).

7. Inicie sesión en el cargador de aplicaciones (tenga en cuenta que debe [crear una contraseña específica de la aplicación](https://support.apple.com/ht204397) para el identificador de Apple).
8. Seleccione **Deliver Your App** (Entregar la aplicación) y haga clic en el botón **Elegir**:

    ![Seleccione Deliver Your App (Entregar la aplicación)](publishing-to-the-app-store-images/publishvs01.png "Seleccione Deliver Your App (Entregar la aplicación)")

9. Seleccione el archivo .ipa que ha creado anteriormente y haga clic en **Aceptar**.
10. Application Loader validará el archivo:

    ![Pantalla de validación](publishing-to-the-app-store-images/publishvs02.png "Pantalla de validación")

11. Haga clic en el botón **Siguiente** y la aplicación se validará en el App Store:

    ![Validación en el App Store](publishing-to-the-app-store-images/publishvs03.png "Validación en el App Store")

12. Haga clic en el botón **Enviar** para enviar la aplicación a Apple para su revisión.
13. Application Loader le informará cuando el archivo se haya cargado correctamente.

    > [!NOTE]
    > Apple puede rechazar aplicaciones con el archivo **iTunesMetadata.plist** incluido en el archivo .ipa, lo que produce un error como el siguiente:
    >
    > `ERROR: ERROR ITMS-90047: "Disallowed paths ( "iTunesMetadata.plist" ) found at: Payload/iPhoneApp1.app"`
    >
    > Para encontrar una solución a este error, eche un vistazo a [esta publicación en los foros de Xamarin](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1).

-----

## <a name="itunes-connect-status"></a>Estado de iTunes Connect

Para ver el estado del envío de la aplicación, inicie sesión en iTunes Connect y seleccione la aplicación. El estado inicial debería ser **Waiting For Review** (En espera de revisión), aunque podría aparecer temporalmente **Upload Received** (Carga recibida) mientras se está procesando.

![En espera de revisión](publishing-to-the-app-store-images/image21.png "En espera de revisión")

## <a name="tips-and-tricks"></a>Sugerencias y trucos

### <a name="customize-the-ipa-location"></a>Personalización de la ubicación del archivo .ipa

Una propiedad de **MSBuild**, `IpaPackageDir`, hace que sea posible personalizar la ubicación de salida del archivo .ipa. Si `IpaPackageDir` está establecido en una ubicación personalizada, el archivo .ipa se colocará en esa ubicación en lugar de en el subdirectorio predeterminado con marca de tiempo. Esto puede ser útil durante la creación automatizada de compilaciones basadas en una ruta de acceso del directorio específica para funcionar correctamente, como las utilizadas para las compilaciones de integración continua (CI).

Hay varias formas de usar la nueva propiedad. Por ejemplo, para la salida del archivo .ipa en el directorio predeterminado anterior (como en Xamarin.iOS 9.6 y versiones anteriores), puede establecer la propiedad `IpaPackageDir` en `$(OutputPath)` mediante uno de los métodos siguientes. Ambos métodos son compatibles con todas las compilaciones de Xamarin.iOS de Unified API, incluidas las compilaciones del IDE, así como las de la línea de comandos en las que se usa **msbuild** o **mdtool**:

- La primera opción se basa en establecer la propiedad `IpaPackageDir` de un elemento `<PropertyGroup>` en un archivo **MSBuild**. Por ejemplo, puede agregar el siguiente elemento `<PropertyGroup>` en la parte inferior del archivo .csproj del proyecto de la aplicación de iOS, justo antes de la etiqueta de cierre `</Project>`:

    ```xml
    <PropertyGroup>
      <IpaPackageDir>$(OutputPath)</IpaPackageDir>
    </PropertyGroup>
    ```

- Un método mejor consiste en agregar un elemento `<IpaPackageDir>` en la parte inferior del elemento `<PropertyGroup>` existente que corresponde a la configuración usada para compilar el archivo .ipa. Esta opción es mejor porque permite preparar el proyecto para la futura compatibilidad con una configuración prevista en la página de propiedades del proyecto iOS IPA Options (Opciones de IPA para iOS). Si actualmente usa la configuración de `Release|iPhone` para compilar el archivo .ipa, el grupo completo de propiedades actualizadas puede ser similar al siguiente:

    ```xml
    <PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Release|iPhone'">
       <Optimize>true</Optimize>
       <OutputPath>bin\iPhone\Release</OutputPath>
       <ErrorReport>prompt</ErrorReport>
       <WarningLevel>4</WarningLevel>
       <ConsolePause>false</ConsolePause>
       <CodesignKey>iPhone Developer</CodesignKey>
       <MtouchUseSGen>true</MtouchUseSGen>
       <MtouchUseRefCounting>true</MtouchUseRefCounting>
       <MtouchFloat32>true</MtouchFloat32>
       <CodesignEntitlements>Entitlements.plist</CodesignEntitlements>
       <MtouchLink>SdkOnly</MtouchLink>
       <MtouchArch>;ARMv7, ARM64</MtouchArch>
       <MtouchHttpClientHandler>HttpClientHandler</MtouchHttpClientHandler>
       <MtouchTlsProvider>Default</MtouchTlsProvider>
       <PlatformTarget>x86&</PlatformTarget>
       <BuildIpa>true</BuildIpa>
       <IpaPackageDir>$(OutputPath</IpaPackageDir>
    </PropertyGroup>
    ```

Una técnica alternativa para las compilaciones de las líneas de comandos **msbuild** consiste en agregar un argumento de línea de comandos `/p:` para establecer la propiedad `IpaPackageDir`. Observe que, en este caso, **msbuild** no expande expresiones `$()` que pasan a la línea de comandos, por lo que no es posible utilizar la sintaxis `$(OutputPath)`. En su lugar, debe proporcionar un nombre completo de la ruta de acceso.

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:ServerAddress="192.168.1.3" /p:ServerUser="macuser" /p:IpaPackageDir="%USERPROFILE%\Builds" /t:Build SingleViewIphone1.sln
```

O al siguiente en Mac:

```bash
msbuild /p:Configuration="Release" /p:Platform="iPhone" /p:IpaPackageDir="$HOME/Builds" /t:Build SingleViewIphone1.sln
```

Una vez que se ha creado y archivado su compilación de distribución, ahora está listo para enviar su aplicación a iTunes Connect.

## <a name="summary"></a>Resumen

En este artículo se describe cómo configurar, compilar y enviar una aplicación iOS para su publicación en el App Store.

## <a name="related-links"></a>Vínculos relacionados

- [Portal para desarrolladores de Apple (Apple)](https://developer.apple.com/account/)
- [iTunes Connect (Apple)](https://itunesconnect.apple.com)
- [Directrices de revisión del App Store (Apple)](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Rechazos de aplicaciones comunes (Apple)](https://developer.apple.com/app-store/review/rejections/)
- [Trabajar con funcionalidades en Xamarin.iOS](~/ios/deploy-test/provisioning/capabilities/index.md)
- [Trabajar con derechos en Xamarin.iOS](~/ios/deploy-test/provisioning/entitlements.md)
- [Configuración de una aplicación en iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Iconos de aplicación en Xamarin.iOS](~/ios/app-fundamentals/images-icons/app-icons.md)
- [Pantallas de inicio para aplicaciones Xamarin.iOS](~/ios/app-fundamentals/images-icons/launch-screens.md)
- [Documentación de Application Loader (Apple)](https://help.apple.com/itc/apploader/#/apdS673accdb)
