#define BLYNK_PRINT Serial
#include <ESP8266WiFi.h> //library for ESP8266 Module
#include <BlynkSimpleEsp8266.h>
#include <DHT.h> //Library for DHT sensor (temperature and humidity)
char auth[] = "T_HEtnODBL8ZuxP0OcGVBEIeDGXIN0Xn"; //Enter Auth code send by Blynk
char ssid[] = "OnePlus Nord CE 2"; //Enter your network name 
char pass[] = "pakku243@"; //Enter your WIFI Password
DHT dht(D3, DHT11); //(sensor pin, sensor type)
BlynkTimer timer;
void weather() {
 float h = dht.readHumidity(); //reading humidity value from DHT11 Sensor
 float t = dht.readTemperature();
 int r = analogRead(A0);
 bool l = digitalRead(D0);
 r = map(r, 0, 1023, 100, 0);
Blynk.virtualWrite(V0, t); //V0 is for Temperature
Blynk.virtualWrite(V1, h); //V1 is for Humidity
Blynk.virtualWrite(V2, r); //V2 is for Rainfall
 if (l == 0) {
 WidgetLED led1(V3);
 led1.on();
 } else if (l == 1) {
 WidgetLED led1(V3);
 led1.off();
 }
}
void setup() {
 Serial.begin(9600); // See the connection status in Serial Monitor
Blynk.begin(auth, ssid, pass, IPAddress(117,236,190,213),8080);
 dht.begin();
 // Setup a function to be called every second
 timer.setInterval(10L, weather);
}
void loop() {
 Blynk.run(); // Initiates Blynk
 timer.run(); // Initiates SimpleTimer
}


