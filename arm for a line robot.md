# arm


To add object detection and gripping functionality to your crane arm using an ultrasonic sensor, you'll need to integrate the sensor into your existing setup. Here’s a step-by-step guide to achieve this:

### Materials Needed:
1. **Arduino Board** (e.g., Arduino Uno)
2. **Servo Motor** (for the arm)
3. **Ultrasonic Sensor** (e.g., HC-SR04)
4. **Servo Motor for Gripper** (optional, if you want a separate servo for gripping)
5. **Jumper Wires**
6. **Breadboard** (optional)
7. **Power Supply** (for the servos)

### Steps:

#### 1. **Wiring the Ultrasonic Sensor:**
   - **Ultrasonic Sensor Pinout:**
     - **VCC** to 5V on Arduino
     - **GND** to GND on Arduino
     - **TRIG** to a digital pin on Arduino (e.g., pin 8)
     - **ECHO** to a digital pin on Arduino (e.g., pin 7)

   - **Wiring Example:**
     - TRIG to pin 8
     - ECHO to pin 7
     - VCC to 5V
     - GND to GND

#### 2. **Update Arduino Code:**
   Here’s an updated example code that integrates the ultrasonic sensor with the servo arm:

   ```cpp
   #include <Servo.h>

   // Create servo objects
   Servo armServo;
   Servo gripperServo;  // Optional, for gripping

   // Define pins for ultrasonic sensor
   const int trigPin = 8;
   const int echoPin = 7;

   // Define variables for ultrasonic sensor
   long duration;
   int distance;

   void setup() {
     armServo.attach(9);  // Attach the arm servo to pin 9
     gripperServo.attach(10);  // Optional: Attach the gripper servo to pin 10

     // Set up ultrasonic sensor
     pinMode(trigPin, OUTPUT);
     pinMode(echoPin, INPUT);
     
     Serial.begin(9600);
   }

   void loop() {
     // Measure distance using ultrasonic sensor
     digitalWrite(trigPin, LOW);
     delayMicroseconds(2);
     digitalWrite(trigPin, HIGH);
     delayMicroseconds(10);
     digitalWrite(trigPin, LOW);

     duration = pulseIn(echoPin, HIGH);
     distance = duration * 0.0344 / 2;  // Convert to cm

     // Print distance for debugging
     Serial.print("Distance: ");
     Serial.print(distance);
     Serial.println(" cm");

     // Check if an object is within a certain distance
     if (distance < 20) {  // Object detected within 20 cm
       // Move the arm to the gripping position
       armServo.write(90);  // Example angle, adjust as needed
       delay(1000);  // Wait for the arm to move

       // Optionally, activate the gripper to hold the object
       gripperServo.write(90);  // Example angle for gripping
       delay(1000);  // Wait to ensure grip is secure

       // Perform additional actions as needed

       // Return arm to the initial position
       armServo.write(0);
       delay(1000);
       
       // Release the object
       gripperServo.write(0);  // Example angle to release
       delay(1000);
     }

     delay(500);  // Delay between distance measurements
   }
   ```

#### 3. **Testing and Calibration:**
   - **Upload the Code:** Upload the code to your Arduino using the Arduino IDE.
   - **Test the Setup:** Place an object in front of the ultrasonic sensor to see if the arm responds correctly.
   - **Adjust Parameters:** You might need to adjust the distances, angles, and delays in the code depending on your specific setup and requirements.

#### 4. **Fine-Tuning:**
   - **Calibration:** Fine-tune the servo angles and distances based on the actual physical setup and the object sizes you are dealing with.
   - **Gripper Mechanism:** If you have a separate servo for gripping, ensure it’s properly calibrated to hold and release objects securely.

With this setup, your crane arm will be able to detect objects using the ultrasonic sensor and perform actions like gripping and moving based on the distance measurements. Let me know if you need further assistance!
