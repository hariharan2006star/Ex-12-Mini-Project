# Ex-12 Mini Project
# PASSWORD-BASED DOOR LOCK SYSTEM USING ARDUINO
## AIM:

To design and simulate a password-based door lock system using an Arduino Uno, a 4×4 keypad, an I2C LCD, and a servo motor in Tinkercad.

## REQUIREMENTS:
Arduino Uno
4×4 matrix keypad
16×2 LCD with I2C interface
Micro servo motor
Connecting wires
Computer with internet access
Tinkercad Circuits
## PROCEDURE:
Open Tinkercad Circuits and create a new circuit.
Add an Arduino Uno, a 4×4 keypad, a 16×2 I2C LCD, and a servo motor.
Connect keypad rows R1, R2, R3, and R4 to Arduino pins D9, D8, D7, and D6, respectively.
Connect keypad columns C1, C2, C3, and C4 to D5, D4, D3, and D2, respectively.
Connect the servo signal terminal to D12, its VCC terminal to 5V, and its ground terminal to GND.
Connect LCD SDA to A4, SCL to A5, VCC to 5V, and GND to GND.
Open the code editor, select Text mode, and enter the program below.
Ensure that the LCD’s I2C address matches the address in the program.
Start the simulation and observe the message “Enter Password:” on the LCD.
Enter the password 1234 using the keypad and press #.
Observe that the LCD displays “Access Granted” and “Door Open”, while the servo rotates to 90°.
Verify that the servo returns to the locked position of 0° after five seconds.
Enter an incorrect password and press #. Observe the message “Wrong Password” and verify that the servo remains locked.
Press * to clear an entry and enter the password again.
Record the output and stop the simulation after verifying the operation.

Wiring correction: The earlier screenshot shows the keypad connected to D7–D0. Change those connections to D9–D2 as specified above to match this program.

## PROGRAM:

```
#include <Keypad.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>

const byte ROWS = 4;
const byte COLS = 4;

char keys[ROWS][COLS] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

byte rowPins[ROWS] = {9, 8, 7, 6};
byte colPins[COLS] = {5, 4, 3, 2};

Keypad keypad = Keypad(
  makeKeymap(keys), rowPins, colPins, ROWS, COLS
);

LiquidCrystal_I2C lcd(0x20, 16, 2);
Servo doorServo;

const int SERVO_PIN = 12;
const int LOCK_ANGLE = 0;
const int OPEN_ANGLE = 90;

const String correctPassword = "1234";
String enteredPassword = "";

void homeScreen() {
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Enter Password:");
  lcd.setCursor(0, 1);
  lcd.print("PIN: ");
}

void displayStars() {
  lcd.setCursor(0, 1);
  lcd.print("                ");
  lcd.setCursor(0, 1);
  lcd.print("PIN: ");

  for (unsigned int i = 0; i < enteredPassword.length(); i++) {
    lcd.print("*");
  }
}

void checkPassword() {
  if (enteredPassword == correctPassword) {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Access Granted");
    lcd.setCursor(0, 1);
    lcd.print("Door Open");

    doorServo.write(OPEN_ANGLE);
    delay(5000);

    doorServo.write(LOCK_ANGLE);
    lcd.clear();
    lcd.print("Door Locked");
    delay(1500);
  }
  else {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Wrong Password");
    lcd.setCursor(0, 1);
    lcd.print("Try Again");
    delay(2000);
  }

  enteredPassword = "";
  homeScreen();
}

void setup() {
  doorServo.attach(SERVO_PIN);
  doorServo.write(LOCK_ANGLE);

  lcd.init();
  lcd.backlight();
  homeScreen();
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    if (key >= '0' && key <= '9') {
      if (enteredPassword.length() < 8) {
        enteredPassword += key;
        displayStars();
      }
    }
    else if (key == '*') {
      enteredPassword = "";
      homeScreen();
    }
    else if (key == '#') {
      checkPassword();
    }
    else if (key == 'A') {
      doorServo.write(LOCK_ANGLE);
      enteredPassword = "";

      lcd.clear();
      lcd.print("Door Locked");
      delay(1000);
      homeScreen();
    }
  }
}
```
## OUTPUT

<img width="1448" height="1086" alt="image" src="https://github.com/user-attachments/assets/ed04c903-7120-4dd5-8e0b-89ebea407fd5" />

## RESULT:


Thus, the password-based door lock system using Arduino Uno was successfully simulated in Tinkercad. Password entry through the keypad, status display on the LCD, and locking and unlocking operation using the servo motor were verified.
