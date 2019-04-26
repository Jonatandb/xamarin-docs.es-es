---
title: Voz de Android
description: En este artículo se trata los aspectos básicos del uso del espacio de nombres de Android.Speech muy eficaz. Desde sus inicios, Android ha sido capaz de reconocer la voz y pasar los resultados como texto. Es un proceso relativamente sencillo. Para texto a voz, sin embargo, el proceso es más complicado, ya no solo el motor de voz tiene que tener en cuenta, pero también los idiomas disponibles e instalados en el sistema de texto a voz (TTS).
ms.prod: xamarin
ms.assetid: FA3B8EC4-34D2-47E3-ACEA-BD34B28115B9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/02/2018
ms.openlocfilehash: e88f6e24cbf4c8b2f0c0486c6408e234e87066cc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228599"
---
# <a name="android-speech"></a>Voz de Android

_En este artículo se trata los aspectos básicos del uso del espacio de nombres de Android.Speech muy eficaz. Desde sus inicios, Android ha sido capaz de reconocer la voz y pasar los resultados como texto. Es un proceso relativamente sencillo. Para texto a voz, sin embargo, el proceso es más complicado, ya no solo el motor de voz tiene que tener en cuenta, pero también los idiomas disponibles e instalados en el sistema de texto a voz (TTS)._

## <a name="speech-overview"></a>Información general de voz

Tener un sistema que "comprende" voz humana y enunciates lo que se escribe, conversión de voz en texto y texto a voz, es un área siempre creciente de desarrollo para dispositivos móviles como aumenta la demanda de comunicación naturales con nuestros dispositivos. Existen muchas instancias donde tiene una característica que convierte el texto en voz, o viceversa, es una herramienta muy útil para incorporar a su aplicación android.

Por ejemplo, con abrazadera hacia abajo en el uso de teléfono móvil mientras conducen, los usuarios desean una forma de manos libres de funcionamiento de sus dispositivos. La gran cantidad de factores de forma diferente de Android, como Android Wear, y la inclusión nunca de ampliación de los que puede usar dispositivos Android (como tabletas y blocs de notas), ha creado un mayor foco en excelentes aplicaciones TTS.

Google Proporciona al desarrollador un amplio conjunto de API en el espacio de nombres Android.Speech para cubrir la mayoría de las instancias de hacer que un dispositivo "compatibles con voz" (por ejemplo, un software diseñado for the blind).  El espacio de nombres incluye la capacidad para permitir que se traduzca voz a través de texto `Android.Speech.Tts`, control sobre el motor usado para realizar la traducción, así como un número de `RecognizerIntent`s, que permiten que se puede convertir en texto de voz.

Aunque existen las funciones de voz a entender, hay limitaciones sobre el hardware utilizado. No es probable que el dispositivo interpretar correctamente todo el contenido se habla de todos los idiomas disponibles.

## <a name="requirements"></a>Requisitos

No hay ningún requisito especial para esta guía, que no sean el dispositivo que tiene un micrófono y altavoces.

El núcleo de un dispositivo Android interpretación de voz es el uso de un `Intent` con su correspondiente `OnActivityResult`.
Sin embargo, es importante reconocer que no se entiende la voz, pero interpretadas al texto. La diferencia es importante.

### <a name="the-difference-between-understanding-and-interpreting"></a>La diferencia entre comprender e interpretar

Una definición simple de conocimiento es que es capaz de determinar el verdadero significado de lo que se dice tono y contexto. Para interpretar simplemente significa tomar las palabras y en otro formato de salida de ellos.

Considere el siguiente ejemplo simple que se utiliza en todos los días conversación: 

<kbd>¿Hola cómo estás?</kbd>

Sin inflexión (énfasis en palabras específicas o partes de palabras), es una pregunta sencilla. Sin embargo, si se aplica un ritmo lento a la línea, la persona a la escucha detectará que la persona que preguntó no es demasiado feliz y quizás necesita cheering o que la persona que preguntó es encontrarse mal. Si se coloca el énfasis en "son", la persona que le pregunta es normalmente más interesada en la respuesta.

