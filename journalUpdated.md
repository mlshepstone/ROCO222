# Roco222 Lab Book
## 25/09/17
### Practical 1: Software engineering for roboticists
1. Write your first markdown document

Here are the the basic syntax rules of markdown:
* Hash is used to denote a heading, with additional hashes being used for further sub-headings
```markdown
# Heading
``` 
* Bold is represented using either double asterisk or underscore either side of what you want bold
```markdown
**Bold**
```
* Italics is represented using either an asterisk or an underscore either side of what you want to have in italic
```markdown
_Italics_
```
* Paragraphs are separated by a blank line
```markdown

Paragraph
```
* Two spaces at the end of a line leaves a line break
```markdown
Line break  
```
* Three dashes create a horizontal rule
```markdown
--- 
```
* Asterisks are used to create a bullet list
```markdown
* Bullet point
```
* Numbers and a period creates a numbered list
```markdown
1. Numbered list
```
* Putting text in square brackets creates a link, with the url of the link in brackets afterwords
```markdown
[http://example.com](http://example.com)
```

2. Command-Line 101

* ls - list directory contents
* cd /tmp - changes directory to a tempory directory
* cd $HOME - changes directory to the home directory, the $ represents the user account 
* mkdir - used to make directories
* echo "Hello" > hello.md - outputs the text "Hello" and saves it to a .md file
* cat hello.md - outputs the text in the .md file
* cp hello.md hello-again.md - copies the contents of hello.md into a new file
* mv hello-again.md hello-hello.md - renames file
* rm hello.md - removes the file 
* rm -rf - force deletes everything (doesn't work in practice)
* cat /proc/cpuinfo - displays the specs of the cpu

4. Your first git repository

ls -al - displays all data on the files and folders in the selected directory, it shows things such as the number of bits and the creation date and time of each file and folder.

6. Version tracking

So far i've learnt that git uses repositories to store data. These repositories allow tracking of the various versions of added data through the use of commit messages. Repositories can be stored locally or online.

## 02/10/17
### Practical 2: Build a DC motor

1. Build a commutator

The commutator is used to power the motor coils. We had no issues with construction at this time.

2. Add a support shaft

The inserted nails support the motor and allow it to rotate. We had no issues with construction at this time. 

3. Wind the armature coil

The coil cuts the magnetic field which causes motion when a current is flowing. We had a small amount trouble whilst winding (tangling etc.), but nothing too major. We ended up with 120 turns in our coil.

4. Build the shaft support and magnet brackets
5. Build the baseplate

These components are use to hold the commutator and coil whilst it spins. We had some issues when attaching the supports to the baseplate, but we managed to solve this problem by using extra washers.

## 09/10/17

1. Finish the motor
2. Test the motor

Initially we had some issues as we hadn't correctly turned on the power supply, once this was corrected the motor turned successfully. From our tests we found a few issues with the design that we should be able to fix with future iterations. These issues are as follows:
* Crumpled commutator - Through the building process the commutator became damaged, this caused some issues whilst the motor was in motion.
* Single wire brushes - We used stripped single core wire as the motor brushes, and used our hands to support them. These two factors caused a very unreliable connection.
* Loose motor supports - The paper clip motor supports allowed for quite a lot a shifting when the motor was turning.

3. A better DC motor

We began the design for our next DC motor at the end of the session, we will use what we learnt from our previous attempt to ensure a more reliable system. 

## 16/10/17

This is the DC motor we built: 

![Better motor](https://github.com/Schenkington/Images/blob/master/Better%20motor.JPG)

Motor specs:
* Number of coils: 2
* Turns per coil: 60
* Coil resistance (per coil): ~2 ohms

The motor is made from 2 coils which cross each other each are soldered to with each end of a coil solder onto opozing comutators which are ,ade from quartered copper compression olives. The core of the armature is made from some washers that we glued together, with cork halves on either end Under the idea that havign a mettal core would creat a hreater flux under less turns makingthe motor more efficient. The motor brushes are made from half spheres of solder that are sprung to allow the brushes remain in contact with the comutator at all times. 

However we were not able ot get our motor to functio properly, as we tested that current stillran through the coils as we rotated it we narrowed down the problem to 2 factors: 
1. The magnetic field may be too weak as 2 of the neodymium magnets broke so the motor only has 2 in it.
2. There is too much friction generated where the motor is supported and betwen the comutators and brushesfor it to turn using its own power

## 23/10/17
### Practical 3: Incremental Encoder
1. Circuit diagram
2. Build the IR LED light source
3. Build the light detector
The circuit we built looks like this:
![Encoder](https://github.com/Schenkington/Images/blob/master/Encoder.JPG)

We constructed the ciruit  by soldering the components onto stripboard, Using the circuit diagram from in step 1. Jumper cables were used for the +5V, GND and Sense Voltage connections.we used 2 500 ohm resistors to substitute the 1k resistor as wewereanable to get oen at the time.
The circuit was tested by first looking by using a mobile phone camera to check wether or not the IR Emmiter was reciving power and functioning correctly, you were able to tell as through the camera you can see a dim purple glow from the ir emmiter. The phototransistor was tested by connecting the sense voltage cable to an oscilloscope and noting how the voltage cahnged when the diode was covered and when it was not. 

4. Place an encoder disc on the motor
Our motor with disc: 
![Encoder disk](https://github.com/Schenkington/Images/blob/master/Encoder%20disc.JPG)

The disc was attached to the motor using a small screw instaed of bluetac blue tack as this will keep it more secure. 
When spun on the motor the disc successfully blocks all the light except when the gap spins to the appropriate position.

## 30/10/17

5. Calculate the angular velocity of your motor (using arduino)

you needed to edit a previous piece of code so that it would count the number of light pulses in a second.

```cpp
const byte ledPin = 13;
const byte interruptPin = 2;
volatile byte state = LOW;
int count = 0;

void setup() {
  Serial.begin(9600);
  pinMode(ledPin, OUTPUT);
  pinMode(interruptPin, INPUT);
  attachInterrupt(digitalPinToInterrupt(interruptPin), countfunc, RISING);
}

void loop() {
  delay(1000)
  pulsecount();
}

void countfunc() {
  count += 1;
}

void pulsecount(){
  Serial.println("Pulse count:");
  Serial.println(count); 
  count = 0; 
  delay(1000);
}
```

In the new code the interrupt increments a vairiable each time it occurs. Every second the program outputs thevalue of the vairiable to the console and then resets the variable. 
Using this program we wereable to estimat estimate the motor we used to have a angular velocity of ~20 rads/s

## 06/11/17
### Practical 4: Controlling the motor
1. Install the motor shield
2. Control a small hobby DC motor

The audino motor shield was installed onto the audino by carefully alighning the pins on the sheild with the coreresponding portson the audino. The following code was used to run the motor.

```cpp
/*************************************************************
Motor Shield 1-Channel DC Motor
by Randy Sarafan
For more information see:
https://www.instructables.com/id/Arduino-Motor-Shield-Tutorial/
*************************************************************/
void setup() {
  //Setup Channel A
  pinMode(12, OUTPUT); //Initiates Motor Channel A pin
  pinMode(9, OUTPUT); //Initiates Brake Channel A pin
}
void loop(){
  //forward @ full speed
  digitalWrite(12, HIGH); //Establishes forward direction of Channel A
  digitalWrite(9, LOW);
  //Disengage the Brake for Channel A
  analogWrite(3, 255);
  //Spins the motor on Channel A at full speed
  delay(3000);
  digitalWrite(9, HIGH); //Engage the Brake for Channel A
  delay(1000);
  //backward @ half speed
  digitalWrite(12, LOW); //Establishes backward direction of Channel A
  digitalWrite(9, LOW);
  //Disengage the Brake for Channel A
  analogWrite(3, 123);
  //Spins the motor on Channel A at half speed
  delay(3000);
  digitalWrite(9, HIGH); //Eengage the Brake for Channel A
  delay(1000);
}
```
To change the motors direction you would change the value of pin 12, where a HIGH would turn it in one direction which was refered to as forward, and a LOW would turn it in the opiste direction wich we refered to as backward. To change the speed of the motor you write a value of 0-255 to pin 3 with 255 being at full speed and 0 being off (motionless).

3. Get your DC motor to rotate 
4. Close the loop

This is the program that adjusts the speed of the motor based on the values from the encoder.

```cpp 
const byte ledPin = 13;
const byte interruptPin = 2;
volatile byte state = LOW;
int count = 0;
int val = 255;
int RPM = 150;
int pulses;

void setup() {
  pinMode(12, OUTPUT);
  pinMode(9, OUTPUT);
  pinMode(ledPin, OUTPUT);
  pinMode(interruptPin, INPUT);
  attachInterrupt(digitalPinToInterrupt(interruptPin), countfunc, RISING);
}

void loop() {
  digitalWrite(12, HIGH);
  digitalWrite(9, LOW);
  analogWrite(3, val);
  delay(1000);
  pulsecount();
}

void countfunc() {
  count += 1;
}

void pulsecount(){
  pulses = RPM/60;
  if (pulses > count){
    val += 1;
  }
  else if (pulses < count){
    val -= 1;
  }
  else if (val >= 255){
    val = 255;
  }
  count = 0; 
}
```

The program works capares counted number of pulse to a set number of pulses per second that you have decided and the does one of the following. If more pulses than required are counted the value that controls speed is decremented by one, if lesse pulses are counted it is incremented by one. An example RPM of 150 is used in this program.

## 13/11/17
### Practical 5: Stepper motors


