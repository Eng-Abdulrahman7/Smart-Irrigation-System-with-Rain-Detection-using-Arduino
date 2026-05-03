# Smart-Irrigation-System-with-Rain-Detection-using-Arduino
This project is a Smart Irrigation System built using Arduino. It automatically monitors soil moisture and rain conditions to determine whether plants need watering. The system helps optimize water usage by avoiding unnecessary irrigation, especially during rainfall.It also includes a manual mode that allows the user to control the system directly.

⸻

⚙️ 🔍 (How It Works)

The system uses a soil moisture sensor to measure the water level in the soil and a rain sensor to detect rainfall. Based on these inputs, the Arduino processes the data and decides whether to activate the water pump.

* If the soil is dry, the system turns on the pump.
* If the soil is moist, the system turns off the pump.
* If rain is detected, irrigation is stopped regardless of soil condition.

Additionally, a manual mode is implemented using a push button to allow direct control.

⸻

💡 📊 Features

* Automatic irrigation based on soil moisture
* Rain detection to prevent water waste
* Manual control mode using a button
* Visual indicators using LEDs
* Sound alert (buzzer) when soil is dry

⸻

🧰 🔌 Components Used

* Arduino Uno R3
* Soil Moisture Sensor Module
* Raindrop Sensor Module
* LEDs (Red, Green, Blue)
* Buzzer Module
* Push Button Switch
* Breadboard and Jumper Wires

 ⸻

  🔌 Wiring Summary

* Soil Sensor → A0
* Rain Sensor → A1
* Red LED → Pin 3
* Green LED → Pin 4
* Blue LED → Pin 5
* Pump (LED simulation) → Pin 8
* Buzzer → Pin 9
* Button → Pin 7

⸻

🌍 🌱 Real-Life Applications 

* Saves water by avoiding unnecessary irrigation
* Improves plant health by maintaining optimal soil moisture
* Reduces manual effort in plant care
* Can be used in home gardens, farms, and smart agriculture systems

⸻

🔊 Indicators Explanation

* Red LED: Soil is dry
* Green LED: Soil is in good condition
* Blue LED: Rain detected
* Pump LED: Watering is active
* Buzzer: Alerts when soil is dry

⸻

 Code Explanation 

The Arduino reads analog values from the soil moisture and rain sensors. It then uses conditional statements (if-else) to determine the system state. Based on these conditions, it controls the LEDs, buzzer, and pump. A button is used to toggle between automatic and manual modes.

⸻

🚀 Future Improvements

* Add a mobile app for remote control
* Connect to IoT platforms (WiFi module)
* Display data on an LCD screen
* Use a real water pump instead of LED

⸻


Code Emplimintation

int soilPin = A0;
int rainPin = A1;

int redLED = 3;
int greenLED = 4;
int blueLED = 5;
int pumpLED = 8;

int buzzer = 9;
int buttonPin = 7;

bool manualMode = false;

void setup() {
  pinMode(redLED, OUTPUT);
  pinMode(greenLED, OUTPUT);
  pinMode(blueLED, OUTPUT);
  pinMode(pumpLED, OUTPUT);
  pinMode(buzzer, OUTPUT);
  pinMode(buttonPin, INPUT_PULLUP);

  Serial.begin(9600);
}

void loop() {

  int soilValue = analogRead(soilPin);
  int rainValue = analogRead(rainPin);

  // 🔍 Print sensor values (for monitoring and calibration)
  Serial.print("Soil: ");
  Serial.print(soilValue);
  Serial.print(" | Rain: ");
  Serial.println(rainValue);

  // Toggle mode using button (Manual / Automatic)
  if (digitalRead(buttonPin) == LOW) {
    manualMode = !manualMode;
    delay(300); // simple debounce
  }

  // Reset all outputs (avoid conflicts)
  digitalWrite(redLED, LOW);
  digitalWrite(greenLED, LOW);
  digitalWrite(blueLED, LOW);
  digitalWrite(pumpLED, LOW);
  digitalWrite(buzzer, LOW);

  // 👆 Manual Mode
  if (manualMode) {
    digitalWrite(pumpLED, HIGH); // Turn ON pump only
  }

  // 🤖 Automatic Mode
  else {

    if (rainValue < 500) {
      // 🌧️ Rain detected → stop irrigation
      digitalWrite(blueLED, HIGH);
    }

    else if (soilValue > 600) {
      // 🌵 Soil is dry → turn ON pump and buzzer
      digitalWrite(redLED, HIGH);
      digitalWrite(pumpLED, HIGH);
      digitalWrite(buzzer, HIGH);
    }

    else {
      // 🌱 Soil is in good condition → no watering needed
      digitalWrite(greenLED, HIGH);
    }
  }

  delay(500); // small delay for stability
}
