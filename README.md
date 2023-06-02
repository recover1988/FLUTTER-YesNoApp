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
