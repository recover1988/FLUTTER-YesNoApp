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

## Burbuja de Chat

### Mis Mensajes

```
import 'package:flutter/material.dart';

class MyMessageBubble extends StatelessWidget {
  const MyMessageBubble({super.key});

  @override
  Widget build(BuildContext context) {
    final colors = Theme.of(context).colorScheme;
    return Column(
      crossAxisAlignment: CrossAxisAlignment.end,
      children: [
        Container(
          decoration: BoxDecoration(
              color: colors.primary, borderRadius: BorderRadius.circular(20)),
          child: const Padding(
            padding: EdgeInsets.symmetric(horizontal: 20, vertical: 10),
            child: Text(
              'Hola',
              style: TextStyle(color: Colors.black),
            ),
          ),
        ),
        const SizedBox(
          height: 10,
        )
      ],
    );
  }
}
```

`Theme.of(context)` -> obtenemos valores desde el contexto en este caso `colorScheme`.
`crossAxisAlignment: CrossAxisAlignment.end,` -> es la posicion que tienen los widget de la columna.
`decoration: BoxDecoration()` modificamos los estilos del `Container` como el color y borderRadius.
`SizedBox()` vendria a ser el tamaño que tiene el container.

### Los mensajes del otro

```
import 'package:flutter/material.dart';

class HerMessageBubble extends StatelessWidget {
  const HerMessageBubble({super.key});

  @override
  Widget build(BuildContext context) {
    final colors = Theme.of(context).colorScheme;
    return Column(
      crossAxisAlignment: CrossAxisAlignment.start,
      children: [
        Container(
          decoration: BoxDecoration(
              color: colors.secondary, borderRadius: BorderRadius.circular(20)),
          child: const Padding(
            padding: EdgeInsets.symmetric(horizontal: 20, vertical: 10),
            child: Text(
              'Aute elit reprehenderit ea dolore irure exercitation laboris nulla veniam reprehenderit occaecat ullamco.',
              style: TextStyle(color: Colors.black),
            ),
          ),
        ),
        const SizedBox(
          height: 5,
        ),
        //todo: imagen
        _ImageBubble(),
        const SizedBox(
          height: 10,
        )
      ],
    );
  }
}

class _ImageBubble extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    return ClipRRect(
        borderRadius: BorderRadius.circular(20),
        child: Image.network(
            'https://yesno.wtf/assets/no/11-e6b930256265890554c1464973ebba55.gif',
            width: size.width * 0.7,
            height: 150,
            fit: BoxFit.cover));
  }
}
```

`final size = MediaQuery.of(context).size;` esta propiedad nos permite obtener las dimensiones del dispositivo donde se esta ejecutando.
`ClipRRect` crea un rectangulo con los bordes redondeados.

## Loading Image

```
class _ImageBubble extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final size = MediaQuery.of(context).size;
    return ClipRRect(
        borderRadius: BorderRadius.circular(20),
        child: Image.network(
          'https://yesno.wtf/assets/no/11-e6b930256265890554c1464973ebba55.gif',
          width: size.width * 0.7,
          height: 150,
          fit: BoxFit.cover,
          loadingBuilder: (context, child, loadingProgress) {
            if (loadingProgress == null) return child;
            return Container(
              width: size.width * 0.7,
              height: 150,
              padding: const EdgeInsets.symmetric(horizontal: 10, vertical: 5),
              child: const Text('Mi amor esta enviando una imagen.'),
            );
          },
        ));
  }
}
```

`Image.network` tiene una propiedad llamada `loadingBuilder` el cual acepta una funcion que nos permite tener cierto parametros como:
. context -> es el entorno
. child -> que en este caso es la imagen cargada
. loadingProgress -> que es el estado de la consulta si es `null` siginifica que hay respuesta

## TextFormField

```
import 'package:flutter/material.dart';

class MessageFieldBox extends StatelessWidget {
  const MessageFieldBox({super.key});

  @override
  Widget build(BuildContext context) {
    final colors = Theme.of(context).colorScheme; <- Obtenemos colores

    final outLineInputBorder = UnderlineInputBorder( <- estilos del borde
        borderSide: BorderSide(color: colors.primary),
        borderRadius: BorderRadius.circular(40));

    final inputDecoration = InputDecoration( <- stilos del input
      enabledBorder: outLineInputBorder,
      focusedBorder: outLineInputBorder,
      filled: true,
      suffixIcon: IconButton( <- agregamos un icono
        icon: const Icon(Icons.send_outlined),
        onPressed: () {
          print('Valor de la caja de texto ');
        },
      ),
    );

    return TextFormField(
      decoration: inputDecoration, <- estilos que tiene el textformfield
      onFieldSubmitted: (value) { <- cuando hacemos el submit
        print('Submit value $value');
      },
      onChanged: (value) { <- cuando cambiamos algun valor
        print('onChanged value $value');
      },
    );
  }
}
```

