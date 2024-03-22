# Weather-Reporting-System-using-IoT
Real time Temperature,Humidity and Rainsensing in atmosphere using Arduino Uno.
we able to moniter the real time Temperature,Humidity and Rain with this code and Using DHT and Rain Sensor.


#include “DHT.h”

#include <LiquidCrystal_I2C.h>

#define DHT11_PIN 2  // Input to the  Arduino from DHT sensor

#define POWER_PIN 3  // The Arduino pin that provides the power to the rain sensor

#define DO_PIN 4     // The Arduino’s pin connected to DO pin of the rain sensor

DHT dht11(DHT11_PIN, DHT11);

LiquidCrystal_I2C lcd(0x27, 16, 2);

void setup() {

  Serial.begin(9600);

  dht11.begin(); // initialize the sensor

  // initialize the Arduino’s pin as an input

  pinMode(POWER_PIN, OUTPUT);  // configure the power pin pin as an OUTPUT

  pinMode(DO_PIN, INPUT);

  lcd.init();

  lcd.backlight();

}



void loop() {

  // wait a few seconds between measurements.

  Delay(2000);



  // read humidity

  float humi  = dht11.readHumidity();

  // read temperature as Celsius

  float tempC = dht11.readTemperature();

  // read temperature as Fahrenheit

  float tempF = dht11.readTemperature(true);

  digitalWrite(POWER_PIN, HIGH);  // turn the rain sensor’s power  ON

  delay(10);                      // wait 10 milliseconds



  int rain_state = digitalRead(DO_PIN);



  digitalWrite(POWER_PIN, LOW);  // turn the rain sensor’s power OFF

  Serial.print(“DHT11# Humidity: “);

  Serial.print(humi);

  Serial.print(“%”);

  Serial.print(“  |  “); 

  Serial.print(“Temperature: “);

  Serial.print(tempC);

  Serial.print(“°C ~ “);

  Serial.print(tempF);

  Serial.println(“°F”);

  lcd.setCursor(0, 0);

  lcd.print(“Tem:”);

  lcd.print(tempC);

  lcd.print(“C|”);

  lcd.print(tempF);

  lcd.print(“F”);

  lcd.setCursor(0, 1);

  lcd.print(“hum : “);

  lcd.print(humi);

  lcd.print(“ % “);

  delay(3000);

  

 if (rain_state == HIGH){



    Serial.println(“The rain is NOT detected”);

    lcd.clear();

    lcd.setCursor(0, 0);

    lcd.print(“Rain Not Detected”);

 }else{

    Serial.println(“The rain is detected”);

    lcd.clear();

    lcd.setCursor(0, 0);

    lcd.print(“Rain Detected”);



    }



  delay(2000);  // pause for 1 sec to avoid reading sensors frequently to prolong the sensor lifetime

}
