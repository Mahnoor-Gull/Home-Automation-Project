# Home-Automation-Project  

---

```md
# 🏡 Home Automation System 🌟  

### **Automate Your Home with Arduino!**
A **smart home automation system** that enhances security and convenience.  

---

## 📌 Overview  
This **Arduino-powered Home Automation System** integrates:  
✔️ **Servo motor** to automate doors  
✔️ **PIR sensor** for motion detection  
✔️ **Temperature sensor** to control fans  
✔️ **LED indicator** for security  

---

## 🎯 Features  
✅ **Hands-free door automation** using an ultrasonic sensor  
✅ **LED activation** upon motion detection  
✅ **Fan control** based on temperature  
✅ **Compact and power-efficient design**  

---

## 🛠️ Components Required  
| Component       | Quantity |
|----------------|----------|
| **Arduino Uno** | 1        |
| **HC-SR04 Ultrasonic Sensor** | 1 |
| **SG90 Servo Motor** | 1 |
| **PIR Sensor** | 1 |
| **LED** | 1 |
| **Temperature Sensor (LM35)** | 1 |
| **Jumper Wires** | - |
| **Power Source (5V)** | 1 |

---

## ⚙️ Circuit Diagram  
📌 *Connect the components as follows:*  

- **Servo Motor** → Pin **8**  
- **Ultrasonic Sensor**:  
  - Trig → Pin **7**  
  - Echo → Pin **7** (shared)  
- **PIR Sensor** → Pin **2**  
- **LED Indicator** → Pin **4**  
- **Fan Control** → Pins **12, 13**  
- **Temperature Sensor (LM35)** → Analog Pin **A0**  
- **VCC & GND** → **Power Supply**

---
```cpp

#include <Servo.h>

// Pin Definitions
const int trigPin = 7;   // Ultrasonic Sensor Trigger Pin
const int echoPin = 7;   // Ultrasonic Sensor Echo Pin (same as trigPin)
const int servoPin = 8;  // Servo Motor Pin
const int pirPin = 2;    // PIR Sensor Pin
const int ledPin = 4;    // LED Pin (PIR indicator)
const int fanPin = 12;   // Fan Control Pin
const int fanPin2 = 13;  // Second Fan Control Pin
const int tempPin = A0;  // Temperature Sensor Pin

Servo doorServo; // Create Servo object

void setup() {
    Serial.begin(9600); // Initialize Serial Monitor
    doorServo.attach(servoPin);
    
    // Set pin modes
    pinMode(pirPin, INPUT);
    pinMode(ledPin, OUTPUT);
    pinMode(fanPin, OUTPUT);
    pinMode(fanPin2, OUTPUT);
    pinMode(tempPin, INPUT);

    digitalWrite(ledPin, LOW);
    digitalWrite(fanPin, LOW);
    digitalWrite(fanPin2, LOW);
}

void loop() {
    float distance = getDistance();

    Serial.print("Distance: ");
    Serial.print(distance);
    Serial.println(" cm");

    // Open door if distance is less than 40 cm
    if (distance > 0 && distance < 40) {
        doorServo.write(90); // Open
        delay(2000);
    } else {
        doorServo.write(0);  // Close
    }

    // PIR Sensor - Motion Detection
    if (digitalRead(pirPin) == HIGH) {
        digitalWrite(ledPin, HIGH); // Turn on LED
        delay(1000);
    } else {
        digitalWrite(ledPin, LOW);
    }

    // Temperature Sensor - Fan Control
    float temperature = analogRead(tempPin) * 0.48; // Convert to Celsius
    Serial.print("Temperature: ");
    Serial.print(temperature);
    Serial.println(" °C");

    if (temperature > 20) {
        digitalWrite(fanPin, HIGH);
        digitalWrite(fanPin2, LOW);
    } else {
        digitalWrite(fanPin, LOW);
        digitalWrite(fanPin2, LOW);
    }

    delay(500); // Small delay to prevent excessive sensor reading
}

// Function to get distance from ultrasonic sensor
float getDistance() {
    pinMode(trigPin, OUTPUT);
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);
    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);
    digitalWrite(trigPin, LOW);

    pinMode(echoPin, INPUT);
    long duration = pulseIn(echoPin, HIGH);
    return (duration * 0.0343) / 2;  // Convert time to cm
}
  

---

## 🎬 How It Works  
🟢 **Step 1:** The ultrasonic sensor detects an object within **40 cm**.  
🟢 **Step 2:** The servo motor moves **90°** to open the door.  
🟢 **Step 3:** After **2 seconds**, the door automatically closes.  
🟢 **Step 4:** The **PIR sensor** detects motion and turns on an **LED indicator**.  
🟢 **Step 5:** If the temperature is **above 20°C**, the **fan turns ON** for cooling.  

---

## 📷 Demo  
![Image](https://github.com/user-attachments/assets/464069e6-f119-464f-8a7f-0f5f6b04f3dc)
---

## 💡 Future Improvements  
🔹 Add **Wi-Fi connectivity** for IoT control  
🔹 Integrate a **mobile app** for remote access  
🔹 Implement **voice control** with Google Assistant  

---


```

---

