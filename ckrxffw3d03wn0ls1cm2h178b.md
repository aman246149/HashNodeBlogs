## How I Convert My HashNode Blog In-App Using 15 Lines Of Code

## I convert my Hashnode blog into an App by using **flutter** only using few Lines of Code.

Here I am using a flutter package    [flutter_webview_plugin: ^0.4.0](https://pub.dev/packages/flutter_webview_plugin) 

Install this package in your pubspec file 

Now here is a code, <br>
 you have to pass your blog URL in URL section.


```
import 'package:flutter/material.dart';
import 'package:flutter_webview_plugin/flutter_webview_plugin.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  @override
  Widget build(BuildContext context) {
    return WebviewScaffold(url: 'https://hashnode.com/@amandev',);
  }
}

``` 

![WhatsApp Image 2021-08-04 at 5.06.07 PM.jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1628077375617/EksseRC6O.jpeg)

![WhatsApp Image 2021-08-04 at 5.06.07 PM (1).jpeg](https://cdn.hashnode.com/res/hashnode/image/upload/v1628077385968/Ht4GsJTnV.jpeg)

**Here is a screenShot what it looks like in mobile**