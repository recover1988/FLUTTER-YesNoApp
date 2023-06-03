# yes_no_app

A new Flutter project.

## Getting Started

This project is a starting point for a Flutter application.

A few resources to get you started if this is your first Flutter project:

-   [Lab: Write your first Flutter app](https://docs.flutter.dev/get-started/codelab)
-   [Cookbook: Useful Flutter samples](https://docs.flutter.dev/cookbook)

For help getting started with Flutter development, view the
[online documentation](https://docs.flutter.dev/), which offers tutorials,
samples, guidance on mobile development, and a full API reference.

# Yes_No_App

En esta sección haremos el diseño de la aplicación de YesNo, la cual eventualmente responderá nuestras preguntas (siempre y cuando sean de si o no).

En esta sección veremos:

TextEditingControllers

Focus Nodes

ThemeData

Widgets como:

Containers

SizeBox

ListViews

CustomWidgets

Expanded

Padding

Image (desde internet)

ClipRRect (bordes redondeados)

## Snipped

Para generar un esqueleto de aplicacion podemos usar el snipped `mate`.
Para quitar la banda de debugg mode usamos `debugShowCheckedModeBanner` y lo ponemos en `false`.
Si queremos un boton relleno podemos usar `FilledButton.tonal(onPressed: () {}, child: const Text('Click me')),` este botono no puede ser constante per su texto si.
`stless` -> para crear un widget stateless.
para ver emoticones `windows + .`

## Cambiar Color de la Aplicacion

Para poder cambiar el color de la aplicaion podemos usar la opcion `theme:` que acepta un objeto de tipo `ThemeData` quue tiene en sus opciones las configuraciones globales.

```
.lib/config/theme/app_theme.dart

import 'package:flutter/material.dart';

const Color _customColor = Color(0xFF5C11D4);

const List<Color> _colorThemes = [
  _customColor,
  Colors.teal,
  Colors.yellow,
  Colors.orange,
  Colors.black,
  Colors.green,
  Colors.deepPurple,
];

class AppTheme {
  final int selectedColor;
  AppTheme({this.selectedColor = 0})
      : assert(selectedColor >= 0 && selectedColor < _colorThemes.length,
            'Color must be between 0 and ${_colorThemes.length}');
  ThemeData theme() {
    return ThemeData(
        useMaterial3: true,
        colorSchemeSeed: _colorThemes[selectedColor],
        brightness: Brightness.dark);
  }
}
```

En esta clase se construye el ThemeData y se pasa algunos parametros como los `Colors` que son los colores que vienen, ademas podemos agregar colores custom agregando al id del color un `0xFF` adelante para indicar que es un color rgb.
El `ThemeData` acepta algunas confiruaciones como:
. useMaterial3: boolean -> que permite usar la config.
. colorSchemeSeed: string -> aca se indica el color de la aplicacion
. brightness: Brightness.dark ->la config para modo oscuro.

Para usar esta clase tenemos q importarla y luego invocarlas

```
    theme: AppTheme(selectedColor: 4).theme(),
```

## Widget ChatScreen

```
import 'package:flutter/material.dart';

class ChatScreen extends StatelessWidget {
  const ChatScreen({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        leading: const Padding(
          padding: EdgeInsets.all(4.0),
          child: CircleAvatar(
            backgroundImage: NetworkImage(
                'https://cdn-icons-png.flaticon.com/256/3874/3874001.png'),
          ),
        ),
        title: const Text('Mi amor ❤'),
        centerTitle: true,
      ),
    );
  }
}
```

Importamos material y con el snipped creamos el Stateless Widget. Este retorna un Scaffold que tiene muchas cosas como `appBar` de tipo `AppBar`, aca ponemos las propiedades de `leading:` que sirve para tene un pequeño incono en la parte superior izquierda.
. `CircleAvatar()` el cual es in circulo para pones desde imagenes.
. `Icons.` con esto podemos elegir un icono.
. `Padding` que sirve para reducir el tamaño.

## ListView

En el `body` del `Scaffold` podemos crear un Container que vendria a ser como un div que requiere dimensiones o un `Expanded()` el cual usa todo el tamaño de la pantalla.
Dentro del `body` podemos llamar a una clase que devuelva un `ListView.builder()` que requiere `itemBuilder` para renderizar elementos.
El `SafeArea` es un widget que permite dejar espacio para los botones de la pantalla.
El `Padding` con un `padding` de `EdgeInsets.symmetric(horizontal: 20)` nos permite dejar un espacio en el eje X.
Se usa un Widget `Column` para tener los child uno encima del otro.

```
class _ChatView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return SafeArea(
      child: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 20),
        child: Column(
          children: [
            Expanded(child: ListView.builder(
              itemBuilder: (context, index) {
                return Text('Indice: $index');
              },
            )),
            Text('Hola'),
            Text('Mundo')
          ],
        ),
      ),
    );
  }
}
```

## Mis Mensajes - Burbuja de Chat
