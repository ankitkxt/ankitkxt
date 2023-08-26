import 'package:flutter/material.dart';
import 'package:geolocator/geolocator.dart';
import 'package:flutter_sms/flutter_sms.dart';

void main() {
  runApp(WomenSafetyApp());
}

class WomenSafetyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: HomeScreen(),
    );
  }
}

class HomeScreen extends StatefulWidget {
  @override
  _HomeScreenState createState() => _HomeScreenState();
}

class _HomeScreenState extends State<HomeScreen> {
  final Geolocator _geolocator = Geolocator();
  final String _emergencyMessage = "Help! I need assistance. My current location is: ";

  void _sendEmergencyMessage(String location) async {
    List<String> contacts = ["+1234567890", "+9876543210"]; // Add your emergency contact numbers here
    String message = _emergencyMessage + location;
    String result = await sendSMS(message: message, recipients: contacts);
    print("Emergency message sent: $result");
  }

  void _handleSendEmergency() async {
    Position position = await _getCurrentLocation();
    String location =
        "Latitude: ${position.latitude}, Longitude: ${position.longitude}";

    _sendEmergencyMessage(location);
  }

  Future<Position> _getCurrentLocation() async {
    try {
      return await _geolocator.getCurrentPosition(
          desiredAccuracy: LocationAccuracy.best);
    } catch (e) {
      print("Error getting location: $e");
      return null;
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Women Safety App"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: _handleSendEmergency,
              child: Text("Send Emergency Message"),
            ),
            SizedBox(height: 20),
            ElevatedButton(
              onPressed: () {
                Navigator.push(
                  context,
                  MaterialPageRoute(
                    builder: (context) => SafetyTipsScreen(),
                  ),
                );
              },
              child: Text("Safety Tips"),
            ),
          ],
        ),
      ),
    );
  }
}

class SafetyTipsScreen extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Safety Tips"),
      ),
      body: Padding(
        padding: EdgeInsets.all(16.0),
        child: ListView(
          children: [
            ListTile(
              leading: Icon(Icons.check),
              title: Text("Stay in well-lit areas at night."),
            ),
            ListTile(
              leading: Icon(Icons.check),
              title: Text("Avoid isolated and unsafe places."),
            ),
            ListTile(
              leading: Icon(Icons.check),
              title: Text("Be aware of your surroundings."),
            ),
            ListTile(
              leading: Icon(Icons.check),
              title: Text("Share your live location with trusted contacts."),
            ),
            // Add more safety tips as needed.
          ],
        ),
      ),
    );
  }
}
