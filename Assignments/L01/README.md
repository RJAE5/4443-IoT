# L01 Introduction Lab
## Rykir Evans
### 🛠️ TLDR (AI Enhanced)
> Refer to [AI Disclaimer](./../README.md) for more information

This lab kicked off the course by exploring **basic Arduino programming** and **introductory circuit design**. Through a series of small experiments, I learned how to power LEDs, control them with **code**, and integrate **analog components** like potentiometers and RGB LEDs for dynamic control.

- 🔌 **Basic LED Circuit**  
  - Powered a single LED using pin 5 with a 220Ω resistor.  
  - First experiment in wiring and breadboarding.  
  - Learned basic `pinMode()` and `digitalWrite()` functions.  
  - Resulted in a simple "always on" LED.

- ⏱️ **Blinking LED (Digital Control)**  
  - Moved logic to `loop()` and added `delay()` calls to make the LED blink every second.  
  - Introduced core timing mechanics of the Arduino platform.

- 🎚️ **Potentiometer-Controlled Blink**  
  - Used a potentiometer (connected to analog pin A3) to dynamically control the LED blink speed.  
  - Introduced `analogRead()` and `map()` to convert analog values (0–1023) into a usable delay (100–1000 ms).  
  - Gave real-time analog input control to the circuit.  
  - ✅ [Demo Video](./media/potentiometer-close.mov)

- 🌈 **RGB LED Control**  
  - Identified pin mapping of RGB LED through trial and error.  
  - Used pins 9, 10, and 11 to power red, green, and blue individually.  
  - Created a cycling color effect using `digitalWrite()` and delay logic.  
  - ✅ [Demo Video](./media/rgb-led-close.mov)

- 🧪 **Bonus: RGB LED “Yellow Thief” Experiment**  
  - Inserting a yellow LED (or a resistor) caused an unexpected result:  
    - RGB LED began alternating red and green instead of showing yellow.  
  - Hypothesis: current redirection caused by altered load/resistance, affecting shared anode/cathode voltage dynamics.  
  - Confirmed with ChatGPT analysis (included in full description).  
  - ✅ [Demo Video](./media/rgb-yellow-thief.mov)

- 📷 **Media Samples**:  
  | Blink Setup | Potentiometer Setup | RGB LED Circuit |  
  | :---------: | :-----------------: | :-------------: |  
  | ![](./media/intro-birds-eye.png) | ![](./media/potentiometer-birds-eye.png) | ![](./media/rgb-led-birds-eye.png) |

> This lab introduced essential hardware and software skills for working with embedded systems. For full schematics, circuit explanations, and extra insights, see the **Full Description** below.

### Full Description:

This folder contains the documentation for the first lab activity in 4443-IoT. This introductory lab exhibits basic use and code examples for the Arduino as well as basic hardware implementation of LEDs and resistors. 

### Introduction Work
Getting familiar with the breadboard and hardware components is a central aspect of this course, but with no prior knowledge, I had to start somewhere. To do so, I wired a simple circuit from the arduino to the anode of the LED, and put a 220Ω resistor on the cathode side before wiring the circuit back to ground. This simple schematic is pictured below followed by pictures of the physical circuit

|                                Schematic                                   |
| :------------------------------------------------------------------------: |
|  <img src="https://images2.imgbox.com/c6/73/ewvrpgJ5_o.png" width="300">   |

<br>

|                   Bird's Eye                  |                     Close-up                 |
| :-------------------------------------------: | :------------------------------------------: |
|  <img src="./media/intro-birds-eye.png" width="400"> | <img src="./media/intro-close.png" width="400"> |


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
On the code side, it was a simple tweak to change this circuit to make the LED flash on and off every second. Simply moving the `digitalWrite(5, HIGH)` command to the loop function, using `delay(1000)` to pause the runtime for one second, and adding `digitalWrite(5, LOW)` to turn the LED off, and delaying again as shown below.

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

### Potentiometer Controlled Blink Rate
Implementing an analog element into the circuit was our next step. As part of this lab, we used a potentiometer to control the blink rate of an LED. The potentiometer has 3 pins, 2 for the positive and negative terminals of the potentiometer, and a 3rd to transmit the value of the potentiometer to the Arduino.

This required some minor rewiring of the circuit, but otherwise was a simple upgrade. The code saw the most change. The schematic is pictured below, and you can see the main change is the implementation of the potentiometer which is receiving 5V+ directly to the positive terminal (right-most, red wire). The negative terminal (left-most, black wire) is connected to the ground row on the breadboard. The center pin controls the analog output of the potentiometer, and it is subsequently connected to an analog pin on the Arduino (A3, yellow wire). Otherwise, the wiring for the LED remains exactly the same, just moved around slightly on the schematic.

|                                Schematic                                   |
| :------------------------------------------------------------------------: |
|  <img src="https://images2.imgbox.com/cf/14/BxjAU3PC_o.png" width="300">   |


<br> 

The code implementation showcases several new functions. The first, `analogRead()` which as the name suggests, reads in the input to the specified analog pin. However, the `map()` function has a lot more to unpack. The first of five values, in our case `potValue` is the initial input. The next two values are the desired range of this input, basically defining how fine-tuned the control will be. From `0` to `1023`, our potentiometer is controlled based on this range, with `0` being the minimum value, and `1023` being the maximum value. The remaining two values are what the variable `blinkDelay` will ultimately result in, depending on the input from the potentiometer. In our case, if the potentiometer is inputing `0`, `blinkDelay` will be mapped to 100ms. Conversely, an input of `1023` will map to 1 second. The map function is crucial for seamless conversion from input to output, and handles all rounding and approximation for the values inbetween the minimum and maximum.

