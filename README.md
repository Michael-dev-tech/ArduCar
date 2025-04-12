# Obstacle Avoiding Robot

This Arduino project controls a 4-wheel robot that autonomously navigates and avoids obstacles using an ultrasonic sensor mounted on a servo motor.

## Description

The robot moves forward until it detects an obstacle within a close range (15cm). When an obstacle is detected, the robot stops, reverses slightly, and then uses a servo motor to turn an ultrasonic sensor left and right to check for the clearest path. It then turns towards the direction with more space before continuing to move forward.

## Hardware Requirements

* Arduino board (e.g., Arduino UNO)
* Adafruit Motor Shield V1 (or compatible motor driver capable of controlling 4 DC motors)
* 4 DC Motors connected to the motor shield
* Ultrasonic Sensor (HC-SR04 or similar compatible with NewPing library)
* Servo Motor (SG90 or similar)
* Appropriate chassis and wheels for a 4-wheel robot
* Power source (batteries) for Arduino and motors
* Connecting wires

## Software Dependencies

This code requires the following Arduino libraries:

1.  **Adafruit Motor Shield Library (V1):** [`AFMotor.h`](https://github.com/adafruit/Adafruit-Motor-Shield-library) - For controlling the DC motors via the Adafruit Motor Shield.
2.  **NewPing Library:** [`NewPing.h`](https://bitbucket.org/teckel12/arduino-new-ping/src/master/) - For interfacing with the ultrasonic distance sensor.
3.  **Servo Library:** [`Servo.h`](https://www.arduino.cc/reference/en/libraries/servo/) - For controlling the servo motor (usually included with the Arduino IDE).

Ensure these libraries are installed in your Arduino IDE before compiling and uploading the code.

## Pinout / Connections

* **Ultrasonic Sensor:**
    * `TRIG_PIN` connected to Arduino Pin `A0`
    * `ECHO_PIN` connected to Arduino Pin `A1`
* **Servo Motor:**
    * Signal pin connected to Arduino Pin `10` (as defined by `myservo.attach(10);`)
* **DC Motors:**
    * Motor 1 connected to Motor Shield Port `M1`
    * Motor 2 connected to Motor Shield Port `M2`
    * Motor 3 connected to Motor Shield Port `M3`
    * Motor 4 connected to Motor Shield Port `M4`
* **Motor Shield:** Mounted on the Arduino board as per the shield's instructions.

## How it Works

1.  **Setup:** Initializes the servo, sets its initial position to face forward, and takes initial distance readings.
2.  **Loop:**
    * Continuously checks the distance using the ultrasonic sensor facing forward.
    * **Obstacle Detected (<= 15cm):**
        * Stops all motors (`moveStop()`).
        * Moves backward for a short duration (`moveBackward()`).
        * Stops again.
        * Turns the servo to look right (`lookRight()`), measures distance.
        * Turns the servo to look left (`lookLeft()`), measures distance.
        * Compares right and left distances.
        * Turns the robot towards the direction with the greater distance (`turnRight()` or `turnLeft()`).
        * Stops briefly after turning.
    * **Path Clear (> 15cm):**
        * Moves the robot forward (`moveForward()`). The speed gradually increases to `MAX_SPEED`.
    * Reads the forward distance again for the next loop iteration.

## How to Use

1.  Assemble the robot hardware according to the **Hardware Requirements** and **Pinout / Connections**.
2.  Install the required **Software Dependencies** (libraries) in your Arduino IDE.
3.  Open the code in the Arduino IDE.
4.  Adjust `MAX_SPEED` (default 100) if necessary to control the robot's speed.
5.  Compile and upload the sketch to your Arduino board.
6.  Power on the robot. It should start moving forward and avoid obstacles based on the logic described.
