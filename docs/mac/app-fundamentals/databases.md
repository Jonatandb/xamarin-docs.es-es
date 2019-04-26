---
title: Bases de datos de Xamarin.Mac
description: En este artículo se describe el uso de codificación y observación para permitir el enlace de datos entre bases de datos de SQLite y elementos de interfaz de usuario en Interface Builder de Xcode de pares clave-valor de pares clave-valor. También se describe mediante el SQLite.NET ORM para proporcionar acceso a los datos de SQLite.
ms.prod: xamarin
ms.assetid: 44FAFDA8-612A-4E0F-8BB4-5C92A3F4D552
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 465b4a34d54dbee92461669b16c3b8a13188bbde
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61378372"
---
# <a name="databases-in-xamarinmac"></a>Bases de datos de Xamarin.Mac

_En este artículo se describe el uso de codificación y observación para permitir el enlace de datos entre bases de datos de SQLite y elementos de interfaz de usuario en Interface Builder de Xcode de pares clave-valor de pares clave-valor. También se describe mediante el SQLite.NET ORM para proporcionar acceso a los datos de SQLite._

## <a name="overview"></a>Información general

Cuando se trabaja con C# y .NET en una aplicación de Xamarin.Mac, tener acceso a las mismas bases de datos SQLite que puede tener acceso una aplicación Xamarin.iOS o Xamarin.Android.

En este artículo se cubren dos maneras de obtener acceso a datos de SQLite lo siguiente:

