#include <WiFi.h>
#include <HTTPClient.h>

// Replace with your Wi-Fi credentials
const char* ssid = "YOUR_WIFI_SSID";
const char* password = "YOUR_WIFI_PASSWORD";

// Ubidots details
String ubidotsToken = "BBUS-c24da1e83fbb35f79ecaad8b7fa7*****"; // API Token
String deviceLabel = "DOOR_LOCK";
String variableLabel = "lock_status";

const int arduinoRxPin = 13; // ESP32 pin connected to Arduino TX (pin 1)
HardwareSerial ArduinoSerial(1);  // Use Serial1 for ESP32

void setup() {
  Serial.begin(115200);
  ArduinoSerial.begin(9600, SERIAL_8N1, arduinoRxPin, -1); // Only RX used (from Arduino)

  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nWiFi connected.");
}

void loop() {
  if (ArduinoSerial.available()) {
    String status = ArduinoSerial.readStringUntil('\n');
    status.trim();  // Remove any whitespace or newline

    if (status == "ON" || status == "OFF") {
      int value = (status == "ON") ? 1 : 0;
      sendToUbidots(value);
      Serial.println("Status sent to Ubidots: " + status);
    }
  }
  delay(500);
}

void sendToUbidots(int value) {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;

    String url = "http://industrial.api.ubidots.com/api/v1.6/devices/" + deviceLabel;
    String payload = "{\"" + variableLabel + "\":" + String(value) + "}";

    http.begin(url);
    http.addHeader("Content-Type", "application/json");
    http.addHeader("X-Auth-Token", ubidotsToken);

    int httpResponseCode = http.POST(payload);

    if (httpResponseCode > 0) {
      Serial.println("Ubidots response code: " + String(httpResponseCode));
    } else {
      Serial.println("Error sending data to Ubidots: " + String(httpResponseCode));
    }

    http.end();
  } else {
    Serial.println("WiFi disconnected.");
  }
}
