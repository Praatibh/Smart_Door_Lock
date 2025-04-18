#include <Keypad.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

#define RELAY_PIN 10
#define SERVO_PIN 11

Servo myServo;
LiquidCrystal_I2C lcd(0x27, 20, 4);

const String password = "****";  // Your PIN
String input = "";

const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2, 3, 4, 5};
byte colPins[COLS] = {6, 7, 8, 9};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

void setup() {
  Serial.begin(9600);  // TX pin to ESP32 RX
  myServo.attach(SERVO_PIN);
  pinMode(RELAY_PIN, OUTPUT);

  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Enter PIN:");

  lockDoor(); // Start locked
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    lcd.setCursor(0, 1);

    if (key == '#') {
      if (input == password) {
        lcd.clear();
        lcd.print("Access Granted");
        unlockDoor();
        Serial.println("UNLOCK");  // Notify ESP32
        delay(3000);
      } else {
        lcd.clear();
        lcd.print("Access Denied");
        delay(2000);
      }
      input = "";
      lcd.clear();
      lcd.print("Enter PIN:");
    } else if (key == '*') {
      input = "";
      lcd.clear();
      lcd.print("Enter PIN:");
    } else {
      input += key;
      lcd.print("*");
    }
  }
}

void lockDoor() {
  myServo.write(0);
  digitalWrite(RELAY_PIN, LOW);
  Serial.println("LOCK"); // Notify ESP32
}

void unlockDoor() {
  myServo.write(90);
  digitalWrite(RELAY_PIN, HIGH);
}
