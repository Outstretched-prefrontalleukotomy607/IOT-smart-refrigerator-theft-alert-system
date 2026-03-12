# 🧊 IoT Based Smart Refrigerator Theft Alert System

An **IoT-based security monitoring system** that detects unauthorized access to a refrigerator using an **IR sensor and ESP8266 (NodeMCU)**.
When someone opens the refrigerator during restricted hours, the system triggers a **buzzer alert** and sends a **mobile notification using the Blynk IoT platform**.

---

# 📌 Project Overview

In shared environments such as **hostels, offices, or shared kitchens**, refrigerator doors may be opened by unauthorized users, leading to food theft or misuse.

This project introduces a **smart IoT monitoring solution** that detects refrigerator access and sends alerts to the user.

The system combines **sensor detection, microcontroller processing, and cloud-based notifications** to improve monitoring and security.

---

# ⚙️ Technologies Used

* ESP8266 NodeMCU
* Arduino IDE
* Blynk IoT Platform
* IR Sensor
* Buzzer
* NTP Time Synchronization

---

# 🧩 Components Required

| Component        | Quantity | Purpose               |
| ---------------- | -------- | --------------------- |
| ESP8266 NodeMCU  | 1        | Main microcontroller  |
| IR Sensor        | 1        | Detects door movement |
| Buzzer           | 1        | Alert sound           |
| Jumper Wires     | Few      | Connections           |
| Blynk Mobile App | 1        | Notification system   |
| WiFi Network     | 1        | Internet connectivity |

---

# 🔌 Circuit Connections

| ESP8266 Pin | Connected Component |
| ----------- | ------------------- |
| D2          | IR Sensor OUT       |
| 3.3V        | IR Sensor VCC       |
| GND         | IR Sensor GND       |
| D5          | Buzzer Positive     |
| GND         | Buzzer Negative     |

---

# 🧠 Working Principle

1. ESP8266 connects to the **WiFi network** and Blynk IoT platform.
2. An **IR sensor** is placed near the refrigerator handle.
3. When someone attempts to open the refrigerator, the sensor detects the object.
4. ESP8266 checks the **current time using NTP server**.
5. If the access occurs during **restricted hours**, the system:

   * Activates a **buzzer alert**
   * Sends a **notification via Blynk**
   * Updates the **status on the mobile dashboard**

---

# 📱 Blynk Mobile Application Setup

1. Install **Blynk IoT App**
2. Create a **template for ESP8266**
3. Add device to template
4. Create event called:

```
fridge_open
```

5. Enable **mobile notifications**
6. Add **Label widget**
7. Set **Datastream → Virtual Pin V0**

---

# 💻 Arduino Code

```cpp
#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h>
#include <BlynkSimpleEsp8266.h>
#include <NTPClient.h>
#include <WiFiUdp.h>

char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";

int sensor = D2;
int buzzer = D5;

WiFiUDP udp;
NTPClient timeClient(udp, "pool.ntp.org", 19800);

int startHour = 13;
int endHour = 14;

void setup()
{
Serial.begin(9600);
pinMode(sensor, INPUT);
pinMode(buzzer, OUTPUT);

Blynk.begin("YOUR_AUTH_TOKEN", ssid, pass);

timeClient.begin();
}

void loop()
{
Blynk.run();
timeClient.update();

int detect = digitalRead(sensor);
int hour = timeClient.getHours();

if(detect == LOW && hour >= startHour && hour <= endHour)
{
digitalWrite(buzzer, HIGH);

Blynk.logEvent("fridge_open","Someone is opening the fridge!");

Blynk.virtualWrite(V0,"Door Opened");

delay(3000);

digitalWrite(buzzer, LOW);
}
else
{
Blynk.virtualWrite(V0,"Door Closed");
}
}
```

---

# 📸 Project Demonstration

You can add the following in your repository:

* Project Setup Image
* Circuit Diagram
* Demonstration Video

Example:

```
Images/project_setup.jpg
Video/demo_link.txt
```

---

# 🚀 Applications

* Hostel refrigerator monitoring
* Office refrigerator security
* Shared kitchen monitoring
* Smart home access control

---

# ✅ Advantages

* Low-cost IoT solution
* Easy to implement
* Real-time mobile notifications
* Prevents unauthorized refrigerator access

---

# ⚠️ Limitations

* Requires WiFi connection
* Depends on power supply
* Sensor range must be calibrated properly

---

# 🔮 Future Scope

* Camera-based monitoring
* Face recognition system
* RFID or fingerprint authentication
* Refrigerator usage logging
* Temperature monitoring system

---

# 👩‍💻 Developed By

Group 12 – IoT Project

* Sameer
* Ragini
* Saumya
* Aditya

Academic Project – Internet of Things