## Comportamiento del formfield

```
import 'package:flutter/material.dart';

class MessageFieldBox extends StatelessWidget {
  const MessageFieldBox({super.key});

  @override
  Widget build(BuildContext context) {
    final textController = TextEditingController(); <- obtenemos el texto
    final focusNode = FocusNode(); <- obtenemos el foco del elemento para poder usarlo

    final colors = Theme.of(context).colorScheme;

    final outLineInputBorder = UnderlineInputBorder(
        borderSide: BorderSide(color: colors.primary),
        borderRadius: BorderRadius.circular(40));

    final inputDecoration = InputDecoration(
      hintText: 'End your message with a "??"', <- placeholder
      enabledBorder: outLineInputBorder, <- estilos cuando esta habilitado
      focusedBorder: outLineInputBorder, <- estilos cuando esta focuseado
      filled: true,
      suffixIcon: IconButton(
        icon: const Icon(Icons.send_outlined),
        onPressed: () {
          final textValue = textController.value.text;
          print('Valor de la caja de texto: $textValue ');
          textController.clear(); <- limpiamos el texto
        },
      ),
    );

    return TextFormField(
      onTapOutside: (event) { <- evento que sucede cuando hacemos click fuera
        focusNode.unfocus(); <- quitamos el foco del widget
      },
      keyboardAppearance: colors.brightness, <- modifica apariencia del teclado
      focusNode: focusNode, <- mantenga el foco
      controller: textController,
      decoration: inputDecoration,
      onFieldSubmitted: (value) {
        print('Submit value $value');
        textController.clear();
        focusNode.requestFocus(); <- mantenemos el foco
      },
    );
  }
}
```

## Entidad - Message

Para la logica de negocio tenemos que usar Dart puro, ya que estos no se crea widget.

```
lib/domain/entities/message.dart

enum FromWho { mine, hers }

class Message {
  final String text;
  final String? imageUrl;
  final FromWho fromWho;

  Message({required this.text, this.imageUrl, required this.fromWho});
}

```

Con este codigo hacems un `enum`. Y inicializamos la clase pidiendo en el constructor que los valores de text y fromWho sean requeridos.

## Install Packet

Para instalar un paquete podemos buscarlos en pub.dev. Y asegurarnos que es compatible con el entrono de trabajo que tenemos.
Para instalar un paquete podemos usar el comando

```
flutter pub add provider
```

Otra forma es agregarlo en el archivo `pubspec.yaml`.
Y la ultima es usar la extension de VSC llamada Pubspec Assist que permite instalar desde el promt de vsc.

## Uso Provider

Una vez instalado el provider podemos usarlo en el rango mas alto de aplicacion para que pueda ser usado en toda las widgets que lo necesiten.

```
void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MultiProvider(
      providers: [ChangeNotifierProvider(create: (_) => ChatProvider())],
      child: MaterialApp(
          title: 'Yes No App',
          debugShowCheckedModeBanner: false,
          theme: AppTheme(selectedColor: 0).theme(),
          home: const ChatScreen()),
    );
  }
}
```

En el main podemos envolver la aplicaicon en un `MultiProvider` el cual requeire el o los providers que son lo elementos que se compartiran.

### Chat Provider

El ChatProvider es la instancia que se va a guardar en el estado

```
import 'package:flutter/material.dart';
import 'package:yes_no_app/domain/entities/message.dart';

class ChatProvider extends ChangeNotifier {
  List<Message> messageList = [
    Message(text: 'Hola', fromWho: FromWho.me),
    Message(text: 'Ya regresaste del trabajo', fromWho: FromWho.me)
  ];

  Future<void> sendMessage(String text) async {
    //todo: implementar metodo.
  }
}
```

### Añadir mensajes al provider

Para esto tenemos que añadir un metodo al `chat_provider.dart` que agregue datos o `Message` a la lista que se encuentra en el estado.
Se puede ver que estamos añadiendo el metodo `sendMessage` que agrega a la lista y luego con el `notifyListeners()` le decimos que renderize.

```
lib\presentation\providers\chat_provider.dart

import 'package:flutter/material.dart';
import 'package:yes_no_app/domain/entities/message.dart';

class ChatProvider extends ChangeNotifier {
  List<Message> messageList = [
    Message(text: 'Hola', fromWho: FromWho.me),
    Message(text: 'Ya regresaste del trabajo', fromWho: FromWho.me)
  ];

  Future<void> sendMessage(String text) async {
    final newMessage = Message(text: text, fromWho: FromWho.me);
    messageList.add(newMessage);
    notifyListeners();
  }
}
```

De la caja de texto hacemos que el mensaje se guarde en una variable para luego utilizarla cuando se sube.

