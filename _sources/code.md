# Краен код

```arduino
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>
#include <Keypad.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);
//Piezo on pin 13
const int buzzer = 13;
//LED on pin 12
const int led = 12;
//four rows
const byte ROWS = 4;
//columns
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};

//connect to the row pinouts of the keypad
byte rowPins[ROWS] = {2, 3, 4, 5}; 
//connect to the column pinouts of the keypad
byte colPins[COLS] = {6,7,8,9}; 

Keypad keypad = Keypad( makeKeymap(keys), rowPins, colPins, ROWS, COLS );

void setup(){
  //Piezo as output pin
  pinMode(buzzer, OUTPUT);
  //LED as output pin
  pinMode(led, OUTPUT);
  //Initialization of serial
  Serial.begin(9600); 
}

void loop(){
  //LED is off from start
  digitalWrite(led, LOW);
  //Buzzer is muted from start
  noTone(buzzer); 
  delay(1000);

  //Read keypad state
  char key = keypad.getKey();

  if(key == '8') {
    //Set the alarm on
    tone(buzzer, 500);
    delay(3600);
  }

  if (key == '6') {
    noTone(buzzer);
    //LED light on indicating danger to people who are outside, not safe to turn on the alarm
    digitalWrite(led, HIGH);
    delay(5000);
  }
  
  if(key == '2') {
    noTone(buzzer);
    //Simulate sending a text message to 911
    Serial.println("We have an emergency hostage situation, send help"); 
  }

  if(key == '3') {
    noTone(buzzer);
    //Simulate potential suspect recognition
    Serial.println("Potential suspects are identified, check security cameras");
  }

delay(4000);
}
```
