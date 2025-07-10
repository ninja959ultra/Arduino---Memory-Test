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
byte buzzer = 6;

byte currentLength = 4;
byte count = 0;
byte round = 1;

bool checkDone = true;
bool player_winnig = true

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
        tone(buzzer, 400);
        delay(700);
        noTone(buzzer);
        digitalWrite(blueLED, LOW);
        break;

      case 2:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, HIGH);
        digitalWrite(redLED, LOW);
        digitalWrite(whiteLED, LOW);
        tone(buzzer, 500);
        delay(700);
        noTone(buzzer);
        digitalWrite(yellowLED, LOW);
        break;

      case 3:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, HIGH);
        digitalWrite(whiteLED, LOW);
        tone(buzzer, 600);
        delay(700);
        noTone(buzzer);
        digitalWrite(redLED, LOW);
        break;

      case 4:
        digitalWrite(blueLED, LOW);
        digitalWrite(yellowLED, LOW);
        digitalWrite(redLED, LOW);
        digitalWrite(whiteLED, HIGH);
        tone(buzzer, 700);
        delay(700);
        noTone(buzzer);
        digitalWrite(whiteLED, LOW);
        break;
    }
    

    delay(1000);
  }

  digitalWrite(blueLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(redLED, LOW);
  digitalWrite(whiteLED, LOW);
}


void winnigScreen() {
  digitalWrite(redLED, HIGH);
  digitalWrite(blueLED, HIGH);
  digitalWrite(yellowLED, HIGH);
  digitalWrite(whiteLED, HIGH);
  tone(buzzer, 700);
  delay(1500);
  digitalWrite(redLED, LOW);
  digitalWrite(blueLED, LOW);
  digitalWrite(yellowLED, LOW);
  digitalWrite(whiteLED, LOW);
  noTone(buzzer);
}


void setup() {
  randomSeed(analogRead(A3));
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);

  for (byte i=2; i<7; i++) {
    pinMode(i, OUTPUT);
  }

  pinMode(button, INPUT_PULLUP);

  while (digitalRead(button) == HIGH){ // Start button
    lcd.clear();
    lcd.print("press to Start");
    delay(10);  
  }

  for (byte z=0; z<4; z++) {
    correctMoves[z] = random(1,5);
  }

  lcd.clear();
}


void loop() {

  if (round < 7 && player_winnig == true) {
    showOrderDirections();

    while (count < currentLength){
      xRead = analogRead(pinX); 
      yRead = analogRead(pinY);

      if (yRead < 420) {
        moves[count] = 1; // blue (UP)
        tone(buzzer, 400);
        digitalWrite(blueLED, HIGH);
        delay(700);
        digitalWrite(blueLED, LOW);
        noTone(buzzer);
        count++;
      }

      if (yRead > 620 ) {
        moves[count] = 3; // red (DOWN)
        tone(buzzer, 600);
        digitalWrite(redLED, HIGH);
        delay(700);
        digitalWrite(redLED, LOW);
        noTone(buzzer);
        count++;
      }

      if (xRead > 620 ) {
        moves[count] = 2; // yellow (RIGHT)
        tone(buzzer, 500);
        digitalWrite(yellowLED, HIGH);
        delay(700);
        digitalWrite(yellowLED, LOW);
        noTone(buzzer);
        count++;
      }

      if (xRead < 420 ) {
        moves[count] = 4; // white (LEFT)
        tone(buzzer, 700);
        digitalWrite(whiteLED, HIGH);
        delay(700);
        digitalWrite(whiteLED, LOW);
        noTone(buzzer);
        count++;
      }
    } // End while loop


    for (byte z=0; z<currentLength; z++) {
      if (moves[z] != correctMoves[z]) {
        checkDone = false;
        break;
      }
    }

    if (checkDone){
      Serial.println("Correct!");
      delay(3000);
    }

    else{
      Serial.println("Wrong!");
      player_winnig = false;
      delay(3000);
    }



    // Serial.println(xRead);
    // Serial.println(yRead);
    Serial.println("End");
    delay(100);

  } // End Of rounds

  if (round == 7 && player_winnig == true) {
    winnigScreen();
  }

  else{}
}

```