Sin audio bastante eficaces que hacen uso de la inflexión y un grado de inteligencia artificial (IA) para entender el contexto, el software incluso no puede empezar a entender lo que decía: lo mejor que puede hacer una sencilla de teléfono se convierta la voz en texto.

## <a name="setting-up"></a>Configuración

Antes de usar el sistema de voz, siempre es conveniente comprobar para asegurarse de que el dispositivo tiene un micrófono. Sería mucho sentido intentar ejecutar la aplicación en un bloc de notas Kindle o Google sin un micrófono instalado.

El ejemplo de código siguiente muestra la consulta si hay un micrófono disponible y si no, para crear una alerta. Si ningún micrófono está disponible en este momento se podría salir de la actividad o deshabilitar la capacidad de grabar la voz.

```csharp
string rec = Android.Content.PM.PackageManager.FeatureMicrophone;
if (rec != "android.hardware.microphone")
{
    var alert = new AlertDialog.Builder(recButton.Context);
    alert.SetTitle("You don't seem to have a microphone to record with");
    alert.SetPositiveButton("OK", (sender, e) =>
    {
        return;
    });
    alert.Show();
}
```

### <a name="creating-the-intent"></a>Creación de la intención

La intención de que el sistema de voz usa un tipo determinado de intención llama el `RecognizerIntent`. Esta intención controla un gran número de parámetros, incluidos ¿durante cuánto tiempo debe para esperar en silencio hasta que se considera la grabación, sobre los idiomas adicionales para reconocer y de salida y cualquier texto que se incluyen en el `Intent`del cuadro de diálogo modal como medio de la instrucción. En este fragmento de código, `VOICE` es un `readonly int` usado para reconocimiento en `OnActivityResult`.

```csharp
var voiceIntent = new Intent(RecognizerIntent.ActionRecognizeSpeech);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguageModel, RecognizerIntent.LanguageModelFreeForm);
voiceIntent.PutExtra(RecognizerIntent.ExtraPrompt, Application.Context.GetString(Resource.String.messageSpeakNow));
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputPossiblyCompleteSilenceLengthMillis, 1500);
voiceIntent.PutExtra(RecognizerIntent.ExtraSpeechInputMinimumLengthMillis, 15000);
voiceIntent.PutExtra(RecognizerIntent.ExtraMaxResults, 1);
voiceIntent.PutExtra(RecognizerIntent.ExtraLanguage, Java.Util.Locale.Default);
StartActivityForResult(voiceIntent, VOICE);
```

### <a name="conversion-of-the-speech"></a>Conversión de voz

El texto que se interpreta de la voz se entregará en la `Intent`, que se devuelve cuando la actividad se ha completado y se accede a través de `GetStringArrayListExtra(RecognizerIntent.ExtraResults)`. Esto devolverá un `IList<string>`, de que el índice puede usarse y muestra según el número de idiomas solicitados en la intención del autor de llamada (y se especifica en el `RecognizerIntent.ExtraMaxResults`). Como con cualquier lista no obstante, merece la pena comprobaciones para asegurarse de que no hay datos que se mostrarán.

Cuando la escucha para el valor devuelto de un `StartActivityForResult`, el `OnActivityResult` método tiene que proporcionarse.

En el ejemplo siguiente, `textBox` es un `TextBox` utiliza para obtener lo que ha sido dictado. Se podría usar igualmente para pasar el texto a alguna forma de intérprete y desde allí, la aplicación puede comparar el texto y la rama a otra parte de la aplicación.

```csharp
protected override void OnActivityResult(int requestCode, Result resultVal, Intent data)
{
    if (requestCode == VOICE)
    {
        if (resultVal == Result.Ok)
        {
            var matches = data.GetStringArrayListExtra(RecognizerIntent.ExtraResults);
            if (matches.Count != 0)
            {
                string textInput = textBox.Text + matches[0];
                textBox.Text = textInput;
                switch (matches[0].Substring(0, 5).ToLower())
                {
                    case "north":
                        MovePlayer(0);
                        break;
                    case "south":
                        MovePlayer(1);
                        break;
                }
            }
            else
            {
                textBox.Text = "No speech was recognised";
            }
        }
        base.OnActivityResult(requestCode, resultVal, data);
    }
}
```

