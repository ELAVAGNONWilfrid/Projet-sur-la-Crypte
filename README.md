# Project-on-the-Crypt
This project involves using a DHT11 temperature sensor and passive buzzer with an Arduino Uno board to simulate an ancient artifact that responds to breath by playing a melody. The goal is to cool the sensor to trigger melodies.
# Context
You are in a dark and mysterious room. In the middle, there is a strange object: a small crystal connected to a box that can play music.
On the crystal it is written:
“Only the breath of the stars will reveal the melody.”
You understand that this crystal is like a magic thermometer (the TMP36 sensor). He measures the temperature around him. If the air is too hot, it remains silent. But if you blow on it to cool it, it activates and plays a melody thanks to the sound box (the buzzer).
Challenge: blow on the sensor to lower the temperature and trigger the music!

# Necessary equipment

## The DHT11 module
![image](https://github.com/user-attachments/assets/0ace4fdc-a18f-40ec-b0c5-116233a9fe8b)

### Features 
Power supply: 3V to 5.5V  
Consumption: 0.5 mA nominal / 2.5 mA maximum
Temperature measurement range: 0°C to 50°C ± 2°C
Humidity measurement range: 20-90%RH ±5%RH
Size:15mm x 12mm x 5.5mm
Weight: 3g

## Arduino Uno 
Arduino Uno is a microcontroller board based on the ATmega328P.

![image](https://github.com/user-attachments/assets/7ac1187d-d48a-40df-bce6-43f1f234e987)

• Microcontroller: ATmega328P (8 bits, 16 MHz).

• Memory (32 KB flash memory, 2 KB RAM, 1 KB EEPROM memory).

• Inputs/Outputs (14 digital pins, 6 analog inputs).

• Power supply: 5V (via USB or external 7-12V power supply).

• Communication: USB port for programming and serial communication.

## Breadboard
![image](https://github.com/user-attachments/assets/3ee9cc5d-983f-429f-b13d-c6453c4be193)

## Cables
![image](https://github.com/user-attachments/assets/bcd6168c-daad-4920-8855-6d3bf8b5d7dd)

## Circuit assembly
DHT11 Sensor Connections
- Vout pin of the DHT11 → Pin 2 on the Arduino (to read the temperature).
- Vcc pin of DHT11 → 5V on the Arduino.
- GND pin of the DHT11 → GND on the Arduino.

Passive buzzer connections
- Positive pin of the buzzer → Pin 8 on the Arduino (to control the melody).
- Buzzer negative pin → GND on the Arduino.
![image](https://github.com/user-attachments/assets/42a67c32-4c94-4970-974e-145ffae27e19)

## Arduino code
// Include the DHT library for temperature and humidity sensing

cpp

#include <DHT.h>

#define DHT_PIN 2       // Pin where the DHT sensor is connected

#define BUZZER_PIN 8

#define TEMP_SEUIL_20 20  // Temperature threshold in degrees Celsius (for magical melody)

#define TEMP_SEUIL_22 22  // Temperature threshold in degrees Celsius (for Frère Jacques melody)

// Define the verification delay (30 seconds)

#define DELAI_VERIFICATION 30000  // 30 seconds in milliseconds

/* Settings for the melody below 20°C */

// Frère Jacques melody - Notes (frequencies) and durations (in milliseconds)
int melody[] = {
  262, 294, 330, 262,            // Frère Jacques
  262, 294, 330, 262,            // Frère Jacques
  330, 349, 392,                 // Dormez-vous ?
  330, 349, 392,                 // Dormez-vous ?
  392, 440, 392, 349, 330, 262,  // Sonnez les matines !
  392, 440, 392, 349, 330, 262,  // Sonnez les matines !
  262, 196, 262,                 // Ding, dang, dong
  262, 196, 262                  // Ding, dang, dong
};

// Durations for each note in the melody
int noteDurations[] = {
  500, 500, 500, 500,
  500, 500, 500, 500,
  500, 500, 1000,
  500, 500, 1000,
  250, 250, 250, 250, 500, 500,
  250, 250, 250, 250, 500, 500,
  500, 500, 1000,
  500, 500, 1000
};

/* Settings for the melody below 20°C */

// Define musical notes (frequencies in Hz)

#define NOTE_C4 262

#define NOTE_D4 294

#define NOTE_E4 330

#define NOTE_F4 349

#define NOTE_G4 392

#define NOTE_A4 440

#define NOTE_AS4 466

#define NOTE_B4 494

#define NOTE_C5 523

#define NOTE_D5 587

#define NOTE_E5 659

#define NOTE_F5 698

#define NOTE_G5 784

#define NOTE_A5 880

#define NOTE_B5 988

#define NOTE_C6 1047

#define NOTE_D6 1175

#define NOTE_E6 1319

// Magical and fantastic melody - inspired by discovery/success music in video games
int magicalMelody[] = {
  // Part 1: Mysterious opening (arpeggio ascent)
  NOTE_C5, NOTE_E5, NOTE_G5, NOTE_C6,

  // Part 2: First triumphant motif
  NOTE_E6, NOTE_D6, NOTE_C6, NOTE_G5,
  NOTE_E6, NOTE_D6, NOTE_C6, NOTE_G5,

  // Part 3: Magical passage (series of chords)
  NOTE_A5, NOTE_C6, NOTE_A5, NOTE_C6,
  NOTE_G5, NOTE_B5, NOTE_G5, NOTE_B5,

  // Part 4: Culminating fanfare
  NOTE_C6, NOTE_G5, NOTE_E5, NOTE_G5,
  NOTE_C6, NOTE_E6, NOTE_D6, NOTE_E6,

  // Part 5: Magical finale (descending arpeggio followed by ascent)
  NOTE_C6, NOTE_A5, NOTE_F5, NOTE_D5,
  NOTE_C5, NOTE_D5, NOTE_F5, NOTE_A5,
  NOTE_C6, NOTE_E6
};

// Durations of the notes (1 = quarter note, 2 = eighth note, 4 = sixteenth note, etc.)
int magicalDurations[] = {
  // Part 1: Mysterious opening - longer-held notes
  3, 3, 3, 2,

  // Part 2: First triumphant motif - more marked rhythm
  2, 2, 2, 1,
  2, 2, 2, 1,

  // Part 3: Magical passage - swaying rhythm
  4, 4, 4, 4,
  4, 4, 4, 4,

  // Part 4: Culminating fanfare - triumphant march
  2, 4, 4, 2,
  2, 3, 3, 1,

  // Part 5: Magical finale - epic conclusion
  2, 2, 2, 2,
  4, 4, 4, 4,
  2, 1
};

/* Settings for the melody above 30°C */

// Define musical notes (frequencies in Hz)

#define NOTE_A3 220  // Low A

#define NOTE_B3 247  // B

#define NOTE_C4 262  // C

#define NOTE_D4 294  // D

#define NOTE_E4 330  // E

#define NOTE_F4 349  // F

#define NOTE_G4 392  // G

#define NOTE_A4 440  // A

#define NOTE_D5 587  // High D

#define NOTE_A5 880  // High A

// Notes for the alert melody (progression from low to high)
int Alertmelody[] = {
  // Phase 1 - Initial tension (water droplets)
  NOTE_A3, NOTE_D4, NOTE_A3, 0,
  NOTE_A3, NOTE_D4, NOTE_A3, 0,

  // Phase 2 - Increasing tension
  NOTE_A3, NOTE_D4, NOTE_A3, NOTE_E4,
  NOTE_A3, NOTE_D4, NOTE_A3, NOTE_E4,

  // Phase 3 - Urgency
  NOTE_C4, NOTE_F4, NOTE_C4, NOTE_G4,
  NOTE_C4, NOTE_F4, NOTE_G4, NOTE_A4,

  // Phase 4 - Imminent danger
  NOTE_D4, NOTE_A4, NOTE_D4, NOTE_A4,
  NOTE_D5, NOTE_A4, NOTE_D5, NOTE_A5
};

// Durations of the notes (1 = quarter note, 2 = eighth note, 4 = sixteenth note)
// Smaller values = faster notes
int alertnoteDurations[] = {
  // Phase 1 - Slower at the beginning
  4, 4, 4, 8,
  4, 4, 4, 8,

  // Phase 2 - Progressive acceleration
  4, 4, 4, 4,
  4, 4, 4, 4,

  // Phase 3 - Even faster
  3, 3, 3, 3,
  3, 3, 3, 3,

  // Phase 4 - Fast and shrill notes
  2, 2, 2, 2,
  2, 2, 2, 2
};

// Volumes (for PWM-compatible buzzers)
// Values increase to create a crescendo
int volumes[] = {
  // Phase 1 - Low volume
  100, 100, 100, 0,
  120, 120, 120, 0,

  // Phase 2 - Medium volume
  140, 140, 140, 140,
  160, 160, 160, 160,

  // Phase 3 - Louder volume
  180, 180, 180, 180,
  200, 200, 200, 200,

  // Phase 4 - Maximum volume
  220, 220, 230, 230,
  240, 240, 255, 255
};

void setup() {
  // Start serial communication at 9600 baud for debugging
  Serial.begin(9600);
  Serial.println("Initializing DHT sensor...");

  // Initialize the DHT sensor
  dht.begin();

  // Configure the buzzer pin as an output
  pinMode(BUZZER_PIN, OUTPUT);
}

// Function to play the Frère Jacques melody
void FrereJacques() {
  for (int i = 0; i < sizeof(melody) / sizeof(int); i++) {
    tone(BUZZER_PIN, melody[i], noteDurations[i]);
    delay(noteDurations[i] * 1.3);  // Small pause between notes
    noTone(BUZZER_PIN);             // Stop the note
  }
}

// Function to play the magical achievement melody
void playMagicalAchievementMelody() {
  int size = sizeof(magicalMelody) / sizeof(magicalMelody[0]);

  for (int note = 0; note < size; note++) {
    // Calculate the duration of the note
    int noteDuration = 1000 / magicalDurations[note];

    // Play the note
    tone(BUZZER_PIN, magicalMelody[note], noteDuration * 0.9);

    // Small pause between notes to distinguish them
    delay(noteDuration + 30);
    noTone(BUZZER_PIN);
  }
}

// Function to play the water alert melody
void playWaterAlertMelody() {
  int size = sizeof(Alertmelody) / sizeof(Alertmelody[0]);

  for (int note = 0; note < size; note++) {
    // Calculate the duration of the note (1 second divided by note type)
    int noteDuration = 1000 / alertnoteDurations[note];

    if (Alertmelody[note] == 0) {
      // If the note is 0, it's a silence
      delay(noteDuration);
    } else {
      // Play the note with the appropriate volume if PWM is available
      // If no PWM, use standard tone()
      tone(BUZZER_PIN, Alertmelody[note], noteDuration * 0.9);

      // A slight pause between notes to distinguish them
      delay(noteDuration);
      noTone(BUZZER_PIN);
    }

    // Short pause between notes
    delay(30);
  }
}

void loop() {
  unsigned long debutAttente = millis();  // Record the start time
  bool seuilAtteint = false;              // Variable to check if the threshold is reached

  // Check if the temperature is above 30°C
  /*
  float temperature = dht.readTemperature();
  if (temperature >= 30) {
    playWaterAlertMelody(); // Play the water alert melody
  } */

  // Wait for 30 seconds while checking the temperature
  while (millis() - debutAttente < DELAI_VERIFICATION) {
    // Read the temperature in Celsius
    float temperature = dht.readTemperature();
    
    if (temperature >= 30) {
    playWaterAlertMelody(); // Play the water alert melody
  }

    // Check if the reading is valid
    if (isnan(temperature)) {
      Serial.println("Error: Unable to read temperature!");
    } else {
      Serial.print("Temperature: ");
      Serial.print(temperature);
      Serial.println(" °C");

      // Check if the temperature is below the threshold
      if (temperature < TEMP_SEUIL_22) {
        FrereJacques();  // Play the Frère Jacques melody
        break;           // Exit the loop
      }

      if (temperature <= TEMP_SEUIL_20) {
        playMagicalAchievementMelody();  // Play the magical achievement melody
      }
    }
    delay(1000);  // Wait 1 second between each reading
  }

  // If the temperature did not drop below the threshold
  Serial.println("The temperature did not drop below the threshold.\n Crypt closed and melody lost");
  digitalWrite(ACTION_PIN, HIGH);  // Turn on the LED (or perform another action)
  delay(5000);                     // Keep the action active for 5 seconds
  digitalWrite(ACTION_PIN, LOW);   // Turn off the LED
  delay(1000);  // Wait 1 second before restarting
}
---
