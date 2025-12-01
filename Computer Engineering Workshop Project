#include <Keypad.h>
#include <Servo.h>

// Servo and buzzer setup
Servo myServo;
int servoPin = 10;
int buzzerPin = 11;

// Password setup
String password = "1234";
String inputPassword = "";

// Keypad setup
const byte ROWS = 4;
const byte COLS = 4;
char keys[ROWS][COLS] = {
  {'1','2','3','A'},
  {'4','5','6','B'},
  {'7','8','9','C'},
  {'*','0','#','D'}
};
byte rowPins[ROWS] = {2,3,4,5};
byte colPins[COLS] = {6,7,8,9};
Keypad keypad = Keypad(makeKeymap(keys), rowPins, colPins, ROWS, COLS);

// Timing variables
bool unlocked = false;
unsigned long unlockTime = 0;
unsigned long buzzerStart = 0;

void setup() {
  myServo.attach(servoPin);
  myServo.write(90);       // Locked
  pinMode(buzzerPin, OUTPUT);
  Serial.begin(9600);
  Serial.println("Enter password and press #");
}

void loop() {
  char key = keypad.getKey();
  unsigned long currentMillis = millis();

  // Keypad input
  if (key != NO_KEY) {
    Serial.println(key);

    if (key == '#') { // Submit
      if (inputPassword == password) {
        Serial.println("Access Granted");
        myServo.write(180); // Unlock
        unlocked = true;
        unlockTime = currentMillis;
      } else {
        Serial.println("Access Denied");
        digitalWrite(buzzerPin, HIGH);
        buzzerStart = currentMillis;
      }
      inputPassword = "";
    } else if (key == '*') { // Clear input
      inputPassword = "";
      Serial.println("Input cleared");
    } else {
      inputPassword += key; // Add digit
    }
  }

  // Turn off buzzer after 500ms
  if (digitalRead(buzzerPin) == HIGH && buzzerStart != 0) {
    if (currentMillis - buzzerStart >= 500) {
      digitalWrite(buzzerPin, LOW);
      buzzerStart = 0;
    }
  }

  // Lock servo back after 3 seconds
  if (unlocked && (currentMillis - unlockTime >= 4000)) {
    myServo.write(90); // Lock
    unlocked = false;
  } }
}
