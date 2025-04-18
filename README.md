**Objective and Problem Statement
**Problem:
Conventional door locks are prone to physical tampering and lack remote monitoring or control capabilities. In today's digital world, there's a need for smarter, IoT-integrated security systems.
Objectives:
•
Create a secure door lock system with dual control via Keypad and Mobile App (Ubidots).
•
Use ESP32 for sending notifications and receiving lock/unlock commands via the internet.
•
Display real-time status on a 20x4 LCD.
•
Notify users of access status or changes via Ubidots dashboard or app.
Proposed Solution & Methodology
Tools and Materials:
•
Arduino Uno
•
ESP32
•
20x4 I2C LCD
•
4x4 Matrix Keypad
•
Servo Motor (Door Lock)
•
Relay Module
•
Ubidots Platform
2
•
Jumper Wires, Breadboard, and 5V Power Supply
Methodology:
1.
User inputs password via Keypad.
2.
If correct, Arduino triggers Servo Motor/Relay to unlock the door.
3.
All actions are mirrored on the LCD for feedback.
4.
ESP32 sends status updates to Ubidots and listens for lock/unlock commands.
5.
Remote access can control the door via the Ubidots dashboard.
