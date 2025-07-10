# Arduino---Memory-Test


# Code Below:
```cpp
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);


byte correctMoves[10];
byte moves[10];


byte blueLED = 2;
byte yellowLED = 3;
byte redLED = 4;
byte whiteLED = 5;
byte button = 7;
byte currentLength = 4;

int pinX = A0;
int pinY = A1;

int xRead, yRead;


void showOrderDirections() {
  for (byte i=0; i<currentLength; i++) {
    byte move = correctMoves[i];

    switch (move) {
      case 1:
        digitalWrite(blueLED, HIGH);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, LOW);
        digitalWrite(whiteLED, LOW);
        break;

      case 2:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, HIGH);
        digitalWrite(redLED, LOW);
        digitalWrite(whiteLED, LOW);
        break;

      case 3:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, HIGH);
        digitalWrite(whiteLED, LOW);
        break;

      case 4:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, LOW);
        digitalWrite(whiteLED, HIGH);
        break;
    }

    delay(1000);
  }

  digitalWrite(blueLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(redLED, LOW);
  digitalWrite(whiteLED, LOW);
}



void setup() {
  randomSeed(analogRead(A3));
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);

  for (byte i=2; i<6; i++) {
    pinMode(i, OUTPUT);
  }

  pinMode(button, INPUT_PULLUP);

  while (digitalRead(button) == HIGH){ // Start button
    lcd.clear();
    lcd.print("press to Start");
    delay(10);  
  }

  for (byte z=0; z<4; z++) {
    correctMoves[z] = z+1;
  }

  lcd.clear();
}


void loop() {
  xRead = analogRead(pinX); 
  yRead = analogRead(pinY); 

  showOrderDirections();


  // Serial.println(xRead);
  // Serial.println(yRead);
  delay(3000);
}

```