```
lib\presentation\widgets\shared\message_field_box.dart

import 'package:flutter/material.dart';

class MessageFieldBox extends StatelessWidget {
  final ValueChanged<String> onValue;    <---
  const MessageFieldBox({super.key, required this.onValue});

  @override
  Widget build(BuildContext context) {
    final textController = TextEditingController();
    final focusNode = FocusNode();

    final colors = Theme.of(context).colorScheme;

    final outLineInputBorder = UnderlineInputBorder(
        borderSide: BorderSide(color: colors.primary),
        borderRadius: BorderRadius.circular(40));

    final inputDecoration = InputDecoration(
      hintText: 'End your message with a "?"',
      enabledBorder: outLineInputBorder,
      focusedBorder: outLineInputBorder,
      filled: true,
      suffixIcon: IconButton(
        icon: const Icon(Icons.send_outlined),
        onPressed: () {
          final textValue = textController.value.text;
          onValue(textValue);        <---
          textController.clear();
        },
      ),
    );

    return TextFormField(
      onTapOutside: (event) {
        focusNode.unfocus();
      },
      keyboardAppearance: colors.brightness,
      focusNode: focusNode,
      controller: textController,
      decoration: inputDecoration,
      onFieldSubmitted: (value) {
        onValue(value);         <---
        textController.clear();
        focusNode.requestFocus();
      },
    );
  }
}
```

Y por ultimo unimos todo donde tenemos instaciada el provider que es el `chat_screen.dart` .

```
lib\presentation\screens\chat\chat_screen.dart

import 'package:flutter/material.dart';
import 'package:provider/provider.dart';
import 'package:yes_no_app/domain/entities/message.dart';
import 'package:yes_no_app/presentation/providers/chat_provider.dart';
import 'package:yes_no_app/presentation/widgets/chat/her_message_bubble.dart';
import 'package:yes_no_app/presentation/widgets/chat/my_message_bubble.dart';
import 'package:yes_no_app/presentation/widgets/shared/message_field_box.dart';

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
      body: _ChatView(),
    );
  }
}

class _ChatView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final chatProvider = context.watch<ChatProvider>();

    return SafeArea(
      child: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 20),
        child: Column(
          children: [
            Expanded(
                child: ListView.builder(
              itemCount: chatProvider.messageList.length,
              itemBuilder: (context, index) {
                final message = chatProvider.messageList[index];
                return (message.fromWho == FromWho.hers)
                    ? const HerMessageBubble()
                    : MyMessageBubble(message: message);
              },
            )),
            // Caja de Texto          <--------
            MessageFieldBox(
              // onValue: (String value) => chatProvider.sendMessage(value),
              onValue: chatProvider.sendMessage,
            ),
          ],
        ),
      ),
    );
  }
}
```

## Mover el Scroll al final

Primero hacemos la conexion del controlador del `ListView.builder()` con el controlador del `chatProvider`, de esta forma podemos manejar el scroll desde el provider.

```
class _ChatView extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final chatProvider = context.watch<ChatProvider>();

    return SafeArea(
      child: Padding(
        padding: const EdgeInsets.symmetric(horizontal: 20),
        child: Column(
          children: [
            Expanded(
                child: ListView.builder(
              controller: chatProvider.chatScrollController,
              itemCount: chatProvider.messageList.length,
              itemBuilder: (context, index) {
                final message = chatProvider.messageList[index];
                return (message.fromWho == FromWho.hers)
                    ? const HerMessageBubble()
                    : MyMessageBubble(message: message);
              },
            )),
            // Caja de Texto
            MessageFieldBox(
              // onValue: (String value) => chatProvider.sendMessage(value),
              onValue: chatProvider.sendMessage,
            ),
          ],
        ),
      ),
    );
  }
}
```

En el provider creamos la variable final ` final ScrollController chatScrollController = ScrollController();` y con la funcion `moveScrollToBottom` hacemos que se vea en la parte de abajo.

```
import 'package:flutter/material.dart';
import 'package:yes_no_app/domain/entities/message.dart';

class ChatProvider extends ChangeNotifier {
  final ScrollController chatScrollController = ScrollController();
  List<Message> messageList = [
    Message(text: 'Hola', fromWho: FromWho.me),
    Message(text: 'Ya regresaste del trabajo', fromWho: FromWho.me)
  ];

  Future<void> sendMessage(String text) async {
    if (text.isEmpty) return;
    final newMessage = Message(text: text, fromWho: FromWho.me);
    messageList.add(newMessage);
    notifyListeners();
    moveScrollToBottom();
  }

  void moveScrollToBottom() async {
    await Future.delayed(const Duration(milliseconds: 100));
    chatScrollController.animateTo(
        chatScrollController.position.maxScrollExtent, <- Fijamos posiciones
        duration: const Duration(milliseconds: 300),  <- ponemos duracion
        curve: Curves.easeOut); <- le damos la animacion
  }
}

```
