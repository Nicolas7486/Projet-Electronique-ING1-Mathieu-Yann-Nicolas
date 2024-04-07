# Projet-Electronique-ING1-Mathieu-Yann-Nicolas

#include <Arduino.h>

// Define pins for LEDs and buttons
const int buttonPins[] = {2, 3, 4, 5};
const int ledPins[] = {6, 7, 8, 9};

void setup() {
  // Initialize LEDs and buttons
  for (int i = 0; i < 4; ++i) {
    pinMode(buttonPins[i], INPUT_PULLUP);
    pinMode(ledPins[i], OUTPUT);
  }
}

void loop() {
  // Generate a random sequence of button presses
  int sequence[10];
  for (int i = 0; i < 10; ++i) {
    sequence[i] = random(4);
  }
  
  // Display the sequence
  for (int i = 0; i < 10; ++i) {
    digitalWrite(ledPins[sequence[i]], HIGH);
    delay(500);
    digitalWrite(ledPins[sequence[i]], LOW);
    delay(250);
  }
  
  // Read player input
  for (int i = 0; i < 10; ++i) {
    while (digitalRead(buttonPins[0]) == HIGH &&
           digitalRead(buttonPins[1]) == HIGH &&
           digitalRead(buttonPins[2]) == HIGH &&
           digitalRead(buttonPins[3]) == HIGH) {
      // Wait for button press
    }
    
    int buttonPressed = -1;
    for (int j = 0; j < 4; ++j) {
      if (digitalRead(buttonPins[j]) == LOW) {
        buttonPressed = j;
        break;
      }
    }
    
    if (buttonPressed != sequence[i]) {
      // Wrong button pressed, game over
      gameOver();
      return;
    }
  }
}

void gameOver() {
  // Blink LEDs to indicate game over
  for (int i = 0; i < 5; ++i) {
    for (int j = 0; j < 4; ++j) {
      digitalWrite(ledPins[j], HIGH);
    }
    delay(500);
    for (int j = 0; j < 4; ++j) {
      digitalWrite(ledPins[j], LOW);
    }
    delay(500);
  }
}
Ce code est un exemple très rudimentaire et peut être amélioré pour ajouter plus de fonctionnalités et de convivialité. N'oubliez pas de consulter la documentation de l'ATtiny et d'Arduino pour des informations plus détaillées sur la programmation et le câblage. Amusez-vous bien à créer votre jeu !




MG
a la place des leds jai un écran oled tu pourrais faire les modis, et en quoi consiste le simon says ?
ChatGPT
Bien sûr ! Si vous utilisez un écran OLED à la place des LED, vous pouvez afficher des motifs ou des symboles pour représenter les séquences à reproduire dans le jeu "Simon Says". Voici une version modifiée du jeu qui utilise un écran OLED :

cpp
Copy code
#include <Arduino.h>
#include <Adafruit_SSD1306.h>
#include <Adafruit_GFX.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET -1
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

// Define pins for buttons
const int buttonPins[] = {2, 3, 4, 5};

void setup() {
  // Initialize display
  if(!display.begin(SSD1306_SWITCHCAPVCC, 0x3C)) {
    Serial.println(F("SSD1306 allocation failed"));
    for(;;);
  }

  display.display();
  delay(2000);  // Pause for 2 seconds
  
  // Clear the buffer
  display.clearDisplay();
  display.display();
  
  // Initialize buttons
  for (int i = 0; i < 4; ++i) {
    pinMode(buttonPins[i], INPUT_PULLUP);
  }
}

void loop() {
  // Generate a random sequence of button presses
  int sequence[10];
  for (int i = 0; i < 10; ++i) {
    sequence[i] = random(4);
  }
  
  // Display the sequence
  for (int i = 0; i < 10; ++i) {
    display.clearDisplay();
    display.setTextSize(2);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(10, 20);
    display.print("Step ");
    display.println(i+1);
    display.display();
    delay(1000);
    
    // Display symbol for each button press
    switch(sequence[i]) {
      case 0:
        displayButtonSymbol(0);
        break;
      case 1:
        displayButtonSymbol(1);
        break;
      case 2:
        displayButtonSymbol(2);
        break;
      case 3:
        displayButtonSymbol(3);
        break;
    }
    delay(1000);
    display.clearDisplay();
    display.display();
    delay(500);
  }
  
  // Read player input
  for (int i = 0; i < 10; ++i) {
    while (digitalRead(buttonPins[0]) == HIGH &&
           digitalRead(buttonPins[1]) == HIGH &&
           digitalRead(buttonPins[2]) == HIGH &&
           digitalRead(buttonPins[3]) == HIGH) {
      // Wait for button press
    }
    
    int buttonPressed = -1;
    for (int j = 0; j < 4; ++j) {
      if (digitalRead(buttonPins[j]) == LOW) {
        buttonPressed = j;
        break;
      }
    }
    
    if (buttonPressed != sequence[i]) {
      // Wrong button pressed, game over
      gameOver();
      return;
    }
  }
}

void displayButtonSymbol(int buttonIndex) {
  switch(buttonIndex) {
    case 0:
      // Display symbol for button 0
      display.setTextSize(2);
      display.setTextColor(SSD1306_WHITE);
      display.setCursor(40, 20);
      display.println("BTN 1");
      display.display();
      break;
    case 1:
      // Display symbol for button 1
      display.setTextSize(2);
      display.setTextColor(SSD1306_WHITE);
      display.setCursor(40, 20);
      display.println("BTN 2");
      display.display();
      break;
    case 2:
      // Display symbol for button 2
      display.setTextSize(2);
      display.setTextColor(SSD1306_WHITE);
      display.setCursor(40, 20);
      display.println("BTN 3");
      display.display();
      break;
    case 3:
      // Display symbol for button 3
      display.setTextSize(2);
      display.setTextColor(SSD1306_WHITE);
      display.setCursor(40, 20);
      display.println("BTN 4");
      display.display();
      break;
  }
}

void gameOver() {
  // Blink display to indicate game over
  for (int i = 0; i < 5; ++i) {
    display.clearDisplay();
    display.display();
    delay(500);
    display.setTextSize(2);
    display.setTextColor(SSD1306_WHITE);
    display.setCursor(10, 20);
    display.println("GAME OVER");
    display.display();
    delay(500);
  }
}
