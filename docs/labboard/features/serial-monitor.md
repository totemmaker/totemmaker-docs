# 4. Serial monitor

![Mini Lab LabBoard serial mode](/assets/images/mini-lab/labboard-serial-mode.png)

Serial monitor mode allows to interact with TotemDuino over Serial. It can be used for code debugging or as integral part of application (view and control). Multiple features are available:

- Print serial output on display.
- Control LabBoard LED.
- Read LabBoard key press.

## Start

`bAU 57600` will be displayed when mode is selected.

Using ++"SET\+"++ and ++"SET\-"++ select configured baud rate and press ++"Right SELECT"++.  
Primary displayed baud rate can be changed in [settings](/labboard/features/setup/#serial).  
_Note: This number should match the one used in `Serial.begin(baudrate)`. This mode can also work in parallel with Arduino IDE Serial Monitor._

LabBoard revision **v.2.1** and **v.2.2** will ask to connect jumper cables to required pins. You will see information on display:  
`d0-t][d` - connect pins `D0` -> `TXD`  
`d1-dIG2` - connect pins `D1` -> `DIG2`  

![Mini Lab LabBoard serial mode wire check](/assets/images/mini-lab/labboard-serial-mode-wire-check.png)  

After valid connection - baud rate selection will be displayed.

## Display

LabBoard is capable of displaying 9 numbers or letters ant the same moment. Serial data stream from TotemDuino is intercepted and displayed with each new line. To be registered - serial data must end with new line character (\n) or `Serial.println()`. If there are more than 9 symbols - text is aligned to the right (start of text is cut off).

## Control LED grid

LabBoard contains 9 separate LED below display. Their state can be changed by sending serial command to LabBoard:  
`LabBoard:led<id>:<state>` - **id**: LED number [0:8], **state**: turn LED off/on [0:1].

- **0** - 50V
- **1** - 5V
- **2** - 5.0V
- **3** - DAC1
- **4** - DAC2
- **5** - DAC3
- **6** - VIN
- **7** - VREG
- **8** - mAmp

### Example

Turn DAC1, VREG LED on and 50V, mAmp off:  

```arduino
Serial.println("LabBoard:led3:1"); // DAC1 on
Serial.println("LabBoard:led7:1"); // VREG on
Serial.println("LabBoard:led0:0"); // 50V off
Serial.println("LabBoard:led8:0"); // mAmp off
```

### Function

A helper function can be added to sketch in order to control LED more easily:

```arduino
void LabBoard_setLED(int number, int state) {
  char cmd[] = "LabBoard:led0:0";
  cmd[12] = number + '0';
  if (state) cmd[14] = '1';
  Serial.println(cmd);
}
```

To change state of the LED, simply call function:

```arduino
LabBoard_setLED(3, 1) // DAC1 on
LabBoard_setLED(7, 1) // VREG on
LabBoard_setLED(0, 0) // 50V on
LabBoard_setLED(8, 0) // mAmp on
```

## Control DIGx LED

Two additional LED are available in the left corner of LabBoard:  
`LabBoard:dig<id>:<state>` - **id**: LED number [1:2], **state**: turn LED off/on [0:1].

- **1** - DIG1
- **2** - DIG2

### Example

Turn DIG1 on and DIG2 off  

```arduino
Serial.println("LabBoard:dig1:1"); // DIG1 on
Serial.println("LabBoard:dig2:0"); // DIG2 off
```

### Function

A helper function can be added to sketch in order to control DIGx LED more easily:

```arduino
void LabBoard_setDIG(int number, int state) {
  char cmd[] = "LabBoard:dig1:0";
  if (number == 2) cmd[12] = '2';
  if (state) cmd[14] = '1';
  Serial.println(cmd);
}
```

To change state of the LED, simply call function:

```arduino
LabBoard_setDIG(1, 1) // DIG1 on
LabBoard_setDIG(2, 0) // DIG2 off
```

## Read key press

LabBoard contains 5 keys. They state is transmitted over serial and can be intercepted inside TotemDuino. This is represented in command:  
`LabBoard:<name>:<state>` - **name**: key name, **state**: released/pressed [0:1].

- **key+** -  ++"SET\+"++
- **key-** -  ++"SET\-"++
- **keyL** -  ++"Left SELECT"++
- **keyC** -  ++"Middle SELECT"++
- **keyR** -  ++"Right SELECT"++

### Example

Serial data received when ++"SET\+"++ was pressed and released:   
```
LabBoard:key+:1
LabBoard:key+:0
```

It can be intercepted using Arduino [Serial read](https://www.arduino.cc/reference/en/language/functions/communication/serial/readstringuntil/){target=_blank} functions.

### Function

A helper function can be added to sketch in order to read key state more easily:

```arduino
bool LabBoard_readKey(String &key, bool &pressed) {
  if (Serial.available() > 0) {
    String cmd = Serial.readStringUntil('\n');
    if (cmd.startsWith("LabBoard:")) {
      pressed = cmd.charAt(14) == '1';
      key = cmd.substring(9, 13);
      return true;
    }
  }
  return false;
}
```

Call function to read key state:  

```arduino
// Prepare variables which is used to store button state
String key;
bool pressed;
if (LabBoard_readKey(key, pressed)) {
  // Key state changed. "key" holds button name and "pressed" is the state of the button
  Serial.print("Key name: ");
  Serial.print(key);
  Serial.print(", state: ");
  Serial.println(pressed);
}
```

## Example

```arduino
// Set LabBoard LED state
void LabBoard_setLED(int number, int state) {
  char cmd[] = "LabBoard:led0:0";
  cmd[12] = number + '0';
  if (state) cmd[14] = '1';
  Serial.println(cmd);
}
// Set LabBoard DIGx LED state
void LabBoard_setDIG(int number, int state) {
  char cmd[] = "LabBoard:dig1:0";
  if (number == 2) cmd[12] = '2';
  if (state) cmd[14] = '1';
  Serial.println(cmd);
}
// Read LabBoard key action
bool LabBoard_readKey(String &key, bool &pressed) {
  if (Serial.available() > 0) {
    String cmd = Serial.readStringUntil('\n');
    if (cmd.startsWith("LabBoard:")) {
      pressed = cmd.charAt(14) == '1';
      key = cmd.substring(9, 13);
      return true;
    }
  }
  return false;
}

float count = 0;
int ledNumber = 0;
int ledState = 0;
uint32_t skipCount = 0;

void setup() {
  // put your setup code here, to run once:
  Serial.begin(57600);
  pinMode(13, OUTPUT);
}

void loop() {
  // Read LabBoard key press
  String key;
  bool pressed;
  if (LabBoard_readKey(key, pressed)) {
    // Turn ON TotemDuino LED if any LabBoard key was pressed
    digitalWrite(13, pressed ? HIGH : LOW);
    // Print key status
    Serial.print(key);
    Serial.print(" ");
    Serial.println(pressed ? 1 : 0);
    // Ignore printing count value for 1 second
    skipCount = millis() + 1000;
  }
  // Run animation trought all LabBoard LED grid
  LabBoard_setLED(ledNumber, 0);
  if (++ledNumber > 8) ledNumber = 0;
  LabBoard_setLED(ledNumber, 1);
  // Blink between LabBoard DIGx LED
  LabBoard_setDIG(1, ledState);
  LabBoard_setDIG(2, !ledState);
  ledState = !ledState;
  // Increment and print floating point value
  if (skipCount < millis()) {
    count += 0.133;
    Serial.println(count);
  }
  // Wait 100ms for next print
  delay(100);
}
```
