---
title: Las rutas de acceso y el texto de SkiaSharp
description: Este artículo explora la intersección de las rutas de acceso de SkiaSharp y texto y esto muestra con código de ejemplo.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
ms.openlocfilehash: 366a6e9585817c5a47ba5bec14fb2f238ab23a6b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61022063"
---
# <a name="paths-and-text-in-skiasharp"></a>Las rutas de acceso y el texto de SkiaSharp

[![Descargar ejemplo](~/media/shared/download.png) descargar el ejemplo](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Explore la intersección de las rutas de acceso y el texto_

En los sistemas de gráficos modernos, fuentes de texto son colecciones de contornos de carácter, normalmente definidos curvas Bézier cuadráticas. Por lo tanto, muchos sistemas de gráficos modernos incluyen una característica para convertir caracteres de texto en un trazado de gráficos.

Ya hemos visto que puede trazar los contornos de caracteres de texto, así como su relleno. Esto permite mostrar estos contornos de caracteres con un ancho de trazo determinado e incluso un efecto de la ruta de acceso, como se describe en el [ **efectos del trazado** ](effects.md) artículo. Pero también es posible convertir una cadena de caracteres en un `SKPath` objeto. Esto significa que los contornos de texto pueden usarse para recorte con técnicas que se describieron en la [ **recorte con trazados y regiones** ](clipping.md) artículo.

Además de utilizar un efecto de la ruta de acceso para trazar un contorno de carácter, también puede crear efectos de la ruta de acceso que se basan en una ruta de acceso que se deriva de una cadena de caracteres y puede incluso combinar los dos efectos:

![](text-paths-images/pathsandtextsample.png "Efecto de texto de ruta de acceso")

En el artículo anterior sobre [ **los efectos de la ruta de acceso**](effects.md), vimos cómo [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) método de `SKPaint` puede obtener un esquema de un trazado. También puede usar este método con las rutas de acceso que se deriva de contornos de caracteres.

Por último, en este artículo se muestra otro intersección de las rutas de acceso y el texto: El [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) método `SKCanvas` permite mostrar una cadena de texto, por lo que sigue a la línea base del texto de un trazado curvo.

## <a name="text-to-path-conversion"></a>Texto a la conversión de la ruta de acceso

El [ `GetTextPath` ](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single)) método `SKPaint` convierte una cadena de caracteres en un `SKPath` objeto:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

El `x` y `y` argumentos indican el punto inicial de la línea base del lado izquierdo del texto. Desempeñan el mismo rol aquí que en el `DrawText` método `SKCanvas`. Dentro de la ruta de acceso, la línea de base de la parte izquierda del texto tendrán las coordenadas (x, y).

El `GetTextPath` método es una exageración si simplemente desea rellenar o trazar la ruta de acceso resultante. El valor normal `DrawText` método le permite hacerlo. El `GetTextPath` método es más útil para otras tareas relacionadas con las rutas de acceso.

Una de estas tareas se recorte. El **texto de recorte** página crea un trazado de recorte basándose en los contornos de caracteres de la palabra "Código". Esta ruta de acceso se ajusta al tamaño de la página para recortar un mapa de bits que contiene una imagen de la **texto de recorte** código fuente:

