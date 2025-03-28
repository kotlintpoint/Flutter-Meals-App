# Call, SMS, and URL Launcher in Flutter

This guide demonstrates how to implement phone call, SMS, and URL launching functionalities using the `url_launcher` package in Flutter.

## ✅ Prerequisites
- Flutter SDK installed
- A Flutter project created using `flutter create my_app`

## 🛠️ Step 1: Install `url_launcher`
Add the following to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  url_launcher: ^6.2.5
```

Run the command:
```bash
flutter pub get
```

---

## 📱 Step 2: Configure Permissions

### **For Android**
Add the following permissions to `android/app/src/main/AndroidManifest.xml`:

```xml
<uses-permission android:name="android.permission.CALL_PHONE" />
<uses-permission android:name="android.permission.SEND_SMS" />
<uses-permission android:name="android.permission.INTERNET" />
```

### **For iOS**
Open `ios/Runner/Info.plist` and add:

```xml
<key>NSAppTransportSecurity</key>
<dict>
  <key>NSAllowsArbitraryLoads</key>
  <true/>
</dict>
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>tel</string>
  <string>sms</string>
  <string>mailto</string>
  <string>http</string>
  <string>https</string>
</array>
```

---

## 🚀 Step 3: Import the Package
```dart
import 'package:flutter/material.dart';
import 'package:url_launcher/url_launcher.dart';
```

---

## 📞 Step 4: Make a Phone Call
```dart
Future<void> makePhoneCall(String phoneNumber) async {
  final Uri phoneUri = Uri(scheme: 'tel', path: phoneNumber);
  if (await canLaunchUrl(phoneUri)) {
    await launchUrl(phoneUri);
  } else {
    throw 'Could not launch $phoneUri';
  }
}
```
**Example:** `makePhoneCall('1234567890');`

---

## 📩 Send an SMS
```dart
Future<void> sendSMS(String phoneNumber, String message) async {
  final Uri smsUri = Uri(scheme: 'sms', path: phoneNumber, queryParameters: {'body': message});
  if (await canLaunchUrl(smsUri)) {
    await launchUrl(smsUri);
  } else {
    throw 'Could not launch $smsUri';
  }
}
```
**Example:** `sendSMS('1234567890', 'Hello from Flutter!');`

---

## 🌐 Launch a URL
```dart
Future<void> launchWebsite(String url) async {
  final Uri uri = Uri.parse(url);
  if (await canLaunchUrl(uri)) {
    await launchUrl(uri);
  } else {
    throw 'Could not launch $uri';
  }
}
```
**Example:** `launchWebsite('https://flutter.dev');`

---

## 🖥️ Example UI with Buttons
```dart
class HomeScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Launcher Example')),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () => makePhoneCall('1234567890'),
              child: Text('Make a Call'),
            ),
            ElevatedButton(
              onPressed: () => sendSMS('1234567890', 'Hello from Flutter!'),
              child: Text('Send SMS'),
            ),
            ElevatedButton(
              onPressed: () => launchWebsite('https://flutter.dev'),
              child: Text('Open Flutter Website'),
            ),
          ],
        ),
      ),
    );
  }
}
```

---

## ⚙️ Additional Tips
- Ensure the emulator or device has a calling or messaging app installed.
- On Android, permissions are required for phone calls and SMS.
- On iOS, calling and SMS might not work on simulators.

---

## 🚧 Troubleshooting
- **Error: Could not launch URL**: Ensure the device has a default app for phone, SMS, or browser.
- **Permission Denied**: Verify the required permissions are correctly added for Android and iOS.

---

## 📜 Conclusion
You have successfully implemented Call, SMS, and URL Launcher functionality in your Flutter app using `url_launcher`. Happy coding! 😊