## <a name="text-to-speech"></a>Texto a voz

Texto a voz no es bastante el inverso de voz a texto y se basa en dos componentes clave; que se instala un motor de texto a voz en el dispositivo y un idioma que se va a instalar.

En gran medida, los dispositivos Android incluyen el valor predeterminado instalado el servicio de Google TTS y al menos un idioma. Esto se establece cuando el dispositivo está configurado en primer lugar y se basará en donde el dispositivo está en el momento (por ejemplo, un teléfono configurado en Alemania instalará el idioma alemán, mientras que uno en Estados Unidos tendrán inglés de Estados Unidos).

### <a name="step-1---instantiating-texttospeech"></a>Paso 1: crear instancias de TextToSpeech

`TextToSpeech` puede tardar hasta 3 parámetros, los dos primeros son necesarios con el tercero que es opcional (`AppContext`, `IOnInitListener`, `engine`). El agente de escucha se utiliza para enlazar con el servicio y la prueba de error con el motor que se va a cualquier número de motores de texto a voz Android disponibles. Como mínimo, el dispositivo tendrá el motor de Google.

### <a name="step-2---finding-the-languages-available"></a>Paso 2: buscar los idiomas disponibles

El `Java.Util.Locale` clase contiene un método útil llamado `GetAvailableLocales()`. A continuación, puede probar esta lista de idiomas admitidos por el motor de voz en los idiomas instalados.

Es un asunto trivial para generar la lista de idiomas "entiende". Siempre habrá un idioma predeterminado (el idioma al usuario establece al primero configurar su dispositivo), por tanto, en este ejemplo el `List<string>` tiene "Default" como primer parámetro, el resto de la lista se rellena según el resultado de la `textToSpeech.IsLanguageAvailable(locale)`.

```csharp
var langAvailable = new List<string>{ "Default" };
var localesAvailable = Java.Util.Locale.GetAvailableLocales().ToList();
foreach (var locale in localesAvailable)
{
    var res = textToSpeech.IsLanguageAvailable(locale);
    switch (res)
    {
        case LanguageAvailableResult.Available:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
        case LanguageAvailableResult.CountryVarAvailable:
          langAvailable.Add(locale.DisplayLanguage);
          break;
    }
}
langAvailable = langAvailable.OrderBy(t => t).Distinct().ToList();
```

