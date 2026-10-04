## SOS_Alert_System
# Intelligent SOS & Location Alert System

An intelligent safety system using **ML-based danger detection, STM32, ESP32, GPS, Wi-Fi, and Telegram** to automatically send emergency alerts with the user's real-time location.

## 🔹 System Workflow

```text
Sensors / IMU
     ↓
ML Detection / Prediction
     ↓
STM32
     │
     │ UART: W / E / S
     ↓
ESP32
     ↓
GPS → Latitude + Longitude
     ↓
Wi-Fi
     ↓
Telegram Bot
     ↓
Emergency Contact
     ↓
Google Maps Location
```
# Hardware 
-STM32<br>
-ESP32<br>
-GPS Module<br>
-IMU / Sensors<br>
-MOSFET / Transistor for GPS power control<br>
-Battery / Power Supply<br>
# Software
-Arduino IDE
-ESP32 Arduino Core
-TinyGPSPlus
-UniversalTelegramBot
-ArduinoJson
-Telegram Bot API
# Telegram Setup
!.)Open Telegram.
2.)Search for @BotFather.
3.)Send /newbot.
4.)Create the bot.
5.)Copy the generated bot token.
6.)Open the new bot and press Start.
7.)Send the bot a message.
8.)Open:
```
https://api.telegram.org/botYOUR_TOKEN/getUpdates
```
9.)Find the ```chat.id``` value.
10.)Use it as the ```CHAT_ID```
Use placeholders in the source code:
```cpp
const char* WIFI_SSID = "YOUR_WIFI_NAME";
const char* WIFI_PASSWORD = "YOUR_WIFI_PASSWORD";

#define BOT_TOKEN "YOUR_BOT_TOKEN"
#define CHAT_ID "YOUR_CHAT_ID"
```
Replace with your own wifi and telegram chat token
# GPS Operation

The GPS is normally kept OFF to save power.

When STM32 sends W or E:

```
STM32
  ↓
ESP32 receives alert
  ↓
GPS Power ON
  ↓
GPS searches for satellite fix
  ↓
Latitude + Longitude
  ↓
Google Maps Link
  ↓
Telegram Alert
  ↓
GPS Power OFF

The GPS waits up to 90 seconds for a valid fix.
```
If a GPS fix cannot be obtained, the Telegram alert is still sent without the location.
# Google Maps Integration

Once GPS coordinates are obtained, the ESP32 automatically generates a Google Maps URL.
```
Latitude  : 12.823600
Longitude : 80.044400
```

the ESP32 generates:
```
https://maps.google.com/?q=12.823600,80.044400
```

The recipient can tap the link to open the location in Google Maps and can get exact location.
#  Telegram Alert Example
``` 
EMERGENCY ALERT

Danger detected.

GPS Location:
12.823600, 80.044400

Google Maps:
https://maps.google.com/?q=12.823600,80.044400
```
# ESP32 Responsibilities

The ESP32 acts as the communication and location-management unit.

Its responsibilities include:

-Receiving STM32 danger states.<br>
-Connecting to Wi-Fi.<br>
-Controlling GPS power.<br>
-Reading GPS data.<br>
-Waiting for a valid GPS fix.<br>
-Extracting latitude and longitude.<br>
-Creating a Google Maps link.<br>
-Sending Telegram alerts.<br>
-Turning GPS OFF after the alert.<br>
# Required Arduino Libraries

Install the following libraries using the Arduino IDE Library Manager.

**TinyGPSPlus**<br>
Used for parsing GPS NMEA data.<br>
**UniversalTelegramBot**
Used for Telegram communication.<br>
**ArduinoJson**
Required as a dependency for Telegram communication.
# WARNING State
When the STM32 sends:
```W```
the ESP32:
1.)Receives W.<br>
2.)anges state to WARNING.<br>
3.)Turns GPS ON.<br>
4.)Starts GPS UART.<br>
5.)Waits for a GPS fix.<br>
6.)Extracts coordinates.<br>
7.)Generates Google Maps link.<br>
8.)Sends Telegram warning.<br>
9.)Turns GPS OFF.<br>

# License
This project is developed for academic, research, and educational purposes.<br>
Before deploying the system for real-world emergency use, the hardware, software, communication reliability, GPS accuracy, power system, and failure-handling mechanisms should be thoroughly validated.

