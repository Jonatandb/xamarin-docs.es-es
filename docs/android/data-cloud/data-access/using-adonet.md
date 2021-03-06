---
title: Uso de ADO.NET con Android
ms.prod: xamarin
ms.assetid: F6ABCEF1-951E-40D8-9EA9-DD79123C2650
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/08/2018
ms.openlocfilehash: 6592bd6d5cf7b78918fa2d020be723d662625e06
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023766"
---
# <a name="using-adonet-with-android"></a>Uso de ADO.NET con Android

Xamarin tiene compatibilidad integrada para la base de datos SQLite que está disponible en Android y se puede exponer con la conocida sintaxis de ADO.NET. El uso de estas API requiere escribir instrucciones SQL procesadas por SQLite, como `CREATE TABLE`, `INSERT` y `SELECT` instrucciones.

## <a name="assembly-references"></a>Referencias de ensamblado

Para usar Access SQLite a través de ADO.NET, debe agregar `System.Data` y `Mono.Data.Sqlite` referencias a su proyecto de Android, como se muestra aquí:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows) 

![Referencias de Android en Visual Studio](using-adonet-images/image7.png "Referencias de Android en Visual Studio") 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio para Mac](#tab/macos) 

![Referencias de Android en Visual Studio para Mac](using-adonet-images/image5.png "Referencias de Android en Visual Studio para Mac") 

-----

Haga clic con el botón secundario en **referencias > Editar referencias...** y, a continuación, haga clic para seleccionar los ensamblados necesarios.

## <a name="about-monodatasqlite"></a>Acerca de mono. Data. SQLite

Usaremos la clase `Mono.Data.Sqlite.SqliteConnection` para crear un archivo de base de datos en blanco y, a continuación, crear una instancia de los objetos `SqliteCommand` que se pueden usar para ejecutar instrucciones SQL en la base de datos.

**Crear una base de datos vacía** &ndash; llamar al método `CreateFile` con una ruta de acceso de archivo válida (es decir, grabable). Debe comprobar si el archivo ya existe antes de llamar a este método; de lo contrario, se creará una nueva base de datos (en blanco) en la parte superior de la antigua y se perderán los datos del archivo anterior.
`Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);` la variable `dbPath` se debe determinar según las reglas descritas anteriormente en este documento.

**Crear una conexión de base** de datos &ndash; una vez creado el archivo de base de datos de SQLite, puede crear un objeto de conexión para obtener acceso a los datos. La conexión se crea con una cadena de conexión que toma la forma de `Data Source=file_path`, como se muestra aquí:

```csharp
var connection = new SqliteConnection ("Data Source=" + dbPath);
connection.Open();
// do stuff
connection.Close();
```

Como se mencionó anteriormente, nunca se debe volver a usar una conexión entre subprocesos diferentes. En caso de duda, cree la conexión según sea necesario y ciérrela cuando haya terminado. pero sea consciente de hacerlo con más frecuencia de lo requerido.

**Crear y ejecutar un comando de base de datos** &ndash; una vez que tenemos una conexión, podemos ejecutar comandos SQL arbitrarios en ella. En el código siguiente se muestra una `CREATE TABLE` instrucción que se está ejecutando.

```csharp
using (var command = connection.CreateCommand ()) {
    command.CommandText = "CREATE TABLE [Items] ([_id] int, [Symbol] ntext, [Name] ntext);";
    var rowcount = command.ExecuteNonQuery ();
}
```

Al ejecutar SQL directamente en la base de datos, debe tomar las precauciones normales para no realizar solicitudes no válidas, como intentar crear una tabla que ya existe. Realice un seguimiento de la estructura de la base de datos para que no se produzca una `SqliteException` como **tabla de errores de SQLite [items] ya existe**.

## <a name="basic-data-access"></a>Acceso a datos básico

El código de ejemplo *DataAccess_Basic* de este documento tiene este aspecto cuando se ejecuta en Android:

![Ejemplo de ADO.NET de Android](using-adonet-images/image8.png "Ejemplo de ADO.NET de Android")

En el código siguiente se muestra cómo realizar operaciones de SQLite simples y se muestra el resultado como texto en la ventana principal de la aplicación.

Deberá incluir estos espacios de nombres:

```csharp
using System;
using System.IO;
using Mono.Data.Sqlite;
```

En el ejemplo de código siguiente se muestra una interacción completa de la base de datos:

1. Crear el archivo de base de datos
2. Insertar algunos datos
3. Consultar los datos

Normalmente, estas operaciones aparecen en varios lugares en todo el código; por ejemplo, puede crear el archivo y las tablas de base de datos cuando la aplicación se inicia por primera vez y realizar lecturas y escrituras de datos en pantallas individuales de la aplicación. En el ejemplo siguiente, se ha agrupado en un único método para este ejemplo:

```csharp
public static SqliteConnection connection;
public static string DoSomeDataAccess ()
{
    // determine the path for the database file
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "adodemo.db3");

    bool exists = File.Exists (dbPath);

    if (!exists) {
        Console.WriteLine("Creating database");
        // Need to create the database before seeding it with some data
        Mono.Data.Sqlite.SqliteConnection.CreateFile (dbPath);
        connection = new SqliteConnection ("Data Source=" + dbPath);

        var commands = new[] {
            "CREATE TABLE [Items] (_id ntext, Symbol ntext);",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'AAPL')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('2', 'GOOG')",
            "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('3', 'MSFT')"
        };
        // Open the database connection and create table with data
        connection.Open ();
        foreach (var command in commands) {
            using (var c = connection.CreateCommand ()) {
                c.CommandText = command;
                var rowcount = c.ExecuteNonQuery ();
                Console.WriteLine("\tExecuted " + command);
            }
        }
    } else {
        Console.WriteLine("Database already exists");
        // Open connection to existing database file
        connection = new SqliteConnection ("Data Source=" + dbPath);
        connection.Open ();
    }

    // query the database to prove data was inserted!
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT [_id], [Symbol] from [Items]";
        var r = contents.ExecuteReader ();
        Console.WriteLine("Reading data");
        while (r.Read ())
            Console.WriteLine("\tKey={0}; Value={1}",
                              r ["_id"].ToString (),
                              r ["Symbol"].ToString ());
    }
    connection.Close ();
}

```

## <a name="more-complex-queries"></a>Consultas más complejas

Dado que SQLite permite ejecutar comandos SQL arbitrarios en los datos, puede realizar cualquier `CREATE`, `INSERT`, `UPDATE`, `DELETE`o `SELECT` instrucciones que desee. Puede leer acerca de los comandos SQL compatibles con SQLite en el sitio web de SQLite. Las instrucciones SQL se ejecutan con uno de los tres métodos de un objeto `SqliteCommand`:

- **ExecuteNonQuery** &ndash; suele usarse para la creación de tablas o la inserción de datos. El valor devuelto para algunas operaciones es el número de filas afectadas; de lo contrario, es-1.

- **ExecuteReader** &ndash; usar cuando se debe devolver una colección de filas como `SqlDataReader`.

- **ExecuteScalar** &ndash; recupera un valor único (por ejemplo, un agregado).

### <a name="executenonquery"></a>EXECUTENONQUERY

las instrucciones `INSERT`, `UPDATE`y `DELETE` devolverán el número de filas afectadas. Todas las demás instrucciones SQL devolverán-1.

```csharp
using (var c = connection.CreateCommand ()) {
    c.CommandText = "INSERT INTO [Items] ([_id], [Symbol]) VALUES ('1', 'APPL')";
    var rowcount = c.ExecuteNonQuery (); // rowcount will be 1
}
```

### <a name="executereader"></a>EXECUTEREADER

El método siguiente muestra una cláusula `WHERE` en la instrucción `SELECT`.
Dado que el código está creando una instrucción SQL completa, debe tener cuidado de escapar caracteres reservados como la comilla (') en torno a las cadenas.

```csharp
public static string MoreComplexQuery ()
{
    var output = "";
    output += "\nComplex query example: ";
    string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal), "ormdemo.db3");

    connection = new SqliteConnection ("Data Source=" + dbPath);
    connection.Open ();
    using (var contents = connection.CreateCommand ()) {
        contents.CommandText = "SELECT * FROM [Items] WHERE Symbol = 'MSFT'";
        var r = contents.ExecuteReader ();
        output += "\nReading data";
        while (r.Read ())
            output += String.Format ("\n\tKey={0}; Value={1}",
                    r ["_id"].ToString (),
                    r ["Symbol"].ToString ());
    }
    connection.Close ();

    return output;
}
```

El método `ExecuteReader` devuelve un objeto `SqliteDataReader`. Además del método `Read` que se muestra en el ejemplo, otras propiedades útiles incluyen:

- **RowsAffected** &ndash; recuento de las filas afectadas por la consulta.

- **HasRows** &ndash; si se devolvieron filas.

### <a name="executescalar"></a>EXECUTESCALAR

Utilice esto para `SELECT` instrucciones que devuelven un valor único (como un agregado).

```csharp
using (var contents = connection.CreateCommand ()) {
    contents.CommandText = "SELECT COUNT(*) FROM [Items] WHERE Symbol <> 'MSFT'";
    var i = contents.ExecuteScalar ();
}
```

El tipo de valor devuelto del método `ExecuteScalar` es `object` &ndash; debe convertir el resultado en función de la consulta de la base de datos. El resultado puede ser un entero de una consulta de `COUNT` o una cadena de una sola columna `SELECT` consulta. Tenga en cuenta que esto es diferente a otros métodos `Execute` que devuelven un objeto lector o un recuento del número de filas afectadas.

## <a name="related-links"></a>Vínculos relacionados

- [Acceso a datos básico (ejemplo)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [Acceso a la configuración avanzada (ejemplo)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Recetas de datos de Android](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Acceso a datos de Xamarin. Forms](~/xamarin-forms/data-cloud/data/databases.md)