1. **Acceso directo** : accediendo directamente a una base de datos de SQLite, podemos usar datos de la base de datos para la codificación de clave-valor y enlace de datos con los elementos de interfaz de usuario creado en Interface Builder de Xcode. Mediante el uso de pares clave-valor de codificación y las técnicas de enlace en la aplicación de Xamarin.Mac de datos, puede reducir significativamente la cantidad de código que debe escribir y mantener para rellenar y trabajar con elementos de interfaz de usuario. También tiene la ventaja de separar aún más los datos de respaldo (_modelo de datos_) de la parte delantera terminar la interfaz de usuario (_Model-View-Controller_), provocando fáciles de mantener, aplicaciones más flexibles diseño.
2. **SQLite.NET ORM** : mediante el uso de código abierto [SQLite.NET](http://www.sqlite.org) Administrador de relaciones de objetos (ORM) podemos reducir en gran medida la cantidad de código necesario para leer y escribir datos desde una base de datos de SQLite. Estos datos, a continuación, se pueden utilizar para rellenar un elemento de interfaz de usuario como una vista de tabla.

[![Un ejemplo de la aplicación en ejecución](databases-images/intro01.png "un ejemplo de la aplicación en ejecución")](databases-images/intro01-large.png#lightbox)

En este artículo, trataremos los aspectos básicos de trabajar con pares clave-valor de codificación y el enlace de datos con bases de datos de SQLite en una aplicación de Xamarin.Mac. Se recomienda que trabaje en el [Hello, Mac](~/mac/get-started/hello-mac.md) artículo en primer lugar, específicamente el [Introducción a Xcode e Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) y [salidas y acciones](~/mac/get-started/hello-mac.md#outlets-and-actions) secciones, tal y como se tratan los conceptos clave y las técnicas que vamos a usar en este artículo.

Puesto que vamos a usar la codificación de clave-valor y enlace de datos, póngase en contacto a través de la [enlace de datos y la codificación de pares clave-valor](~/mac/app-fundamentals/databinding.md) en primer lugar, como los principales técnicas y conceptos que se tratarán que se usará en su ejemplo y esta documentación aplicación.

Es posible que desee echar un vistazo a la [clases de C# exponer / métodos a Objective-C](~/mac/internals/how-it-works.md) sección de la [funcionamiento interno de Xamarin.Mac](~/mac/internals/how-it-works.md) documentar, también explica la `Register` y `Export` atributos se utiliza para conectar las clases de C# para objetos de Objective-C y la interfaz de usuario de elementos.

## <a name="direct-sqlite-access"></a>Acceso directo de SQLite

Para los datos de SQLite que se van a enlazarse a los elementos de interfaz de usuario de Interface Builder de Xcode, se alta sugiere que tener acceso a la base de datos de SQLite directamente (en lugar de mediante una técnica como un ORM), puesto que tiene control total sobre la manera se escriben y se leen los datos  desde la base de datos.

Como hemos visto en el [enlace de datos y la codificación de pares clave-valor](~/mac/app-fundamentals/databinding.md) documentación, mediante el uso de clave y valor de codificación y en la aplicación de Xamarin.Mac, técnicas de enlace de datos puede reducir significativamente la cantidad de código que tiene que escribir y mantener para rellenar y trabajar con elementos de interfaz de usuario. Cuando se combina con acceso directo a una base de datos de SQLite, puede reducir enormemente la cantidad de código necesario para leer y escribir datos en esa base de datos.

En este artículo, modificaremos la aplicación de ejemplo en el enlace de datos y el documento de codificación de pares clave-valor para utilizar una base de datos de SQLite como el origen de respaldo para el enlace.

### <a name="including-sqlite-database-support"></a>Incluida la compatibilidad de base de datos de SQLite

Antes de que podamos seguir, necesitamos agregar compatibilidad de base de datos de SQLite a nuestra aplicación mediante la inclusión de referencias a un par de. Archivos DLL.

Haga lo siguiente:

1. En el **panel de solución**, haga doble clic en el **referencias** carpeta y seleccione **editar referencias**.
2. Seleccione el **Mono.Data.Sqlite** y **System.Data** ensamblados: 

    [![Agregar las referencias necesarias](databases-images/reference01.png "agregar las referencias necesarias")](databases-images/reference01-large.png#lightbox)
3. Haga clic en el **Aceptar** botón para guardar los cambios y agregue las referencias.

### <a name="modifying-the-data-model"></a>Modificar el modelo de datos

Ahora que hemos agregado compatibilidad para tener acceso directamente a una base de datos de SQLite a nuestra aplicación, es necesario modificar el objeto del modelo de datos para leer y escribir datos desde la base de datos (así como proporcionar codificación de clave-valor y enlace de datos). En el caso de nuestra aplicación de ejemplo, se modificará la **PersonModel.cs** clase y dele un aspecto similar al siguiente:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    [Register("PersonModel")]
    public class PersonModel : NSObject
    {
        #region Private Variables
        private string _ID = "";
        private string _managerID = "";
        private string _name = "";
        private string _occupation = "";
        private bool _isManager = false;
        private NSMutableArray _people = new NSMutableArray();
        private SqliteConnection _conn = null;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        [Export("ID")]
        public string ID {
            get { return _ID; }
            set {
                WillChangeValue ("ID");
                _ID = value;
                DidChangeValue ("ID");
            }
        }

        [Export("ManagerID")]
        public string ManagerID {
            get { return _managerID; }
            set {
                WillChangeValue ("ManagerID");
                _managerID = value;
                DidChangeValue ("ManagerID");
            }
        }

        [Export("Name")]
        public string Name {
            get { return _name; }
            set {
                WillChangeValue ("Name");
                _name = value;
                DidChangeValue ("Name");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("Occupation")]
        public string Occupation {
            get { return _occupation; }
            set {
                WillChangeValue ("Occupation");
                _occupation = value;
                DidChangeValue ("Occupation");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isManager")]
        public bool isManager {
            get { return _isManager; }
            set {
                WillChangeValue ("isManager");
                WillChangeValue ("Icon");
                _isManager = value;
                DidChangeValue ("isManager");
                DidChangeValue ("Icon");

                // Save changes to database?
                if (_conn != null) Update (_conn);
            }
        }

        [Export("isEmployee")]
        public bool isEmployee {
            get { return (NumberOfEmployees == 0); }
        }

        [Export("Icon")]
        public NSImage Icon {
            get {
                if (isManager) {
                    return NSImage.ImageNamed ("group.png");
                } else {
                    return NSImage.ImageNamed ("user.png");
                }
            }
        }

        [Export("personModelArray")]
        public NSArray People {
            get { return _people; }
        }

        [Export("NumberOfEmployees")]
        public nint NumberOfEmployees {
            get { return (nint)_people.Count; }
        }
        #endregion

        #region Constructors
        public PersonModel ()
        {
        }

        public PersonModel (string name, string occupation)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (string name, string occupation, bool manager)
        {
            // Initialize
            this.Name = name;
            this.Occupation = occupation;
            this.isManager = manager;
        }

        public PersonModel (string id, string name, string occupation)
        {
            // Initialize
            this.ID = id;
            this.Name = name;
            this.Occupation = occupation;
        }

        public PersonModel (SqliteConnection conn, string id)
        {
            // Load from database
            Load (conn, id);
        }
        #endregion

        #region Array Controller Methods
        [Export("addObject:")]
        public void AddPerson(PersonModel person) {
            WillChangeValue ("personModelArray");
            isManager = true;
            _people.Add (person);
            DidChangeValue ("personModelArray");
        }

        [Export("insertObject:inPersonModelArrayAtIndex:")]
        public void InsertPerson(PersonModel person, nint index) {
            WillChangeValue ("personModelArray");
            _people.Insert (person, index);
            DidChangeValue ("personModelArray");
        }

        [Export("removeObjectFromPersonModelArrayAtIndex:")]
        public void RemovePerson(nint index) {
            WillChangeValue ("personModelArray");
            _people.RemoveObject (index);
            DidChangeValue ("personModelArray");
        }

        [Export("setPersonModelArray:")]
        public void SetPeople(NSMutableArray array) {
            WillChangeValue ("personModelArray");
            _people = array;
            DidChangeValue ("personModelArray");
        }
        #endregion

        #region SQLite Routines
        public void Create(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Create new record ID?
            if (ID == "") {
                ID = Guid.NewGuid ().ToString();
            }

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Save manager ID and create the sub record
                Person.ManagerID = ID;
                Person.Create (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Update(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);
                command.Parameters.AddWithValue ("@COL2", Name);
                command.Parameters.AddWithValue ("@COL3", Occupation);
                command.Parameters.AddWithValue ("@COL4", isManager);
                command.Parameters.AddWithValue ("@COL5", ManagerID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Save children to database as well
            for (nuint n = 0; n < People.Count; ++n) {
                // Grab person
                var Person = People.GetItem<PersonModel>(n);

                // Update sub record
                Person.Update (conn);
            }

            // Save last connection
            _conn = conn;
        }

        public void Load(SqliteConnection conn, string id) {
            bool shouldClose = false;

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Is the database already open?
            if (conn.State != ConnectionState.Open) {
                shouldClose = true;
                conn.Open ();
            }

            // Execute query
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", id);

                using (var reader = command.ExecuteReader ()) {
                    while (reader.Read ()) {
                        // Pull values back into class
                        ID = (string)reader [0];
                        Name = (string)reader [1];
                        Occupation = (string)reader [2];
                        isManager = (bool)reader [3];
                        ManagerID = (string)reader [4];
                    }
                }
            }

            // Is this a manager?
            if (isManager) {
                // Yes, load children
                using (var command = conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

                    // Populate with data from the record
                    command.Parameters.AddWithValue ("@COL1", id);

                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Load child and add to collection
                            var childID = (string)reader [0];
                            var person = new PersonModel (conn, childID);
                            _people.Add (person);
                        }
                    }
                }
            }

            // Should we close the connection to the database
            if (shouldClose) {
                conn.Close ();
            }

            // Save last connection
            _conn = conn;
        }

        public void Delete(SqliteConnection conn) {

            // Clear last connection to prevent circular call to update
            _conn = null;

            // Execute query
            conn.Open ();
            using (var command = conn.CreateCommand ()) {
                // Create new command
                command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

                // Populate with data from the record
                command.Parameters.AddWithValue ("@COL1", ID);

                // Write to database
                command.ExecuteNonQuery ();
            }
            conn.Close ();

            // Empty class
            ID = "";
            ManagerID = "";
            Name = "";
            Occupation = "";
            isManager = false;
            _people = new NSMutableArray();

            // Save last connection
            _conn = conn;
        }
        #endregion
    }
}
```

Echemos un vistazo a las modificaciones en detalle a continuación.

En primer lugar, hemos agregado varias instrucciones using que son necesarias para usar SQLite y hemos agregado una variable para guardar la última conexión a la base de datos de SQLite:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection _conn = null;
```

Vamos a usar esta conexión guardada para guardar automáticamente los cambios en el registro de la base de datos cuando el usuario modifica el contenido en la interfaz de usuario mediante enlace de datos:

```csharp
[Export("Name")]
public string Name {
    get { return _name; }
    set {
        WillChangeValue ("Name");
        _name = value;
        DidChangeValue ("Name");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("Occupation")]
public string Occupation {
    get { return _occupation; }
    set {
        WillChangeValue ("Occupation");
        _occupation = value;
        DidChangeValue ("Occupation");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}

[Export("isManager")]
public bool isManager {
    get { return _isManager; }
    set {
        WillChangeValue ("isManager");
        WillChangeValue ("Icon");
        _isManager = value;
        DidChangeValue ("isManager");
        DidChangeValue ("Icon");

        // Save changes to database?
        if (_conn != null) Update (_conn);
    }
}
```

Los cambios realizados en el **nombre**, **ocupación** o **isManager** propiedades se enviarán a la base de datos si los datos se ha guardado allí antes (por ejemplo, si el `_conn` la variable no es `null`). A continuación, echemos un vistazo a los métodos que hemos agregado a **crear**, **actualización**, **carga** y **eliminar** con las personas de la base de datos.

#### <a name="create-a-new-record"></a>Cree un nuevo registro

Se ha agregado el código siguiente para crear un nuevo registro en la base de datos de SQLite:

```csharp
public void Create(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Create new record ID?
    if (ID == "") {
        ID = Guid.NewGuid ().ToString();
    }

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Save manager ID and create the sub record
        Person.ManagerID = ID;
        Person.Create (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Estamos usando un `SQLiteCommand` para crear el nuevo registro en la base de datos. Obtenemos un nuevo comando de la `SQLiteConnection` (conn) que se pasa al método mediante una llamada a `CreateCommand`. A continuación, establecemos la instrucción SQL para escribir realmente el nuevo registro, proporcionando los parámetros para los valores reales:

```csharp
command.CommandText = "INSERT INTO [People] (ID, Name, Occupation, isManager, ManagerID) VALUES (@COL1, @COL2, @COL3, @COL4, @COL5)";
```

Más adelante se establezca los valores para los parámetros mediante la `Parameters.AddWithValue` método en el `SQLiteCommand`. Mediante el uso de parámetros, nos aseguramos de que los valores (por ejemplo, una comilla sencilla) se codifican correctamente antes de enviarse a SQLite. Ejemplo:

```csharp
command.Parameters.AddWithValue ("@COL1", ID);
```

Por último, dado que una persona puede ser un administrador y tiene una colección de los empleados en ellos, estamos recursivamente una llamada a la `Create` método en esas personas para guardarlos en la base de datos:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Save manager ID and create the sub record
    Person.ManagerID = ID;
    Person.Create (conn);
}
```

#### <a name="updating-a-record"></a>Actualizar un registro

Se ha agregado el código siguiente para actualizar un registro existente en la base de datos de SQLite:

```csharp
public void Update(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);
        command.Parameters.AddWithValue ("@COL2", Name);
        command.Parameters.AddWithValue ("@COL3", Occupation);
        command.Parameters.AddWithValue ("@COL4", isManager);
        command.Parameters.AddWithValue ("@COL5", ManagerID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Save children to database as well
    for (nuint n = 0; n < People.Count; ++n) {
        // Grab person
        var Person = People.GetItem<PersonModel>(n);

        // Update sub record
        Person.Update (conn);
    }

    // Save last connection
    _conn = conn;
}
```

Al igual que **crear** anterior, obtenemos un `SQLiteCommand` desde pasado en `SQLiteConnection`y establezca nuestro SQL para actualizar nuestro registro (proporcionando parámetros):

```csharp
command.CommandText = "UPDATE [People] SET Name = @COL2, Occupation = @COL3, isManager = @COL4, ManagerID = @COL5 WHERE ID = @COL1";
```

Se rellene los valores de parámetro (ejemplo: `command.Parameters.AddWithValue ("@COL1", ID);`) y de nuevo, recursivamente llama a update en todos los registros:

```csharp
// Save children to database as well
for (nuint n = 0; n < People.Count; ++n) {
    // Grab person
    var Person = People.GetItem<PersonModel>(n);

    // Update sub record
    Person.Update (conn);
}
```
#### <a name="loading-a-record"></a>Cargar un registro

Se ha agregado el código siguiente para cargar un registro existente de la base de datos de SQLite:

```csharp
public void Load(SqliteConnection conn, string id) {
    bool shouldClose = false;

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Is the database already open?
    if (conn.State != ConnectionState.Open) {
        shouldClose = true;
        conn.Open ();
    }

    // Execute query
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT * FROM [People] WHERE ID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Pull values back into class
                ID = (string)reader [0];
                Name = (string)reader [1];
                Occupation = (string)reader [2];
                isManager = (bool)reader [3];
                ManagerID = (string)reader [4];
            }
        }
    }

    // Is this a manager?
    if (isManager) {
        // Yes, load children
        using (var command = conn.CreateCommand ()) {
            // Create new command
            command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

            // Populate with data from the record
            command.Parameters.AddWithValue ("@COL1", id);

            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Load child and add to collection
                    var childID = (string)reader [0];
                    var person = new PersonModel (conn, childID);
                    _people.Add (person);
                }
            }
        }
    }

    // Should we close the connection to the database
    if (shouldClose) {
        conn.Close ();
    }

    // Save last connection
    _conn = conn;
}
```

Dado que la rutina se puede llamar de forma recursiva desde un objeto primario (por ejemplo, un objeto de administrador al cargar su objeto de los empleados), se ha agregado código especial para controlar la apertura y cierre la conexión a la base de datos:

```csharp
bool shouldClose = false;
...

// Is the database already open?
if (conn.State != ConnectionState.Open) {
    shouldClose = true;
    conn.Open ();
}
...

// Should we close the connection to the database
if (shouldClose) {
    conn.Close ();
}

```

Como siempre, establecemos nuestra SQL para recuperar el registro y usar parámetros:

```csharp
// Create new command
command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", id);
```

Por último, utilizamos un lector de datos para ejecutar la consulta y devolver los campos de registro (que se copie en la instancia de la `PersonModel` clase):

```csharp
using (var reader = command.ExecuteReader ()) {
    while (reader.Read ()) {
        // Pull values back into class
        ID = (string)reader [0];
        Name = (string)reader [1];
        Occupation = (string)reader [2];
        isManager = (bool)reader [3];
        ManagerID = (string)reader [4];
    }
}
```

Si esta persona es un administrador, es necesario cargar también todos sus empleados (de nuevo, mediante una llamada de forma recursiva sus `Load` método):

```csharp
// Is this a manager?
if (isManager) {
    // Yes, load children
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "SELECT ID FROM [People] WHERE ManagerID = @COL1";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", id);

        using (var reader = command.ExecuteReader ()) {
            while (reader.Read ()) {
                // Load child and add to collection
                var childID = (string)reader [0];
                var person = new PersonModel (conn, childID);
                _people.Add (person);
            }
        }
    }
}
```

#### <a name="deleting-a-record"></a>Eliminar un registro

Se ha agregado el código siguiente para eliminar un registro existente de la base de datos de SQLite:

```csharp
public void Delete(SqliteConnection conn) {

    // Clear last connection to prevent circular call to update
    _conn = null;

    // Execute query
    conn.Open ();
    using (var command = conn.CreateCommand ()) {
        // Create new command
        command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

        // Populate with data from the record
        command.Parameters.AddWithValue ("@COL1", ID);

        // Write to database
        command.ExecuteNonQuery ();
    }
    conn.Close ();

    // Empty class
    ID = "";
    ManagerID = "";
    Name = "";
    Occupation = "";
    isManager = false;
    _people = new NSMutableArray();

    // Save last connection
    _conn = conn;
}
```

Aquí proporcionamos el SQL para eliminar el registro de los administradores y los registros de los empleados en ese administrador (mediante parámetros):

```csharp
// Create new command
command.CommandText = "DELETE FROM [People] WHERE (ID = @COL1 OR ManagerID = @COL1)";

// Populate with data from the record
command.Parameters.AddWithValue ("@COL1", ID);
```

Una vez eliminado el registro, desactive la instancia actual de la `PersonModel` clase:

```csharp
// Empty class
ID = "";
ManagerID = "";
Name = "";
Occupation = "";
isManager = false;
_people = new NSMutableArray();
```

### <a name="initializing-the-database"></a>Inicializar la base de datos

Con los cambios en nuestro modelo de datos en su lugar para admitir la lectura y escritura en la base de datos, es necesario abrir una conexión a la base de datos e inicialícelo en la primera ejecución. Vamos a agregar el código siguiente para nuestro **MainWindow.cs** archivo:

```csharp
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
...

private SqliteConnection DatabaseConnection = null;
...

private SqliteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "People.db3");

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);
    if (!exists)
        SqliteConnection.CreateFile (db);

    // Create connection to the database
    var conn = new SqliteConnection("Data Source=" + db);

    // Set the structure of the database
    if (!exists) {
        var commands = new[] {
            "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
        };
        conn.Open ();
        foreach (var cmd in commands) {
            using (var c = conn.CreateCommand()) {
                c.CommandText = cmd;
                c.CommandType = CommandType.Text;
                c.ExecuteNonQuery ();
            }
        }
        conn.Close ();

        // Build list of employees
        var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
        Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
        Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
        Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
        Craig.Create (conn);

        var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
        Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
        Larry.Create (conn);
    }

    // Return new connection
    return conn;
}
```

Echemos un vistazo en el código anterior. En primer lugar, elegimos una ubicación para la nueva base de datos (en este ejemplo, el escritorio del usuario), buscar para ver si existe la base de datos y, si no es así, créela:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "People.db3");

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
if (!exists)
    SqliteConnection.CreateFile (db);
```

A continuación, establecemos el conectarse a la base de datos mediante la ruta de acceso que hemos creado anteriormente:

```csharp
var conn = new SqliteConnection("Data Source=" + db);
```

A continuación, creamos todas las tablas SQL en la base de datos que se requiere:

```csharp
var commands = new[] {
    "CREATE TABLE People (ID TEXT, Name TEXT, Occupation TEXT, isManager BOOLEAN, ManagerID TEXT)"
};
conn.Open ();
foreach (var cmd in commands) {
    using (var c = conn.CreateCommand()) {
        c.CommandText = cmd;
        c.CommandType = CommandType.Text;
        c.ExecuteNonQuery ();
    }
}
conn.Close ();
```

Por último, utilizamos nuestro modelo de datos (`PersonModel`) para crear un conjunto de registros predeterminado para el tiempo de la base de datos la primera se ejecuta la aplicación o si falta la base de datos:

```csharp
// Build list of employees
var Craig = new PersonModel ("0","Craig Dunn", "Documentation Manager");
Craig.AddPerson (new PersonModel ("Amy Burns", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Joel Martinez", "Web & Infrastructure"));
Craig.AddPerson (new PersonModel ("Kevin Mullins", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Mark McLemore", "Technical Writer"));
Craig.AddPerson (new PersonModel ("Tom Opgenorth", "Technical Writer"));
Craig.Create (conn);

var Larry = new PersonModel ("1","Larry O'Brien", "API Documentation Manager");
Larry.AddPerson (new PersonModel ("Mike Norman", "API Documentor"));
Larry.Create (conn);
``` 

Cuando la aplicación se inicia y abre la ventana principal, realizamos una conexión a la base de datos con el código que agregamos anteriormente:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get access to database
    DatabaseConnection = GetDatabaseConnection ();
}
```

### <a name="loading-bound-data"></a>Carga los datos enlazados

Con todos los componentes de acceso directo a datos enlazados de una base de datos de SQLite en su lugar, se puede cargar directamente los datos en las distintas vistas que proporciona la aplicación y automáticamente se mostrará en la interfaz de usuario.

#### <a name="loading-a-single-record"></a>Cargar un único registro

Para cargar un único registro donde el identificador es saber, podemos usar el código siguiente:

```csharp
Person = new PersonModel (Conn, "0");
```

#### <a name="loading-all-records"></a>Cargar todos los registros

Para cargar todos los usuarios, independientemente de si son o no un administrador, use el código siguiente:

```csharp
// Load all employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People]";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();

```

En este caso, estamos usando una sobrecarga del constructor para la `PersonModel` clase a la persona que se cargan en memoria:

```csharp
var person = new PersonModel (_conn, childID);
```

También estamos llamando a la clase enlazado a datos para agregar la persona a la colección de personas `AddPerson (person)`, esto garantiza que la interfaz de usuario reconoce el cambio y lo muestra:

```csharp
[Export("addObject:")]
public void AddPerson(PersonModel person) {
    WillChangeValue ("personModelArray");
    isManager = true;
    _people.Add (person);
    DidChangeValue ("personModelArray");
}
```

#### <a name="loading-top-level-records-only"></a>Cargar solo los registros de nivel superior

Para cargar solo los administradores (por ejemplo, para mostrar datos en una vista de esquema), usamos el código siguiente:

```csharp
// Load only managers employees
_conn.Open ();
using (var command = _conn.CreateCommand ()) {
    // Create new command
    command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1";

    using (var reader = command.ExecuteReader ()) {
        while (reader.Read ()) {
            // Load child and add to collection
            var childID = (string)reader [0];
            var person = new PersonModel (_conn, childID);
            AddPerson (person);
        }
    }
}
_conn.Close ();
```

La única diferencia real en la instrucción SQL en (que carga solo los administradores `command.CommandText = "SELECT ID FROM [People] WHERE isManager = 1"`), pero funciona igual que en la sección anterior en caso contrario.

<a name="Databases-and-ComboBoxes" />

### <a name="databases-and-comboboxes"></a>Las bases de datos y cuadros combinados

Se puede establecer los controles de menú disponibles para macOS (por ejemplo, el cuadro combinado) para rellenar la lista desplegable desde una lista interna (que se puede definir previamente en el generador de interfaz o rellena por medio de código), o proporcionando su propio origen de datos personalizados y externos. Consulte [que proporciona datos de Control de menú](~/mac/user-interface/standard-controls.md#Providing-Menu-Control-Data) para obtener más detalles.

Por ejemplo, modificar el ejemplo de enlace Simple anterior en el generador de interfaz, agregue un cuadro combinado y exponerlo mediante una salida denominada `EmployeeSelector`:

[![Exponer una toma de cuadro combinado](databases-images/combo01.png "exponer una toma de cuadro combinado")](databases-images/combo01-large.png#lightbox)

En el **Inspector de atributos**, compruebe la **completa automáticamente** y **utiliza el origen de datos** propiedades:

![Configuración de los atributos de cuadro combinado](databases-images/combo02.png "configurar los atributos de cuadro combinado")

Guarde los cambios y vuelva a Visual Studio para Mac sincronizar.

#### <a name="providing-combobox-data"></a>Proporciona datos de cuadro combinado

A continuación, agregue una nueva clase al proyecto denominado `ComboBoxDataSource` y darle un aspecto similar al siguiente:

```csharp
using System;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public class ComboBoxDataSource : NSComboBoxDataSource
    {
        #region Private Variables
        private SqliteConnection _conn = null;
        private string _tableName = "";
        private string _IDField = "ID";
        private string _displayField = "";
        private nint _recordCount = 0;
        #endregion

        #region Computed Properties
        public SqliteConnection Conn {
            get { return _conn; }
            set { _conn = value; }
        }

        public string TableName {
            get { return _tableName; }
            set { 
                _tableName = value;
                _recordCount = GetRecordCount ();
            }
        }

        public string IDField {
            get { return _IDField; }
            set {
                _IDField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public string DisplayField {
            get { return _displayField; }
            set { 
                _displayField = value; 
                _recordCount = GetRecordCount ();
            }
        }

        public nint RecordCount {
            get { return _recordCount; }
        }
        #endregion

        #region Constructors
        public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.DisplayField = displayField;
        }

        public ComboBoxDataSource (SqliteConnection conn, string tableName, string idField, string displayField)
        {
            // Initialize
            this.Conn = conn;
            this.TableName = tableName;
            this.IDField = idField;
            this.DisplayField = displayField;
        }
        #endregion

        #region Private Methods
        private nint GetRecordCount ()
        {
            bool shouldClose = false;
            nint count = 0;

            // Has a Table, ID and display field been specified?
            if (TableName !="" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read count from query
                            var result = (long)reader [0];
                            count = (nint)result;
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return the number of records
            return count;
        }
        #endregion

        #region Public Methods
        public string IDForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string ValueForIndex (nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public string IDForValue (string value)
        {
            NSString result = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", value);

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            result = new NSString ((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return result;
        }
        #endregion 

        #region Override Methods
        public override nint ItemCount (NSComboBox comboBox)
        {
            return RecordCount;
        }

        public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
        {
            NSString value = new NSString ("");
            bool shouldClose = false;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            value = new NSString((string)reader [0]);
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return value;
        }

        public override nint IndexOfItem (NSComboBox comboBox, string value)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";
            nint index = NSRange.NotFound;

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read () && !found) {
                            // Read the display field from the query
                            field = (string)reader [0];
                            ++index;

                            // Is this the value we are searching for?
                            if (value == field) {
                                // Yes, exit loop
                                found = true;
                            }
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return index;
        }

        public override string CompletedString (NSComboBox comboBox, string uncompletedString)
        {
            bool shouldClose = false;
            bool found = false;
            string field = "";

            // Has a Table, ID and display field been specified?
            if (TableName != "" && IDField != "" && DisplayField != "") {
                // Is the database already open?
                if (Conn.State != ConnectionState.Open) {
                    shouldClose = true;
                    Conn.Open ();
                }

                // Escape search string
                uncompletedString = uncompletedString.Replace ("'", "");

                // Execute query
                using (var command = Conn.CreateCommand ()) {
                    // Create new command
                    command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

                    // Populate parameters
                    command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

                    // Get the results from the database
                    using (var reader = command.ExecuteReader ()) {
                        while (reader.Read ()) {
                            // Read the display field from the query
                            field = (string)reader [0];
                        }
                    }
                }

                // Should we close the connection to the database
                if (shouldClose) {
                    Conn.Close ();
                }
            }

            // Return results
            return field;
        }
        #endregion
    }
}
```

En este ejemplo, vamos a crear un nuevo `NSComboBoxDataSource` que puede presentar los elementos del cuadro combinado de cualquier origen de datos de SQLite. En primer lugar, definimos las siguientes propiedades:

- `Conn` : Obtiene o establece una conexión a la base de datos de SQLite.
- `TableName` : Obtiene o establece el nombre de tabla.
- `IDField` : Obtiene o establece el campo que proporciona el identificador único para la tabla dada. El valor predeterminado es `ID`.
- `DisplayField` : Obtiene o establece el campo que se muestra en la lista desplegable.
- `RecordCount` -Obtiene el número de registros en la tabla dada.

Cuando se crea una nueva instancia del objeto, se pasa en la conexión, nombre de la tabla, opcionalmente, el campo Id. y el campo de visualización:

```csharp
public ComboBoxDataSource (SqliteConnection conn, string tableName, string displayField)
{
    // Initialize
    this.Conn = conn;
    this.TableName = tableName;
    this.DisplayField = displayField;
}
```

El `GetRecordCount` método devuelve el número de registros en la tabla dada:

```csharp
private nint GetRecordCount ()
{
    bool shouldClose = false;
    nint count = 0;

    // Has a Table, ID and display field been specified?
    if (TableName !="" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT count({IDField}) FROM [{TableName}]";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read count from query
                    var result = (long)reader [0];
                    count = (nint)result;
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return the number of records
    return count;
}
```

Se llama siempre que el `TableName`, `IDField` o `DisplayField` se cambia el valor de propiedades.

El `IDForIndex` método devuelve el identificador único (`IDField`) para el registro en el índice del elemento de lista desplegable determinado: 

```csharp
public string IDForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

El `ValueForIndex` método devuelve el valor (`DisplayField`) para el elemento en el índice de la lista desplegable determinado:

```csharp
public string ValueForIndex (nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

El `IDForValue` método devuelve el identificador único (`IDField`) para el valor especificado (`DisplayField`):

```csharp
public string IDForValue (string value)
{
    NSString result = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {IDField} FROM [{TableName}] WHERE {DisplayField} = @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", value);

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    result = new NSString ((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return result;
}
```

El `ItemCount` devuelve el número de elementos precalculado en la lista, como cuándo se calculan los `TableName`, `IDField` o `DisplayField` se cambian las propiedades:

```csharp
public override nint ItemCount (NSComboBox comboBox)
{
    return RecordCount;
}
```

El `ObjectValueForItem` método proporciona el valor (`DisplayField`) para el índice del elemento de lista desplegable determinado:

```csharp
public override NSObject ObjectValueForItem (NSComboBox comboBox, nint index)
{
    NSString value = new NSString ("");
    bool shouldClose = false;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC LIMIT 1 OFFSET {index}";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    value = new NSString((string)reader [0]);
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return value;
}
```

Tenga en cuenta que estamos usando la `LIMIT` y `OFFSET` instrucciones en nuestro comando SQLite para limitar a un registro la que estamos necesitábamos.

El `IndexOfItem` método devuelve el índice del elemento de lista desplegable del valor (`DisplayField`) determinado:

```csharp
public override nint IndexOfItem (NSComboBox comboBox, string value)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";
    nint index = NSRange.NotFound;

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] ORDER BY {DisplayField} ASC";

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read () && !found) {
                    // Read the display field from the query
                    field = (string)reader [0];
                    ++index;

                    // Is this the value we are searching for?
                    if (value == field) {
                        // Yes, exit loop
                        found = true;
                    }
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return index;
}
```

Si no se encuentra el valor, el `NSRange.NotFound` valor es el valor devuelto y se anula la selección de todos los elementos en la lista desplegable.

El `CompletedString` método devuelve el primer valor coincidente (`DisplayField`) para una entrada escrita parcialmente:

```csharp
public override string CompletedString (NSComboBox comboBox, string uncompletedString)
{
    bool shouldClose = false;
    bool found = false;
    string field = "";

    // Has a Table, ID and display field been specified?
    if (TableName != "" && IDField != "" && DisplayField != "") {
        // Is the database already open?
        if (Conn.State != ConnectionState.Open) {
            shouldClose = true;
            Conn.Open ();
        }

        // Escape search string
        uncompletedString = uncompletedString.Replace ("'", "");

        // Execute query
        using (var command = Conn.CreateCommand ()) {
            // Create new command
            command.CommandText = $"SELECT {DisplayField} FROM [{TableName}] WHERE {DisplayField} LIKE @VAL";

            // Populate parameters
            command.Parameters.AddWithValue ("@VAL", uncompletedString + "%");

            // Get the results from the database
            using (var reader = command.ExecuteReader ()) {
                while (reader.Read ()) {
                    // Read the display field from the query
                    field = (string)reader [0];
                }
            }
        }

        // Should we close the connection to the database
        if (shouldClose) {
            Conn.Close ();
        }
    }

    // Return results
    return field;
}
```

#### <a name="displaying-data-and-responding-to-events"></a>Mostrar datos y responder a eventos

Para unir todas las piezas, edite el `SubviewSimpleBindingController` y darle un aspecto similar al siguiente:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Data;
using System.IO;
using Mono.Data.Sqlite;
using Foundation;
using AppKit;

namespace MacDatabase
{
    public partial class SubviewSimpleBindingController : AppKit.NSViewController
    {
        #region Private Variables
        private PersonModel _person = new PersonModel();
        private SqliteConnection Conn;
        #endregion

        #region Computed Properties
        //strongly typed view accessor
        public new SubviewSimpleBinding View {
            get {
                return (SubviewSimpleBinding)base.View;
            }
        }

        [Export("Person")]
        public PersonModel Person {
            get {return _person; }
            set {
                WillChangeValue ("Person");
                _person = value;
                DidChangeValue ("Person");
            }
        }

        public ComboBoxDataSource DataSource {
            get { return EmployeeSelector.DataSource as ComboBoxDataSource; }
        }
        #endregion

        #region Constructors
        // Called when created from unmanaged code
        public SubviewSimpleBindingController (IntPtr handle) : base (handle)
        {
            Initialize ();
        }

        // Called when created directly from a XIB file
        [Export ("initWithCoder:")]
        public SubviewSimpleBindingController (NSCoder coder) : base (coder)
        {
            Initialize ();
        }

        // Call to load from the XIB/NIB file
        public SubviewSimpleBindingController (SqliteConnection conn) : base ("SubviewSimpleBinding", NSBundle.MainBundle)
        {
            // Initialize
            this.Conn = conn;
            Initialize ();
        }

        // Shared initialization code
        void Initialize ()
        {
        }
        #endregion

        #region Private Methods
        private void LoadSelectedPerson (string id)
        {

            // Found?
            if (id != "") {
                // Yes, load requested record
                Person = new PersonModel (Conn, id);
            }
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Configure Employee selector dropdown
            EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");

            // Wireup events
            EmployeeSelector.Changed += (sender, e) => {
                // Get ID
                var id = DataSource.IDForValue (EmployeeSelector.StringValue);
                LoadSelectedPerson (id);
            };

            EmployeeSelector.SelectionChanged += (sender, e) => {
                // Get ID
                var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
                LoadSelectedPerson (id);
            };

            // Auto select the first person
            EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
            Person = new PersonModel (Conn, DataSource.IDForIndex(0));
    
        }
        #endregion
    }
}
```

El `DataSource` propiedad proporciona un acceso directo a la `ComboBoxDataSource` (creado anteriormente) adjunto al cuadro combinado.

El `LoadSelectedPerson` método carga la persona de la base de datos para el identificador único especificado:

```csharp
private void LoadSelectedPerson (string id)
{

    // Found?
    if (id != "") {
        // Yes, load requested record
        Person = new PersonModel (Conn, id);
    }
}
```

En el `AwakeFromNib` invalidación del método, se conecte por primera vez una instancia de nuestro origen de datos de cuadro combinado personalizado:

```csharp
EmployeeSelector.DataSource = new ComboBoxDataSource (Conn, "People", "Name");
```

A continuación, se responde al usuario editar el valor de texto del cuadro combinado, busque el identificador único asociado (`IDField`) de los datos se encuentran presentar y carga de la persona especificada si:

```csharp
EmployeeSelector.Changed += (sender, e) => {
    // Get ID
    var id = DataSource.IDForValue (EmployeeSelector.StringValue);
    LoadSelectedPerson (id);
};
```

También se carga a una nueva persona si el usuario selecciona un nuevo elemento en la lista desplegable:

```csharp
EmployeeSelector.SelectionChanged += (sender, e) => {
    // Get ID
    var id = DataSource.IDForIndex (EmployeeSelector.SelectedIndex);
    LoadSelectedPerson (id);
};
```

Por último, se rellenan automáticamente al cuadro combinado y persona mostrada con el primer elemento en la lista:

```csharp
// Auto select the first person
EmployeeSelector.StringValue = DataSource.ValueForIndex (0);
Person = new PersonModel (Conn, DataSource.IDForIndex(0));
```

## <a name="sqlitenet-orm"></a>SQLite.NET ORM

Como se indicó anteriormente, mediante el uso de código abierto [SQLite.NET](http://www.sqlite.org) Administrador de relaciones de objetos (ORM) podemos reducir en gran medida la cantidad de código necesario para leer y escribir datos desde una base de datos de SQLite. Esto puede no ser la mejor ruta que se realizará cuando el enlace de datos debido a varios de los requisitos que la codificación de clave-valor y enlace de datos se colocan en un objeto.

Según el sitio Web de SQLite.Net _"SQLite es una biblioteca de software que implementa un motor de base de datos SQL independiente, sin servidor, sin configuración, transaccional. SQLite es el motor de base de datos más ampliamente implementada en el mundo. El código fuente de SQLite es de dominio público"._

En las secciones siguientes, mostraremos cómo usar SQLite.Net para proporcionar datos para una vista de tabla.

### <a name="including-the-sqlitenet-nuget"></a>Incluido el paquete SQLite.net NuGet

SQLite.NET se presenta como un paquete de NuGet que incluyen en la aplicación. Antes de que podemos agregar compatibilidad de base de datos mediante SQLite.NET, es necesario incluir este paquete.

Siga este procedimiento para agregar el paquete:

1. En el **panel de solución**, haga clic en el **paquetes** carpeta y seleccione **agregar paquetes...**
2. Escriba `SQLite.net` en el **cuadro de búsqueda** y seleccione el **sqlite-net** entrada:

    [![Adición del paquete SQLite NuGet](databases-images/nuget01.png "adición del paquete SQLite NuGet")](databases-images/nuget01-large.png#lightbox)
3. Haga clic en el **Agregar paquete** para terminar.

### <a name="creating-the-data-model"></a>Crear el modelo de datos

Vamos a agregar una nueva clase al proyecto y llamar a en `OccupationModel`. A continuación, vamos a editar el **OccupationModel.cs** de archivos y darle un aspecto similar al siguiente:

```csharp
using System;
using SQLite;

namespace MacDatabase
{
    public class OccupationModel
    {
        #region Computed Properties
        [PrimaryKey, AutoIncrement]
        public int ID { get; set; }

        public string Name { get; set;}
        public string Description { get; set;}
        #endregion

        #region Constructors
        public OccupationModel ()
        {
        }

        public OccupationModel (string name, string description)
        {

            // Initialize
            this.Name = name;
            this.Description = description;

        }
        #endregion
    }
}
```

En primer lugar, se incluye SQLite.NET (`using Sqlite`), a continuación, se exponen varias propiedades, cada uno de los cuales se escribirán en la base de datos cuando se guarda este registro. La primera propiedad, asegúrese de que la clave principal y establézcalo como incremento automático como sigue:

```csharp
[PrimaryKey, AutoIncrement]
public int ID { get; set; }
```
### <a name="initializing-the-database"></a>Inicializar la base de datos

Con los cambios en nuestro modelo de datos en su lugar para admitir la lectura y escritura en la base de datos, es necesario abrir una conexión a la base de datos e inicialícelo en la primera ejecución. Vamos a agregar el código siguiente:

```csharp
using SQLite;
...

public SQLiteConnection Conn { get; set; }
...

private SQLiteConnection GetDatabaseConnection() {
    var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
    string db = Path.Combine (documents, "Occupation.db3");
    OccupationModel Occupation;

    // Create the database if it doesn't already exist
    bool exists = File.Exists (db);

    // Create connection to database
    var conn = new SQLiteConnection (db);

    // Initially populate table?
    if (!exists) {
        // Yes, build table
        conn.CreateTable<OccupationModel> ();

        // Add occupations
        Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
        conn.Insert (Occupation);

        Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
        conn.Insert (Occupation);
    }

    return conn;
}
```

En primer lugar, se obtiene una ruta de acceso a la base de datos (el escritorio del usuario en este caso) y vea si ya existe la base de datos:

```csharp
var documents = Environment.GetFolderPath (Environment.SpecialFolder.Desktop);
string db = Path.Combine (documents, "Occupation.db3");
OccupationModel Occupation;

// Create the database if it doesn't already exist
bool exists = File.Exists (db);
```

A continuación, establecemos una conexión a la base de datos en la ruta de acceso que hemos creado anteriormente:

```csharp
var conn = new SQLiteConnection (db);
```

Por último, se crea la tabla y agregar algunos registros de forma predeterminada:

```csharp
// Yes, build table
conn.CreateTable<OccupationModel> ();

// Add occupations
Occupation = new OccupationModel ("Documentation Manager", "Manages the Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Technical Writer", "Writes technical documentation and sample applications");
conn.Insert (Occupation);

Occupation = new OccupationModel ("Web & Infrastructure", "Creates and maintains the websites that drive documentation");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documentation Manager", "Manages the API Documentation Group");
conn.Insert (Occupation);

Occupation = new OccupationModel ("API Documenter", "Creates and maintains API documentation");
conn.Insert (Occupation);
```

### <a name="adding-a-table-view"></a>Agregar una vista de tabla

Como un ejemplo de uso, vamos a agregar una vista de tabla a nuestra UI en Interface builder de Xcode. Se expondrá esta vista de tabla a través de una salida (`OccupationTable`) por lo que podemos acceder a él a través de código de C#:

[![Exponer una toma de la vista de tabla](databases-images/table01.png "exponer una toma de la vista de tabla")](databases-images/table01-large.png#lightbox)

A continuación, vamos a agregar las clases personalizadas para rellenar esta tabla con los datos de la base de datos de SQLite.NET.

### <a name="creating-the-table-data-source"></a>Creación de la tabla de origen de datos

Vamos a crear un origen de datos personalizado para proporcionar datos para la tabla. En primer lugar, agregue una nueva clase denominada `TableORMDatasource` y darle un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDatasource : NSTableViewDataSource
    {
        #region Computed Properties
        public List<OccupationModel> Occupations { get; set;} = new List<OccupationModel>();
        public SQLiteConnection Conn { get; set; }
        #endregion

        #region Constructors
        public TableORMDatasource (SQLiteConnection conn)
        {
            // Initialize
            this.Conn = conn;
            LoadOccupations ();
        }
        #endregion

        #region Public Methods
        public void LoadOccupations() {

            // Get occupations from database
            var query = Conn.Table<OccupationModel> ();

            // Copy into table collection
            Occupations.Clear ();
            foreach (OccupationModel occupation in query) {
                Occupations.Add (occupation);
            }

        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Occupations.Count;
        }
        #endregion
    }
}
```

Cuando se crea una instancia de esta clase más adelante, pasamos en nuestra conexión abierta de base de datos de SQLite.NET. El `LoadOccupations` método consulta la base de datos y copia los registros se encuentra en la memoria (con nuestro `OccupationModel` modelo de datos).

### <a name="creating-the-table-delegate"></a>Creación del delegado de la tabla

La clase final que necesitamos es un delegado de tabla personalizado para mostrar la información que se ha cargado desde la base de datos de SQLite.NET. Vamos a agregar un nuevo `TableORMDelegate` a nuestro proyecto y dele un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;
using SQLite;

namespace MacDatabase
{
    public class TableORMDelegate : NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "OccCell";
        #endregion

        #region Private Variables
        private TableORMDatasource DataSource;
        #endregion

        #region Constructors
        public TableORMDelegate (TableORMDatasource dataSource)
        {
            // Initialize
            this.DataSource = dataSource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Occupation":
                view.StringValue = DataSource.Occupations [(int)row].Name;
                break;
            case "Description":
                view.StringValue = DataSource.Occupations [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Aquí usamos el origen de datos `Occupations` colección (que se carga desde la base de datos de SQLite.NET) para rellenar las columnas de la tabla a través de la `GetViewForItem` invalidación del método.

### <a name="populating-the-table"></a>Rellenando la tabla

Con todas las piezas en su lugar, vamos a rellenar nuestra tabla al que se aumenta desde el archivo .xib invalidando el `AwakeFromNib` método y haciendo que se parezca a lo siguiente:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Get database connection
    Conn = GetDatabaseConnection ();

    // Create the Occupation Table Data Source and populate it
    var DataSource = new TableORMDatasource (Conn);

    // Populate the Product Table
    OccupationTable.DataSource = DataSource;
    OccupationTable.Delegate = new TableORMDelegate (DataSource);
}
```

En primer lugar, se obtenga acceso a nuestra base de datos de SQLite.NET, crear y rellenar, si aún no existe. A continuación, creamos una nueva instancia de nuestro origen de datos de tabla personalizado, pasar nuestra conexión de base de datos y adjuntarla a la tabla. Por último, crearemos una nueva instancia de nuestro delegado personalizado de la tabla, pase a nuestro origen de datos y adjuntarla a la tabla.

## <a name="summary"></a>Resumen

En este artículo ha tomado una visión detallada de trabajar con el enlace de datos y codificación de pares clave-valor con bases de datos de SQLite en una aplicación de Xamarin.Mac. En primer lugar, examinamos expone una clase de C# y Objective-C con codificación de clave-valor (KVC) y pares clave-valor observando (KVO). A continuación, se ha mostrado cómo usar una clase conforme KVO y enlazar datos a los elementos de interfaz de usuario de Interface Builder de Xcode. El artículo también trata trabajar con datos de SQLite a través de la SQLite.NET ORM y mostrar esos datos en una vista de tabla.



## <a name="related-links"></a>Vínculos relacionados

- [MacDatabase (ejemplo)](https://developer.xamarin.com/samples/mac/MacDatabase/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Enlace de datos y la clave y valor de codificación](~/mac/app-fundamentals/databinding.md)
- [Controles estándar](~/mac/user-interface/standard-controls.md)
- [Vistas de tabla](~/mac/user-interface/table-view.md)
- [Vistas de esquema](~/mac/user-interface/outline-view.md)
- [Vistas de colección](~/mac/user-interface/collection-view.md)
- [Guía de programación de codificación de pares clave-valor](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/KeyValueCoding/index.html)
- [Introducción a los enlaces de cacao temas de programación](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/CocoaBindings/CocoaBindings.html)
- [Introducción a la referencia de enlaces de Cocoa](https://developer.apple.com/library/content/documentation/Cocoa/Reference/CocoaBindingsRef/CocoaBindingsRef.html)
- [NSCollectionView](https://developer.apple.com/documentation/appkit/nscollectionview)
- [Directrices de la interfaz humana de macOS](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
