# SIT210 Task 4.1P – Handling Interrupts

## Overview

This project implements an interrupt-based lighting system using an Arduino Nano 33 IoT. The system uses a PIR motion sensor and a slider switch to control two LEDs. A BH1750 light sensor is also used to measure the surrounding light level.

The main purpose of this task is to demonstrate how hardware interrupts can be used to make an embedded system respond to external events without continuously polling the input devices.

## Hardware Used

* Arduino Nano 33 IoT
* PIR motion sensor
* BH1750 light sensor
* Slider switch
* 2 LEDs
* Resistors
* Breadboard
* Jumper wires

## Pin Configuration

| Component     | Arduino Pin |
| ------------- | ----------- |
| PIR Sensor    | D2          |
| Slider Switch | D3          |
| LED 1         | D5          |
| LED 2         | D6          |
| BH1750 SDA    | SDA         |
| BH1750 SCL    | SCL         |

## How the System Works

The system has two interrupt sources:

### PIR Motion Sensor

The PIR sensor is connected to digital pin 2 and uses a `RISING` interrupt. When motion is detected, the interrupt sets the `pirDetected` flag.

The main loop then checks the light level from the BH1750 sensor. If the measured light level is below the defined dark threshold, both LEDs are switched ON.

### Slider Switch

The slider switch is connected to digital pin 3 using `INPUT_PULLUP` and uses a `FALLING` interrupt. When the switch is activated, the `buttonPressed` flag is set.

The main loop then switches both LEDs ON as a backup method of controlling the lights.

## Light Detection

The BH1750 sensor measures the surrounding light level in lux. The program uses a dark threshold of:

```cpp
const int darkThreshold = 50;
```

If the measured light level is below 50 lux when motion is detected, the system considers the environment dark and turns both LEDs ON.

## Interrupts Used

The program uses Arduino's `attachInterrupt()` function:

```cpp
attachInterrupt(digitalPinToInterrupt(pirPin),
                pirInterrupt, RISING);

attachInterrupt(digitalPinToInterrupt(buttonPin),
                buttonInterrupt, FALLING);
```

The interrupt service routines only set Boolean flags:

```cpp
void pirInterrupt() {
  pirDetected = true;
}

void buttonInterrupt() {
  buttonPressed = true;
}
```

The actual LED control and sensor processing are handled inside the main `loop()` function.

## Serial Monitor

The Serial Monitor provides feedback about the system status and displays the measured light level.

Example messages include:

* `System Started`
* `Motion detected and it is DARK!`
* `LED1 and LED2 ON`
* `Motion detected but it is BRIGHT.`
* `LEDs remain OFF`
* `Switch activated!`
* `LED1 and LED2 ON`

## Software Requirements

* Arduino IDE
* BH1750 Arduino library
* Arduino Wire library

## Running the Project

1. Open `Task4.1Interrupts.ino` in Arduino IDE.
2. Connect the Arduino Nano 33 IoT.
3. Connect the hardware according to the circuit diagram.
4. Install the BH1750 library if required.
5. Select the Arduino Nano 33 IoT board and correct COM port.
6. Upload the program.
7. Open the Serial Monitor at **9600 baud**.
8. Trigger the PIR sensor and slider switch to test the interrupt responses.

## Author

**Shivam Singla**

**SIT210 – Embedded Systems Development**

**Task 4.1P – Handling Interrupts**