[![](text-paths-images/clippingtext-small.png "Captura de pantalla de la página de texto de recorte triple")](text-paths-images/clippingtext-large.png#lightbox "Triple captura de pantalla de la página de texto de recorte")

El [ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) constructor de clase carga el mapa de bits que se almacena como un recurso incrustado en el **Media** carpeta de la solución:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

El `PaintSurface` controlador comienza creando una `SKPaint` adecuado para el texto del objeto. El `Typeface` propiedad está establecida, así como la `TextSize`, aunque para esta aplicación específica del `TextSize` propiedad es meramente arbitraria. Observe también hay ningún `Style` configuración.

El `TextSize` y `Style` valores de las propiedades no son necesarios porque esto `SKPaint` objeto sirve únicamente para el `GetTextPath` llamar mediante la cadena de texto "CODE". El controlador, a continuación, mide el resultante `SKPath` de objetos y se aplica a las tres transformaciones para centrarlo y ajustar la escala al tamaño de la página. A continuación, se puede establecer la ruta de acceso como el trazado de recorte:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Una vez establecido el trazado de recorte, se puede mostrar el mapa de bits y se recortará a los contornos de caracteres. Tenga en cuenta el uso de la [ `AspectFill` ](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) método `SKRect` que calcula un rectángulo para rellenar la página conservando la relación de aspecto.

El **efecto de texto de ruta de acceso** página convierte un carácter de "y" comercial única en una ruta de acceso para crear un efecto de la ruta de acceso de 1 día. Un objeto de pintura con este efecto de la ruta de acceso, a continuación, se usa para trazar el contorno de una versión ampliada de ese mismo carácter:

[![](text-paths-images/textpatheffect-small.png "Captura de pantalla triple de la página de efecto de la ruta de acceso de texto")](text-paths-images/textpatheffect-large.png#lightbox "Triple captura de pantalla de la página de efecto de texto ruta de acceso")

Cantidad de trabajo en el [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) clase se produce en los campos y el constructor. Los dos `SKPaint` objetos definidos como campos sirven para dos propósitos diferentes: La primera (denominado `textPathPaint`) se utiliza para convertir la y comercial con un `TextSize` de 50 a una ruta de acceso para el efecto de la ruta de acceso de 1 día. El segundo (`textPaint`) se usa para mostrar la versión ampliada de la y comercial con el efecto de esa ruta de acceso. Por ese motivo, la `Style` de esta segunda pintura objeto se establece en `Stroke`, pero la `StrokeWidth` no se establece la propiedad porque esa propiedad no es necesaria cuando se usa un efecto de la ruta de acceso 1D:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

El constructor usa primero el `textPathPaint` objeto para medir la y comercial con un `TextSize` de 50. Los valores negativos de las coordenadas del centro del rectángulo, a continuación, se pasan a la `GetTextPath` método para convertir el texto en una ruta de acceso. La ruta de acceso resultante tiene el (0, 0) de punto en el centro del carácter, que es ideal para un efecto de la ruta de acceso de 1 día.

Puede pensar que el `SKPathEffect` creado al final del constructor de objeto se puede establecer en el `PathEffect` propiedad de `textPaint` en lugar de guardarse como un campo. Pero esta no se desactivará out funcionan muy bien porque distorsione los resultados de la `MeasureText` llamar el `PaintSurface` controlador:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Que `MeasureText` llamada se usa para centrar el carácter en la página. Para evitar problemas, el `PathEffect` propiedad está establecida en el objeto de paint después de que se ha medido el texto, pero antes de que se muestre.

## <a name="outlines-of-character-outlines"></a>Contornos de los contornos de caracteres

Normalmente el [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) método `SKPaint` convierte una ruta de acceso a otro mediante la aplicación de las propiedades de paint, sobre todo el ancho y la ruta de acceso efecto trazo. Cuando se usa sin los efectos de la ruta de acceso, `GetFillPath` realmente crea una ruta de acceso que describa otra ruta de acceso. Esto se muestra en el **pulse al contorno de la ruta de acceso** página en el [ **efectos del trazado** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) artículo.

También puede llamar a `GetFillPath` en la ruta de acceso devuelta desde `GetTextPath` pero en primer lugar podría no ser completamente seguro de lo que aspecto que tendrá.

El **contornos de carácter esquema** página demuestra esta técnica. Todo el código pertinente se encuentra en la `PaintSurface` controlador de la [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) clase.

El constructor comienza creando una `SKPaint` objeto denominado `textPaint` con un `TextSize` propiedad en función del tamaño de la página. Esto se convierte en una ruta de acceso mediante la `GetTextPath` método. Los argumentos de coordenadas `GetTextPath` eficazmente del centro de la ruta de acceso en la pantalla:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

El `PaintSurface` controlador, a continuación, crea una nueva ruta denominada `outlinePath`. Esto se convierte en la ruta de acceso de destino en la llamada a `GetFillPath`. El `StrokeWidth` propiedad de 25 causas `outlinePath` para describir el contorno de una ruta de acceso de 25 píxeles de ancho trazado los caracteres de texto. Esta ruta de acceso, a continuación, se muestra en rojo con un ancho de trazo de 5:

[![](text-paths-images/characteroutlineoutlines-small.png "Captura de pantalla de la página de contornos de carácter esquema triple")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Triple captura de pantalla de la página de contornos de contorno de caracteres")

Revise detenidamente y verá que se superpone donde el contorno de la ruta de acceso hace que un vértice agudo. Estos son los artefactos normales de este proceso.

## <a name="text-along-a-path"></a>Texto a lo largo de una ruta de acceso

Normalmente se muestra texto en una línea base horizontal. Se puede girar el texto para ejecutar vertical o diagonalmente, pero la línea de base sigue siendo una línea recta.

Veces, sin embargo, cuando desee que el texto que se ejecute en una curva. Este es el propósito de la [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) método `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

El texto especificado en el primer argumento se realiza para ejecutar a lo largo de la ruta de acceso especificada como segundo argumento. Puede comenzar el texto en un desplazamiento desde el principio de la ruta de acceso con el `hOffset` argumento. Normalmente, la ruta de acceso forma la línea base del texto: Son los ascendentes de texto en un lado de la ruta de acceso y trazos descendentes de texto que se encuentran en el otro. Pero se pueden desplazar la línea base del texto de la ruta de acceso con el `vOffset` argumento.

Este método no tiene ninguna facilidad para proporcionar instrucciones sobre cómo el `TextSize` propiedad de `SKPaint` para poner el texto de tamaño perfectamente para ejecutarse desde el principio de la ruta de acceso al final. A veces puede averiguar ese tamaño de texto por su cuenta. En otras ocasiones, deberá usar funciones de medición de ruta de acceso a se describe en el siguiente artículo en [ **información de ruta de acceso y de enumeración**](information.md).

El **texto Circular** programa ajusta el texto en torno a un círculo. Es fácil determinar la circunferencia de un círculo, por lo que es fácil cambiar el tamaño del texto para ajustarse exactamente a. El `PaintSurface` controlador de la [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) clase calcula un radio de un círculo en función del tamaño de la página. Círculo se convierte en `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

El `TextSize` propiedad de `textPaint` , a continuación, se ajusta para que la longitud de la circunferencia de coincida con el ancho del texto:

[![](text-paths-images/circulartext-small.png "Captura de pantalla de la página de texto Circular triple")](text-paths-images/circulartext-large.png#lightbox "Triple captura de pantalla de la página de texto Circular")

El texto en sí se ha elegido para ser circular algo así: La palabra "círculo" es el sujeto de la oración tanto el objeto de una frase preposicionales.

## <a name="related-links"></a>Vínculos relacionados

- [API de SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
