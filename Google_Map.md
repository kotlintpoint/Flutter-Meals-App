# Integrating Google Maps in Flutter

This guide will help you integrate Google Maps into your Flutter application using the `google_maps_flutter` package.

---

## ✅ Prerequisites
- Flutter installed
- A Google Cloud Project with Google Maps API enabled
- API key for Google Maps

---

## 🛠️ Step 1: Install the Package
Add the following to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter: ^2.5.0
```

Run:
```bash
flutter pub get
```

---

## 🔑 Step 2: Configure Google Maps API Key

### **For Android**
1. Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. Enable the **Maps SDK for Android**.
3. Create an API key.
4. Add the following to `android/app/src/main/AndroidManifest.xml`:

```xml
<application>
    <meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY" />
</application>
```

### **For iOS**
1. Enable **Maps SDK for iOS** in Google Cloud Console.
2. Add your API key to `ios/Runner/AppDelegate.swift`:

```swift
import GoogleMaps

GMSServices.provideAPIKey("YOUR_API_KEY")
```

3. Add the following to `ios/Runner/Info.plist`:

```xml
<key>io.flutter.embedded_views_preview</key>
<true/>
```

---

## 📦 Step 3: Import the Package
```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';
```

---

## 🗺️ Step 4: Create a Map Widget
Here is how you can display a Google Map with a marker:

```dart
class MapScreen extends StatefulWidget {
  @override
  _MapScreenState createState() => _MapScreenState();
}

class _MapScreenState extends State<MapScreen> {
  GoogleMapController? mapController;

  final LatLng initialPosition = LatLng(37.7749, -122.4194); // San Francisco coordinates

  void _onMapCreated(GoogleMapController controller) {
    mapController = controller;
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Google Maps Example')),
      body: GoogleMap(
        onMapCreated: _onMapCreated,
        initialCameraPosition: CameraPosition(
          target: initialPosition,
          zoom: 11.0,
        ),
        markers: {
          Marker(
            markerId: MarkerId('1'),
            position: initialPosition,
            infoWindow: InfoWindow(title: 'San Francisco'),
          ),
        },
      ),
    );
  }
}
```

---

## 🧑‍💻 Additional Features

### 📍 **Add Multiple Markers**
```dart
Set<Marker> markers = {
  Marker(markerId: MarkerId('1'), position: LatLng(37.7749, -122.4194), infoWindow: InfoWindow(title: 'San Francisco')),
  Marker(markerId: MarkerId('2'), position: LatLng(34.0522, -118.2437), infoWindow: InfoWindow(title: 'Los Angeles')),
};
```

### 🌍 **Change Map Type**
```dart
GoogleMap(
  mapType: MapType.hybrid, // Options: normal, satellite, terrain, hybrid
  ...
)
```

### 🚗 **Add a Polyline for Route**
```dart
Polyline polyline = Polyline(
  polylineId: PolylineId('route'),
  color: Colors.blue,
  width: 5,
  points: [LatLng(37.7749, -122.4194), LatLng(34.0522, -118.2437)],
);
```

---

## ⚙️ Troubleshooting
- Ensure the device has internet access.
- Confirm that the API key is active and properly configured.
- On Android, check for missing permissions using the console logs.
- On iOS, ensure the device has location services enabled.

---

# Current Location in Google Map

---

## 🛠️ Step 1: Install the Package
Add the following to your `pubspec.yaml`:

```yaml
dependencies:
  flutter:
    sdk: flutter
  google_maps_flutter: ^2.5.0
  geolocator: ^11.0.0
```
---

## 📦 Step 3: Import the Packages
```dart
import 'package:flutter/material.dart';
import 'package:google_maps_flutter/google_maps_flutter.dart';
import 'package:geolocator/geolocator.dart';
```

---

## 🗺️ Step 4: Display Current Location on Google Maps
Here is how you can display the user's current location with a marker:

```dart
class MapScreen extends StatefulWidget {
  @override
  _MapScreenState createState() => _MapScreenState();
}

class _MapScreenState extends State<MapScreen> {
  GoogleMapController? mapController;
  LatLng? currentPosition;

  @override
  void initState() {
    super.initState();
    _getCurrentLocation();
  }

  void _onMapCreated(GoogleMapController controller) {
    mapController = controller;
  }

  Future<void> _getCurrentLocation() async {
    bool serviceEnabled;
    LocationPermission permission;

    serviceEnabled = await Geolocator.isLocationServiceEnabled();
    if (!serviceEnabled) {
      return;
    }

    permission = await Geolocator.checkPermission();
    if (permission == LocationPermission.denied) {
      permission = await Geolocator.requestPermission();
      if (permission == LocationPermission.denied) {
        return;
      }
    }

    Position position = await Geolocator.getCurrentPosition(desiredAccuracy: LocationAccuracy.high);
    setState(() {
      currentPosition = LatLng(position.latitude, position.longitude);
    });

    mapController?.animateCamera(
      CameraUpdate.newCameraPosition(
        CameraPosition(target: currentPosition!, zoom: 14.0),
      ),
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('Google Maps Example')),
      body: currentPosition == null
          ? Center(child: CircularProgressIndicator())
          : GoogleMap(
              onMapCreated: _onMapCreated,
              initialCameraPosition: CameraPosition(
                target: currentPosition!,
                zoom: 14.0,
              ),
              markers: {
                Marker(
                  markerId: MarkerId('currentLocation'),
                  position: currentPosition!,
                  infoWindow: InfoWindow(title: 'You are here'),
                ),
              },
            ),
    );
  }
}
```

---

## ⚙️ Troubleshooting
- Ensure the device has internet access.
- Confirm that the API key is active and properly configured.
- On Android, check for missing permissions using the console logs.
- On iOS, ensure the device has location services enabled.
- Confirm that location permissions are granted in device settings.

---

## 🎉 Conclusion
You have successfully integrated Google Maps into your Flutter app using `google_maps_flutter` and displayed the user's current location. Experiment with adding more markers, routes, and custom map styling!

Happy coding! 😊

