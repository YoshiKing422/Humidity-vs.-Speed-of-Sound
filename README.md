# Measuring the Effect of Humidity on the Speed of Sound

## Project Overview

This project investigates how changing the humidity of air affects the measured speed of sound. An Arduino Uno R3 collects data from an HC-SR04 ultrasonic sensor and a DHT11 humidity sensor. The ultrasonic sensor measures the time required for a sound pulse to travel to a fixed target and back, while the DHT11 measures the humidity inside the chamber. During each trial, the Arduino records ten measurements consisting of the humidity and calculated speed of sound. The experiment is repeated three times at different humidity levels to determine whether there is a relationship between humidity and the speed of sound.

---

# Research Question

How does increasing the humidity inside an enclosed chamber affect the measured speed of sound?

---

# Hypothesis

Within a predefined distance, if the humidity increases. Then the speed of sound will decrease, because the additional water vapor will slow the transmission of the sound waves.

If the humidity inside the chamber increases while the distance between the ultrasonic sensor and the target remains constant, then the speed of sound will decrease and the ultrasonic sensor will measure a longer echo time, because we predict that additional water vapor in the air slows the transmission of sound waves.

---

# Materials

### Electronics

- Arduino Uno R3
- HC-SR04 Ultrasonic Sensor
- DHT11 Humidity Sensor
- Breadboard
- Jumper wires
- USB cable

### Chamber Materials

- Clear plastic storage container with a tight-fitting lid
- Flat piece of cardboard, acrylic, or wood to use as the target
- Damp Scrub Daddy, Scrub Mommy, or sponge
- Small plastic cup or dish for the sponge
- Tape or hot glue
- Ruler or measuring tape

---

# Building the Chamber

Use a clear plastic storage container with a tight-fitting lid. Mount the HC-SR04 ultrasonic sensor securely on one side of the container so it points directly toward a flat target mounted on the opposite side. Measure the distance carefully and set it to exactly 150mm.

Place the DHT11 near the center of the chamber so it measures the overall humidity of the air instead of the moisture immediately surrounding the sponge.

Place a damp Scrub Daddy, Scrub Mommy, or sponge inside a small plastic cup in one corner of the container. The sponge should be damp but not dripping and should never touch the electronics. Close the lid and allow the humidity to stabilize before collecting data.

---

# Wiring

### HC-SR04 Ultrasonic Sensor

| Sensor Pin | Arduino Uno |
| --- | --- |
| VCC | 5V |
| GND | GND |
| TRIG | D9 |
| ECHO | D10 |

### DHT11 Humidity Sensor

| Sensor Pin | Arduino Uno |
| --- | --- |
| VCC | 5V |
| GND | GND |
| DATA | D2 |

---

# Variables

### Independent Variable

- Humidity inside the chamber (% Relative Humidity)

### Dependent Variables

- Calculated speed of sound (m/s)

### Controlled Variables

- Distance between the ultrasonic sensor and target (50.0 cm)
- Same ultrasonic sensor
- Same Arduino Uno
- Same DHT11 sensor
- Same target
- Same chamber
- Same power supply
- Same sensor positions
- Temperature kept as constant as possible

---

# Experimental Procedure

1. Assemble the circuit according to the wiring table.
2. Secure the HC-SR04 ultrasonic sensor so it cannot move during testing.
3. Measure and set the distance from the ultrasonic sensor to the target at exactly **50.0 cm**.
4. Place the DHT11 near the center of the chamber.
5. Place the damp sponge inside a small plastic cup away from all electronics.
6. Upload the Arduino program.
7. With the chamber dry, run the program to complete **Round 1**, which records **10 measurements** of humidity and speed of sound.
8. Place the damp sponge inside the chamber and close the lid.
9. Wait approximately 10 minutes for the humidity to stabilize.
10. Run the program again to complete **Round 2**, which records another 10 measurements.
11. Allow the humidity to increase further by waiting several more minutes or lightly rewetting the sponge if necessary.
12. Run the program one final time to complete **Round 3**, recording the last 10 measurements.
13. Compare the speed of sound measured at each humidity level to determine whether a correlation exists.

---

# Arduino Code

Before uploading the code, install the **DHT Sensor Library by Adafruit** using the Arduino Library Manager.

```cpp
#include <DHT.h>

#define DHTPIN 2
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);

// HC-SR04 pins
const int trigPin = 9;
const int echoPin = 10;

// Distance from sensor to target (meters)
const float distance = 0.50;

// Number of measurements in one trial
const int NUM_MEASUREMENTS = 10;

void setup() {

  Serial.begin(9600);

  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);

  dht.begin();

  Serial.println("Speed of Sound vs. Humidity");
  Serial.println();
}

void loop() {

  for (int measurement = 1; measurement <= NUM_MEASUREMENTS; measurement++) {

    // Read humidity
    float humidity = dht.readHumidity();

    // Send ultrasonic pulse
    digitalWrite(trigPin, LOW);
    delayMicroseconds(2);

    digitalWrite(trigPin, HIGH);
    delayMicroseconds(10);

    digitalWrite(trigPin, LOW);

    // Measure echo time
    long duration = pulseIn(echoPin, HIGH);

    // Calculate speed of sound
    float speed = (2.0 * distance) / (duration / 1000000.0);

    // Print results
    Serial.print("Measurement ");
    Serial.println(measurement);

    Serial.print("Humidity: ");
    Serial.print(humidity);
    Serial.println(" %");

    Serial.print("Speed of Sound: ");
    Serial.print(speed, 2);
    Serial.println(" m/s");

    Serial.println();

    delay(1000);
  }

  Serial.println("Trial Complete.");

  while (true) {
    // Stop the program until the Arduino is reset.
  }
}
```

## Data — Room Humidity

| Measurement | Humidity (%) | Speed of Sound (m/s) |
| --- | --- | --- |
| 1 | 43.00 | 468.16 |
| 2 | 43.00 | 464.90 |
| 3 | 44.00 | 464.90 |
| 4 | 44.00 | 464.90 |
| 5 | 44.00 | 458.93 |
| 6 | 44.00 | 458.93 |
| 7 | 45.00 | 464.90 |
| 8 | 45.00 | 464.68 |
| 9 | 45.00 | 464.68 |
| 10 | 45.00 | 464.90 |

## Data — High Humidity

| Measurement | Humidity (%) | Speed of Sound (m/s) |
| --- | --- | --- |
| 1 | 82.00 | 525.49 |
| 2 | 82.00 | 521.38 |
| 3 | 85.00 | 521.38 |
| 4 | 85.00 | 521.38 |
| 5 | 85.00 | 521.38 |
| 6 | 85.00 | 519.75 |
| 7 | 86.00 | 521.38 |
| 8 | 86.00 | 521.38 |
| 9 | 86.00 | 521.10 |
| 10 | 86.00 | 521.10 |

## Data — Very High Humidity

| Measurement | Humidity (%) | Speed of Sound (m/s) |
| --- | --- | --- |
| 1 | 86.00 | 531.35 |
| 2 | 86.00 | 525.21 |
| 3 | 91.00 | 526.87 |
| 4 | 91.00 | 526.87 |
| 5 | 91.00 | 520.29 |
| 6 | 91.00 | 505.31 |
| 7 | 91.00 | 572.08 |
| 8 | 91.00 | 570.13 |
| 9 | 91.00 | 581.40 |
| 10 | 91.00 | 581.40 |
