# Финален код

```arduino
#include <DHT.h>
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);
#define DHTPIN 7
#define DHTTYPE DHT22
//Piezo on pin 13
const int buzzer = 13;
//Water sensor on analog pin 2
const int waterSensor = A2;
DHT dht(DHTPIN, DHTTYPE);

void setup(){
  //Piezo as an output pin
  pinMode(buzzer, OUTPUT);
  //Water sensor as an input pin
  pinMode(waterSensor, INPUT);
  /Initialization of dht, lcd and serial
  dht.begin();
  lcd.begin();
  lcd.backlight();
  Serial.begin(9600);
}

void loop(){
  int waterLevel = analogRead(waterSensor);
  Serial.print("Water level: ");
  Serial.println(waterLevel);

  if(waterLevel <= 300) {
    noTone(buzzer);
  }

  else {
    tone(buzzer, 500);
  }
  
  float h = dht.readHumidity();
  // Read temperature as Celsius (the default)
  float t = dht.readTemperature();
  // Read temperature as Fahrenheit (isFahrenheit = true)
  float f = dht.readTemperature(true);

  // Compute heat index in Fahrenheit (the default)
  float hif = dht.computeHeatIndex(f, h);
  // Compute heat index in Celsius (isFahreheit = false)
  float hic = dht.computeHeatIndex(t, h, false);

  //print every measure on the Serial monitor for reasurrance
  Serial.print(F("Humidity: ")); 
  Serial.print(h);
  Serial.print(F("%  Temperature: "));
  Serial.print(t);
  Serial.print(F("°C "));
  Serial.print(f);
  Serial.print(F("°F  Heat index: "));
  Serial.print(hic);
  Serial.print(F("°C "));
  Serial.print(hif);
  Serial.println(F("°F"));
  
  //print the same measures on the lcd screen
  lcd.clear();
  lcd.print("Temperature:");
  lcd.print(t);
  //go to start of 2nd line
  lcd.setCursor (0,1);
  lcd.print("Humidity:");
  lcd.print(h);
  
  delay(5000);
}
```