The code is exhibited below.

<br>

```ino
void setup() 
{
  pinMode(5, OUTPUT);
}

void loop() 
{
  int potValue = analogRead(A3); // Read potentiometer
  int blinkDelay = map(potValue, 0, 1023, 100, 1000); // Map to 100ms–1s

  digitalWrite(5, HIGH);
  delay(blinkDelay);
  digitalWrite(5, LOW);
  delay(blinkDelay);
}
```
<br>

|                   Bird's Eye                  |                     Video Link               |
| :-------------------------------------------: | :------------------------------------------: |
|  <img src="./media/potentiometer-birds-eye.png" width="400"> | [Click here to watch video](./media/potentiometer-close.mov) |

<br>

### RGB LED Experiment
Having seen a standard LED as well as a potentiometer, the next easy step was to work with something that has a lot of pins but is still relatively simple. This introduced the RGB LED, which has 3 pins that will control the three RGB colors, as well as an anode pin. 

First, I had to power each pin seperately to distinguish what pins corresponded to what. My intuition lead me to believe that the longest pin was the anode, and that was indeed a correct assumption. Following that, I simply powered each pin independently in a small test to determine the pin order of the particular LED I had.

Since I had effectively accomplished the simplest task of powering the LED, my true start was to alternate between two colors. The provided code from the lab was an easy implementation, but I did not quite understand the circuitry before tying it to the schematic. Using the following schematic, I assembled a simple alternating RGB LED circuit, but since one color pin was always powered, the alternation was between a mix of color and a solid one.


|                                Schematic                                   |
| :------------------------------------------------------------------------: |
|  <img src="https://images2.imgbox.com/1a/4c/yWIdGLi8_o.png" width="300">   | 

With more understanding, I upgraded to fully utilize the code shown below, and wired each Arduino output pin to a corresponding color within the LED



```ino
int redPin = 9;
int greenPin = 10;
int bluePin = 11;

void setup() 
{
  pinMode(redPin, OUTPUT);
  pinMode(greenPin, OUTPUT);
  pinMode(bluePin, OUTPUT);
}

void loop() 
{
  digitalWrite(redPin, HIGH);
  digitalWrite(greenPin, LOW);
  digitalWrite(bluePin, LOW);
  delay(1000);

  digitalWrite(redPin, LOW);
  digitalWrite(greenPin, HIGH);
  delay(1000);

  digitalWrite(greenPin, LOW);
  digitalWrite(bluePin, HIGH);
  delay(1000);

  digitalWrite(bluePin, LOW);
}
```

|                   Bird's Eye                  |                     Video Link               |
| :-------------------------------------------: | :------------------------------------------: |
|  <img src="./media/rgb-led-birds-eye.png" width="400"> | [Click here to watch video](./media/rgb-led-close.mov) <br> (Original schematic with only 2 lights)|

### RGB LED Extra Experimentation
I added a second RGB LED powered the same way. This experiment was relatively mundane, but upon further experimentation with a yellow LED, something quite interesting happened. 

The RGB LEDs were alternating between yellow (a mix of red and green) as well as pure green. Upon inserting a yellow LED into the circuit, the RGB LEDs started alternating between red and green, appearing as if the yellow LED had "stolen" the yellow light from them. [Video here](./media/rgb-yellow-thief.mov)

This was perplexing, but upon inserting a simple resistor in place of the yellow LED, the same situation happened. This lead me to believe that the resistance caused by the yellow LED caused the current from the green pin to be reduced to minimal levels. This is definitely not a thorough explanation, and even with some knowledge of Ohm's Law, I could not explain it further.

Below is a simple ChatGPT prompt and response for the possible reason:

```
In a breadboard circuit, I had an RGB LED with the green and red pins powered in an alternating fashion. The red was permanently on, so when the green pin turned on, a yellow color was emitted. Upon inserting a yellow LED into the circuit after the RGB LED, the RGB LED seemed to alternate between pure red and pure green. I found this same situation when using a normal resistor instead of an LED. Can you give me a short reason as to why this is?
```

```
Yes — the behavior is due to a change in current path and load introduced by the second component (yellow LED or resistor).

🔧 What's happening:
When you added the yellow LED (or resistor) after the RGB LED, you altered the voltage distribution and current flow in the circuit. This change affects how the green and red elements of the RGB LED are driven.

🧠 Key idea:
RGB LEDs share a common cathode or anode, and their color intensity depends on how much current flows through each pin. By adding another component in series or parallel, you unintentionally shifted the voltage drop or current availability, which:

Disrupts the color blending (red + green = yellow),

And instead causes one color to dominate at a time (pure red or green alternating).

🟡 Why yellow turned into red/green flashing:
When the yellow LED or resistor was added, the circuit may no longer allow simultaneous current flow through both red and green pins.

Instead, only one gets enough current at a time, causing the RGB LED to flash between red and green.

This is a classic case of interaction between circuit components affecting LED behavior due to current sharing or voltage drop changes.
```

### Conclusion
Overall, this lab served its purpose to get someone with zero experience up and running with the most simple ideas in circuitry. I thoroughly enjoyed this lab and would take the knowledge throughout the rest of the term and hopefully beyond.

### Files

**All pictures and videos are contained within the `media` folder**
A detailed table of each file can also be found in the `media` folder [here](./media/README.md)






