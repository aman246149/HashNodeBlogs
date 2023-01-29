# Flutter Local Notifications With WorkManager

```dart

import 'package:flutter/material.dart';
import 'package:flutter/widgets.dart';
import 'package:flutter_local_notifications/flutter_local_notifications.dart';


import 'package:timezone/timezone.dart' as tz;

class NotificationService {
  NotificationService();

  final _localNotifications = FlutterLocalNotificationsPlugin();


  Future<void> initializePlatformNotifications() async {
    const AndroidInitializationSettings initializationSettingsAndroid =
        AndroidInitializationSettings('@mipmap/ic_launcher');

    const InitializationSettings initializationSettings =
        InitializationSettings(
      android: initializationSettingsAndroid,
    );

    await _localNotifications.initialize(initializationSettings,
        onDidReceiveNotificationResponse: onDidReceiveLocalNotification,
        onDidReceiveBackgroundNotificationResponse:
            onDidReceiveBackgroundNotificationResponse);
  }

  void onDidReceiveLocalNotification(NotificationResponse? response) {
    var data = response?.payload.toString();

    // if (response!.id == 0) {
    //   Navigator.push(
    //       context,
    //       MaterialPageRoute(
    //         builder: (context) => SecondScreen(data: response.payload!),
    //       ));
    // } else {
    //   Navigator.push(
    //       context,
    //       MaterialPageRoute(
    //         builder: (context) => ThirdScreen(),
    //       ));
    // }
  }

  static void onDidReceiveBackgroundNotificationResponse(
      NotificationResponse? response) {
    var data = response?.payload.toString();
    print("onDidReceiveBackgroundNotificationResponse");
    print(data);
  }

  // void selectNotification(String? payload) {
  //   if (payload != null && payload.isNotEmpty) {}
  // }

  Future<NotificationDetails> _notificationDetails() async {
    // final bigPicture = await DownloadUtil.downloadAndSaveFile(
    //     "https://images.unsplash.com/photo-1624948465027-6f9b51067557?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1470&q=80",
    //     "drinkwater");

    AndroidNotificationDetails androidPlatformChannelSpecifics =
        const AndroidNotificationDetails(
      'channel id',
      'channel name',
      groupKey: 'com.example.flutter_push_notifications',
      channelDescription: 'channel description',
      importance: Importance.max,
      priority: Priority.max,
      playSound: true,
      ticker: 'ticker',
      // largeIcon: const DrawableResourceAndroidBitmap('justwater'),
      // styleInformation: BigPictureStyleInformation(
      //   FilePathAndroidBitmap(bigPicture),
      //   hideExpandedLargeIcon: false,
      // ),
      color: Color(0xff2196f3),
    );

    final details = await _localNotifications.getNotificationAppLaunchDetails();
    if (details != null && details.didNotificationLaunchApp) {}
    NotificationDetails platformChannelSpecifics = NotificationDetails(
      android: androidPlatformChannelSpecifics,
    );

    return platformChannelSpecifics;
  }

  Future<void> showLocalNotification({
    required int id,
    required String title,
    required String body,
    required String payload,
  }) async {
    final platformChannelSpecifics = await _notificationDetails();
    await _localNotifications.show(
      id,
      title,
      body,
      platformChannelSpecifics,
      payload: payload,
    );
  }

  Future<void> showScheduledLocalNotification({
    required int id,
    required String title,
    required String body,
    required String payload,
    required int seconds,
  }) async {
    final platformChannelSpecifics = await _notificationDetails();
    await _localNotifications.zonedSchedule(
      id,
      title,
      body,
      tz.TZDateTime.now(tz.local).add(Duration(seconds: seconds)),
      platformChannelSpecifics,
      payload: payload,
      uiLocalNotificationDateInterpretation:
          UILocalNotificationDateInterpretation.absoluteTime,
      androidAllowWhileIdle: true,
    );
  }

  Future<void> showPeriodicLocalNotification({
    required int id,
    required String title,
    required String body,
    required String payload,
  }) async {
    final platformChannelSpecifics = await _notificationDetails();
    await _localNotifications.periodicallyShow(
      id,
      title,
      body,
      RepeatInterval.everyMinute,
      platformChannelSpecifics,
      payload: payload,
      androidAllowWhileIdle: true,
    );
  }
}
```

Initialize it in maindart file

```dart
import 'dart:developer';

import 'package:flutter/material.dart';
import 'package:portfolio/service/notification_service.dart';
import 'package:portfolio/weather/api/api.dart';
import 'package:portfolio/weather/model/weather_model.dart';
import 'package:portfolio/weather/screens/weather_screen.dart';
import 'package:permission_handler/permission_handler.dart';
import 'package:timezone/data/latest.dart' as tz;
import 'package:workmanager/workmanager.dart';

@pragma('vm:entry-point')
void callbackDispatcher() {
  // callNotification();
  Workmanager().executeTask((taskName, inputData) async {
    try {
      await callNotification();
    } catch (e) {
      print(e.toString());
    }

    return Future.value(true);
  });
}

Future<void> callNotification() async {
  WeatherModel weatherModel = await Api.getData("Kotdwara");
  await NotificationService().showLocalNotification(
      id: 0,
      title: "Your Today Weather",
      body: "${weatherModel.current!.tempC!} celcius",
      payload: "");

  return Future.value(true);
}

void main() async {
  WidgetsFlutterBinding.ensureInitialized();
  await Workmanager().initialize(
    callbackDispatcher,
  );
  await NotificationService().initializePlatformNotifications();
  await Workmanager().registerPeriodicTask("fetchApi", "fetchWeatherApi",
      initialDelay: Duration(seconds: 5),
      frequency: Duration(minutes: 16),
      tag: "apifetchtag");
  tz.initializeTimeZones();
  await Permission.notification.isDenied.then((value) {
    if (value) {
      Permission.notification.request();
    }
  });

  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      debugShowCheckedModeBanner: false,
      title: 'Flutter Demo',
      theme: ThemeData(
        useMaterial3: true,
        primarySwatch: Colors.blue,
      ),
      home: const WeatherScreen(),
    );
  }
}
```