Este código llama a [TextToSpeech.IsLanguageAvailable](https://developer.xamarin.com/api/member/Android.Speech.Tts.TextToSpeech.IsLanguageAvailable/p/Java.Util.Locale/) para probar si el paquete de idioma para una configuración regional especificada ya está presente en el dispositivo. Este método devuelve un [LanguageAvailableResult](https://developer.xamarin.com/api/type/Android.Speech.Tts.LanguageAvailableResult/), lo que indica si está disponible el idioma de la configuración regional pasada. Si `LanguageAvailableResult` indica que el idioma es `NotSupported`, entonces no hay ningún paquete de voz disponibles (incluso para descarga) para dicho idioma. Si `LanguageAvailableResult` está establecido en `MissingData`, es posible descargar un nuevo paquete de idioma, tal como se explica más adelante en el paso 4.

### <a name="step-3---setting-the-speed-and-pitch"></a>Paso 3: establecer la velocidad y el tono

Android permite al usuario modificar el sonido de la voz modificando el `SpeechRate` y `Pitch` (la velocidad de la velocidad y el tono de voz). Esto va de 0 a 1, con la voz "normal" que se va a 1 para ambos.

### <a name="step-4---testing-and-loading-new-languages"></a>Paso 4: probar y cargar nuevos idiomas

Descargar un nuevo lenguaje se realiza mediante un `Intent`. Hace que el resultado de esta intención el [OnActivityResult](https://developer.xamarin.com/api/member/Android.App.Activity.OnActivityResult/) invocar método. A diferencia del ejemplo de texto a voz (que usa el [RecognizerIntent](https://developer.xamarin.com/api/type/Android.Speech.RecognizerIntent/) como un `PutExtra` parámetro para el `Intent`), las pruebas y la carga `Intent`son `Action`-según:

-   [TextToSpeech.Engine.ActionCheckTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionCheckTtsData/) &ndash; inicia una actividad desde la plataforma `TextToSpeech` motor para comprobar la correcta instalación y la disponibilidad de recursos de idioma en el dispositivo.

-   [TextToSpeech.Engine.ActionInstallTtsData](https://developer.xamarin.com/api/field/Android.Speech.Tts.TextToSpeech+Engine.ActionInstallTtsData/) &ndash; inicia una actividad que solicita al usuario que descargue los idiomas necesarios.

En el ejemplo de código siguiente se muestra cómo usar estas acciones para comprobar si hay recursos de idioma y descargue un nuevo lenguaje de:

```csharp
var checkTTSIntent = new Intent();
checkTTSIntent.SetAction(TextToSpeech.Engine.ActionCheckTtsData);
StartActivityForResult(checkTTSIntent, NeedLang);
//
protected override void OnActivityResult(int req, Result res, Intent data)
{
    if (req == NeedLang)
    {
        var installTTS = new Intent();
        installTTS.SetAction(TextToSpeech.Engine.ActionInstallTtsData);
        StartActivity(installTTS);
    }
}
```

`TextToSpeech.Engine.ActionCheckTtsData` comprueba la disponibilidad de recursos de idioma. `OnActivityResult` se invoca cuando se complete esta prueba. Si necesitan recursos de idioma para descargarse, `OnActivityResult` desactiva el `TextToSpeech.Engine.ActionInstallTtsData` para iniciar una actividad que permite al usuario que descargue los idiomas necesarios. Tenga en cuenta que este `OnActivityResult` implementación no comprueba la `Result` código, ya que, en este ejemplo simplificado, la determinación ya está decidida que es necesario descargar el paquete de idioma.

El `TextToSpeech.Engine.ActionInstallTtsData` hace que la acción la **datos de voz de Google TTS** actividad que se presentará al usuario para la elección de idiomas para descargar:

![Actividad de datos de voz TTS de Google](speech-images/01-google-tts-voice-data.png)

Por ejemplo, el usuario podría elegir a francés y haga clic en el icono de descarga para descargar los datos de voz francés:

![Ejemplo de descarga de idioma francés](speech-images/02-selecting-french.png)

Instalación de estos datos se realiza automáticamente una vez finalizada la descarga.


### <a name="step-5---the-ioninitlistener"></a>Paso 5: el IOnInitListener

Para una actividad para que pueda convertir texto a voz, el método de interfaz `OnInit` debe estar implementada (este es el segundo parámetro especificado para la creación de instancias de la `TextToSpeech` clase). Esto inicializa el agente de escucha y comprueba el resultado.

Debe probar el agente de escucha para ambos `OperationResult.Success` y `OperationResult.Failure` como mínimo.
En el ejemplo siguiente se muestra exactamente eso:

```csharp
void TextToSpeech.IOnInitListener.OnInit(OperationResult status)
{
    // if we get an error, default to the default language
    if (status == OperationResult.Error)
        textToSpeech.SetLanguage(Java.Util.Locale.Default);
    // if the listener is ok, set the lang
    if (status == OperationResult.Success)
        textToSpeech.SetLanguage(lang);
}
```

## <a name="summary"></a>Resumen

En esta guía hemos examinado los aspectos básicos de la conversión de texto a voz y voz a texto y los métodos posibles incluirlos en sus propias aplicaciones. Mientras no cubren cada caso particular, ahora debería tener un conocimiento básico de cómo se interpreta la voz, cómo instalar nuevos idiomas y cómo aumentar la inclusivity de sus aplicaciones.



## <a name="related-links"></a>Vínculos relacionados

- [Xamarin.Forms DependencyService](https://developer.xamarin.com/samples/UsingDependencyService/)
- [Texto a voz (ejemplo)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/TextToSpeech)
- [Voz a texto (ejemplo)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/SpeechToText)
- [Espacio de nombres de Android.Speech](https://developer.xamarin.com/api/namespace/Android.Speech/)
- [Espacio de nombres Android.Speech.Tts](https://developer.xamarin.com/api/namespace/Android.Speech.Tts/)
