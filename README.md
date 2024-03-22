# weather-reporting-system-using-IoT
#include <LiquidCrystal.h>

LiquidCrystal lcd(12, 11, 5, 4, 3, 2);

int wLED = 9;
int lightLevel = 0;
int LDR = A0;
int TMP = A1;
int rLED = 6;
int bLED = 7;
int gLED = 8;
int sensorPin = A1;

void setup() {
  Serial.begin(9600);
  lcd.begin (16, 2);
  pinMode(wLED, OUTPUT);
  pinMode(rLED, OUTPUT);
  pinMode(bLED, OUTPUT);
  pinMode(gLED, OUTPUT);
}

void loop() {
  
  int reading = analogRead(sensorPin);
  
  // converting that reading to voltage, for 3.3v arduino use 3.3
  float voltage = reading * 5.0;
  voltage /= 1024.0; 
 
  // print out the voltage
  Serial.print(voltage); Serial.println(" volts");
 
  // now print out the temperature
  float temperatureC = (voltage - 0.5) * 100 ;  //converting from 10 mv per degree wit 500 mV offset
                                               //to degrees ((voltage - 500mV) times 100)
  Serial.print(temperatureC); Serial.println(" degrees C");
 
  // now convert to Fahrenheit
  //float temperatureF = (temperatureC * 9.0 / 5.0) + 32.0;
  //Serial.print(temperatureF); Serial.println(" degrees F");
 
  lightLevel = analogRead(LDR);  
  Serial.println(lightLevel); 
  
  if (lightLevel < 10) {
    digitalWrite(wLED, HIGH);
  } else {
    digitalWrite(wLED, LOW);
  }
  
    lcd.clear();
  lcd.setCursor(0, 0);
  
  if (temperatureC < 2) {
    digitalWrite(bLED, HIGH);
    digitalWrite(rLED, LOW);
    lcd.print("Cold Weather");
    lcd.setCursor(0, 1);
    lcd.print(temperatureC);
    tone(10, 260);
  } else if (temperatureC >= 2 && temperatureC < 45){
    digitalWrite(bLED, LOW);
    digitalWrite(rLED, LOW);
    lcd.setCursor(0, 0);
    lcd.print("Normal Weather");
    noTone(10);
  } else {
    digitalWrite(rLED, HIGH);
    digitalWrite(bLED, LOW);
    lcd.setCursor(0, 0);
    lcd.print("Hot Weather");
    lcd.setCursor(0, 1);
    lcd.print(temperatureC);
    tone(10, 260);
  }
  
  delay (500);
  
}
