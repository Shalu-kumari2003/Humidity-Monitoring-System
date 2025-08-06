Project Title: Air Temperature and Humidity Monitoring System
1. Introduction
The Air Temperature and Humidity Monitoring System is designed to continuously monitor environmental conditions, specifically ambient temperature and relative humidity. These parameters are crucial for various applications including agriculture, industrial safety, weather forecasting, and home automation. The primary objective of this project is to design, build, and test a real-time monitoring system using affordable and accessible technology.
2. Objectives
•	To measure and display real-time temperature and humidity data.
•	To store and possibly transmit the data for further analysis.
•	To alert users if temperature or humidity exceed predefined thresholds.
•	To implement an easy-to-use system that can be deployed in both indoor and outdoor environments.
3. Components Used
•	Microcontroller: Arduino Uno 
•	Sensors: DHT11 (for temperature and humidity)
•	Display: 16x2 LCD and I2C (for local display of readings)
•	Buzzer: for alert
•	Power Supply: USB or battery power or DC power supply(5V)
•	Additional: Resistors, Relay,Breadboard, Jumper Wires, LED

4 Circuit Diagram
A simple connection between the DHT11 sensor’s data pin and a digital input on the Arduino UNO . The LCD is connected through I2C or directly to digital pins.
5. Software and Tools
•	Programming Language: C/C++ (Arduino IDE)
•	Libraries Used:
o	DHT sensor library
o	Adafruit Unified Sensor
o	LiquidCrystal_I2C (if using I2C LCD)
6. Working Principle
1.	The sensor collects real-time temperature and humidity data from the environment.
2.	The Arduino reads the data at regular intervals (e.g. every 2 seconds).
3.	The data is then displayed on the screen.
4.	If the measured values exceed the defined thresholds, the system can trigger an alert on Buzzer sound.
7. Results
•	The system was able to monitor and display the temperature and humidity accurately.
•	Real-time data transmission to Serial plot monitor was successfully tested.
•	Alerts were triggered when set thresholds were breached.
•	The device was stable for continuous use over several hours.
 8. Project code:
#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHT.h>

// Hardware Pins
#define DHTPIN 2
#define DHTTYPE DHT11
#define BUZZER_PIN 3
#define LED_PIN 4
#define RELAY_PIN 5
// Thresholds
const int DRY_AIR_THRESHOLD = 40;  // Below 40% = dry
const int HUMID_AIR_THRESHOLD = 70;          // Above 70% = humid
// Initialize components (TRY BOTH I2C ADDRESSES)
// LiquidCrystal_I2C lcd(0x27, 16, 2);  // Common address
LiquidCrystal_I2C lcd(0x3F, 16, 2);    // Alternate address
DHT dht(DHTPIN, DHTTYPE);
void setup() {
  Serial.begin(9600);
  
  // Initialize pins
  pinMode(LED_PIN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  pinMode(RELAY_PIN, OUTPUT);
  digitalWrite(RELAY_PIN, HIGH);                // Pump OFF
  
  // LCD Setup
  lcd.init();
  lcd.backlight();
  lcd.clear();
  // Startup message
  lcd.setCursor(0, 0);
  lcd.print("  PLANT MONITOR");
  lcd.setCursor(0, 1);
  lcd.print("  Starting...");
  dht.begin();
  delay(2000); // Allow sensors to stabilize
  lcd.clear();
}
void loop() {
  // Reading  sensors
  float humidity = dht.readHumidity();
  float temp = dht.readTemperature();
// Display humidity & temp
  lcd.setCursor(0, 0);
  lcd.print("H:");
  lcd.print(humidity, 1);
  lcd.print("% ");
   lcd.setCursor(8, 0);
  lcd.print("T:");
  lcd.print(temp, 1);
  lcd.write(223); // Degree symbol
  lcd.print("C");
                                                  // Status and alerts
  lcd.setCursor(0, 1);
  if (isnan(humidity) || isnan(temp)) {
    lcd.print("SENSOR ERROR!");
    digitalWrite(LED_PIN, HIGH);
    tone(BUZZER_PIN, 3000, 1000);
    return;
}
   if (humidity < DRY_AIR_THRESHOLD) {
    lcd.print("WATER NEEDED!   ");
    digitalWrite(LED_PIN, HIGH);
    tone(BUZZER_PIN, 1000, 500);
    digitalWrite(RELAY_PIN, LOW); // Buzzer ON
  } 
  else if (humidity > HUMID_AIR_THRESHOLD) {
    lcd.print("TOO HUMID!     ");
    digitalWrite(LED_PIN, HIGH);
    tone(BUZZER_PIN, 2000, 500);
    digitalWrite(RELAY_PIN, HIGH); // Buzzer OFF
  }
  else {
    lcd.print("NORMAL         ");
    digitalWrite(LED_PIN, LOW);
    noTone(BUZZER_PIN);
    digitalWrite(RELAY_PIN, HIGH);
  }

// Serial Monitor Output
  Serial.print("Humidity: ");
  Serial.print(humidity);
  Serial.print("% Temp: ");
  Serial.print(temp);
  Serial.print("°C | Pump: ");
  Serial.println(digitalRead(RELAY_PIN) ? "OFF" : "ON");
delay(3000);                // Update every 3 seconds
}


9. Applications
•	Smart home automation systems
•	Greenhouse environmental control
•	Weather stations
•	Industrial climate monitoring
•	Storage and warehousing
10. Conclusion
The Air Temperature and Humidity Monitoring System is a cost-effective and efficient tool for real-time environmental monitoring. With potential for expansion into IoT systems, this project can serve as a base for more complex climate monitoring applications.



