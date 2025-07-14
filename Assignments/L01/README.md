## A01 - Basic GitHub Setup
### Rykir Evans
### Description:

This folder contains the documentation for the first lab activity in 4443-IoT. This introductory lab exhibits basic use and code examples for the Arduino as well as basic hardware implementation of LEDs and resistors. 

### Introduction Work
Getting familiar with the breadboard and hardware components is a central aspect of this course, but with no prior knowledge, I had to start somewhere. To do so, I wired a simple circuit from the arduino to the anode of the LED, and put a 220Ω resistor on the cathode side before wiring the circuit back to ground. This simple schematic is pictured below followed by pictures of the physical circuit

|                                Schematic                                   |
| :------------------------------------------------------------------------: |
|  <img src="https://images2.imgbox.com/c6/73/ewvrpgJ5_o.png">               |

<br>

|                   Physical                    |                      Images                  |
| :-------------------------------------------: | :------------------------------------------: |
|  <img src="./media/IMG_1013.png" width="400"> | <img src="./media/IMG_1014.png" width="400"> |


Below is the basic code snippet to provide power to the pin
```ino
void setup() 
{
  pinMode(5, OUTPUT);      // Set pin 5 to output voltage
  digitalWrite(5, HIGH);   // Output voltage through pin 5
}

void loop() 
{}
```
### Expanded Code (Blinking LED)
On the code side, it was a simple tweak to change this circuit to make the LED flash on and off every second. Simply moving the `digitalWrite(5, HIGH)` command to the loop function, using `delay(1000)` to pause the runtime for 1 second, and adding `digitalWrite(5, LOW)` to turn the LED off, and delaying again as shown below.

```ino
void setup() 
{
  pinMode(5, OUTPUT);
}

void loop() {
  digitalWrite(5, HIGH); // Turns LED on if in source config
  delay(1000);
  digitalWrite(5, LOW);  // Turns LED off
  delay(1000);
}
```

### More COMING SOON ***

### Files





