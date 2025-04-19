# IC Testing Device

An academic project aimed at designing and building a **user-friendly IC Testing Device** capable of testing the functionality of **15+ standard logic and specialized ICs**, using **Arduino-based automation**. This project supports academic labs by offering a fast and accurate testing solution to improve hands-on learning and reduce manual errors.

---

## 📌 Project Objectives
- Develop a compact, reliable IC tester for educational and lab use.
- Interface **Arduino** with various digital and analog components.
- Display test results on an **LCD** for ease of interpretation.
- Test common logic ICs, flip-flops, counters, op-amps, and programmable ICs like **8255 PPI**.

---

## 🧠 Basic Working Principle
1. Power and logic signals are supplied to the IC Under Test (ICUT).
2. Inputs are driven using the Arduino and outputs are monitored.
3. Logic functionality is verified based on expected truth tables.
4. Output results are shown via the **LCD module**.

Schematic diagram(Proteus):  <img src="https://github.com/Shubham210204/IC-Testing-Device/blob/main/schematic.png">
<br>

<br>
PCB Layout(Proteus): <img src="https://github.com/Shubham210204/IC-Testing-Device/blob/main/layout.png">
<br>

<br>
Final PCB:  <img src="https://github.com/Shubham210204/IC-Testing-Device/blob/main/PCB.png">
<br>

---

## 🔧 Components Used
- **Arduino Unor**
- **16x2 LCD Display with I2C**
- **Copper-clad PCB and Solder Components**
- **Power Supply (5V Regulated)**
- **Custom PCB**
- **PC to communicate through serial monitor**

---

## 🧪 ICs Supported
**Combinational Logic ICs:**
- 7400 (NAND), 7402 (NOR), 7404 (NOT), 7408 (AND), 7432 (OR), 7486 (XOR)

**Sequential Logic ICs:**
- 7473 (JK Flip-Flop), 7474 (D Flip-Flop), 7490 (Decade Counter)

**Special Purpose ICs:**
- 741 (Op-Amp), 555 Timer, 4017 (Decade Counter),  
- 74138 (Decoder), 7447 (BCD to 7-Segment Driver),  
- 7483 (4-bit Adder/Subtractor), 8255 (PPI)

---

## ⚙️ Working Overview
- The IC is installed in the adequate socket.
- User needs to specify which IC has been installed, using serial monitor in arduino IDE.
- The Arduino sends the required input pattern to the IC.
- Expected outputs are compared against real-time results.
- The LCD displays whether the IC is **working** or **faulty**.
- Modular code allows easy extension to support more IC types.

<br>
Arduino code:

```bash
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

// Initialize LCD with I2C (change address if necessary)
LiquidCrystal_I2C lcd(0x27, 16, 2);

// 16 pin assignment
const int pin1 = 7, pin2 = 6, pin3 = 5, pin4 = 4;
const int pin5 = 2, pin6 = A1, pin7 = A0, pin8 = 13;
const int pin9 = A2, pin10 = 12, pin11 = 11, pin12 = 10;
const int pin13 = 9, pin14 = 8, pin15 = 3, pin16 = A3;

void setup() {
  Serial.begin(9600);
  lcd.begin(16, 2);
  lcd.backlight();

  // Initial display prompt
  lcd.setCursor(0, 0);
  lcd.print("IC Tester");
  lcd.setCursor(0, 1);
  lcd.print("Enter IC number:");
  Serial.println(F("Enter the IC number you want to test (1 to 16):"));
  Serial.println(F("Example: 1 for IC 7400, 2 for IC 7402, 3 for IC 7404, 4 for IC 7408, ..."));
  Serial.println(F(" 5 for IC 7432, 6 for IC 7486, 7 for IC 7483, 8 for IC 7490, ..."));
  Serial.println(F(" 9 for IC 7447, 10 for IC 7473, 11 for IC 8038, 12 for IC 74138, ..."));
  Serial.println(F(" 13 for IC 4017, 14 for IC 741, 15 for IC 555, 16 for IC 8255, ..."));
}

void loop() {
  // Check if user input is available
  if (Serial.available()) {
    int icNumber = Serial.parseInt();
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Testing IC #");
    lcd.print(icNumber);
    delay(3000);

    switch (icNumber) {
      case 1:
        testIC_7400();
        break;
      case 2:
        testIC_7402();
        break;
      case 3:
        testIC_7404();
        break;
      case 4:
        testIC_7408();
        break;
      case 5:
        testIC_7432();
        break;
      case 6:
        testIC_7486();
        break;
      case 7:
        testIC_7483();
        break;
      case 8:
        testIC_7490();
        break;
      case 9:
        testIC_7447();
        break;
      case 10:
        testIC_7473();
        break;
      case 11:
        testIC_7474();
        break;
      case 12:
        testIC_74138();
        break;
      case 13:
        testIC_4017();
        break;
      case 14:
        testIC_741();
        break;
      case 15:
        testIC_555();
        break;
      case 16:
        testIC_8255();
        break;
      default:
        lcd.clear();
        lcd.setCursor(0, 0);
        lcd.print("Device reset.");
        Serial.println(F("Device is reseting."));
        delay(2000);
        break;
    }

    // After test, prompt for next input
    Serial.println(F("Enter the next IC number to test (1 to 19):"));
    delay(1000); // Short delay before next input
  }
}

// IC-specific test functions
void testIC_7400() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7400...");
  Serial.println(F("Testing IC 7400..."));
  delay(3000);

  // Define pin assignments for the 7400 IC gates
  int input1_1 = pin1, input1_2 = pin2, output1 = pin3;
  int input2_1 = pin4, input2_2 = pin5, output2 = pin6;
  int input3_1 = pin11, input3_2 = pin12, output3 = pin10;
  int input4_1 = pin14, input4_2 = pin15, output4 = pin13;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1_1, OUTPUT);
  pinMode(input1_2, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2_1, OUTPUT);
  pinMode(input2_2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3_1, OUTPUT);
  pinMode(input3_2, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4_1, OUTPUT);
  pinMode(input4_2, OUTPUT);
  pinMode(output4, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;

  // Test cases for NAND gates (input combinations)
  int testCases[4][2] = {{LOW, LOW}, {LOW, HIGH}, {HIGH, LOW}, {HIGH, HIGH}};
  int expectedOutputs[4] = {HIGH, HIGH, HIGH, LOW};  // Expected NAND outputs for each case

  // Test each gate with all input combinations
  for (int i = 0; i < 4; i++) {
    // Gate 1 Test
    digitalWrite(input1_1, testCases[i][0]);
    digitalWrite(input1_2, testCases[i][1]);
    delay(20);  // Slightly increased delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2_1, testCases[i][0]);
    digitalWrite(input2_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3_1, testCases[i][0]);
    digitalWrite(input3_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4_1, testCases[i][0]);
    digitalWrite(input4_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P" : "F");
  lcd.setCursor(8, 0);
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P" : "F");
  lcd.setCursor(0, 1);
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P" : "F");
  lcd.setCursor(8, 1);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P" : "F");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7400 Test Done");
  Serial.println(F("IC 7400 test completed."));
}

void testIC_7402() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7402...");
  Serial.println(F("Testing IC 7402..."));
  delay(3000);

  // Define pin assignments for the 7402 IC gates
  int input1_1 = pin3, input1_2 = pin2, output1 = pin1;
  int input2_1 = pin6, input2_2 = pin5, output2 = pin4;
  int input3_1 = pin10, input3_2 = pin12, output3 = pin11;
  int input4_1 = pin13, input4_2 = pin15, output4 = pin14;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1_1, OUTPUT);
  pinMode(input1_2, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2_1, OUTPUT);
  pinMode(input2_2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3_1, OUTPUT);
  pinMode(input3_2, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4_1, OUTPUT);
  pinMode(input4_2, OUTPUT);
  pinMode(output4, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;

  // Test cases for NOR gates (input combinations)
  int testCases[4][2] = {{LOW, LOW}, {LOW, HIGH}, {HIGH, LOW}, {HIGH, HIGH}};
  int expectedOutputs[4] = {HIGH, LOW, LOW, LOW};  // Expected NOR outputs for each case

  // Test each gate with all input combinations
  for (int i = 0; i < 4; i++) {
    // Gate 1 Test
    digitalWrite(input1_1, testCases[i][0]);
    digitalWrite(input1_2, testCases[i][1]);
    delay(20);  // Slight delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2_1, testCases[i][0]);
    digitalWrite(input2_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3_1, testCases[i][0]);
    digitalWrite(input3_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4_1, testCases[i][0]);
    digitalWrite(input4_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P" : "F");
  lcd.setCursor(8, 0);
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P" : "F");
  lcd.setCursor(0, 1);
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P" : "F");
  lcd.setCursor(8, 1);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P" : "F");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7402 Test Done");
  Serial.println(F("IC 7402 test completed."));
}

void testIC_7404() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7404...");
  Serial.println(F("Testing IC 7404..."));
  delay(3000);

  // Define pin assignments for the 7404 IC gates
  int input1 = pin1, output1 = pin2;
  int input2 = pin3, output2 = pin4;
  int input3 = pin5, output3 = pin6;
  int input4 = pin11, output4 = pin10;
  int input5 = pin13, output5 = pin12;
  int input6 = pin15, output6 = pin14;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4, OUTPUT);
  pinMode(output4, INPUT);

  pinMode(input5, OUTPUT);
  pinMode(output5, INPUT);

  pinMode(input6, OUTPUT);
  pinMode(output6, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;
  bool gate5_pass = true;
  bool gate6_pass = true;

  // Test cases for NOT gates (input combinations)
  int testCases[2] = {LOW, HIGH};         // Input cases: LOW and HIGH
  int expectedOutputs[2] = {HIGH, LOW};    // Expected NOT gate outputs

  // Test each gate with both input conditions
  for (int i = 0; i < 2; i++) {
    // Gate 1 Test
    digitalWrite(input1, testCases[i]);
    delay(20);  // Small delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2, testCases[i]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3, testCases[i]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4, testCases[i]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }

    // Gate 5 Test
    digitalWrite(input5, testCases[i]);
    delay(20);
    if (digitalRead(output5) != expectedOutputs[i]) {
      gate5_pass = false;
    }

    // Gate 6 Test
    digitalWrite(input6, testCases[i]);
    delay(20);
    if (digitalRead(output6) != expectedOutputs[i]) {
      gate6_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Display the results for each gate in abbreviated format
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P " : "F ");
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P " : "F ");
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P " : "F ");

  lcd.setCursor(8, 0);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P " : "F ");
  lcd.print("G5: ");
  lcd.print(gate5_pass ? "P " : "F ");
  lcd.print("G6: ");
  lcd.print(gate6_pass ? "P " : "F ");

  delay(4000);  // Hold results for viewing

  // Final message
  lcd.clear();
  lcd.print("7404 Test Done");
  Serial.println(F("IC 7404 test completed."));
}

void testIC_7408() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7408...");
  Serial.println(F("Testing IC 7408..."));
  delay(3000);

  // Define pin assignments for the 7408 IC gates
  int input1_1 = pin1, input1_2 = pin2, output1 = pin3;
  int input2_1 = pin4, input2_2 = pin5, output2 = pin6;
  int input3_1 = pin11, input3_2 = pin12, output3 = pin10;
  int input4_1 = pin14, input4_2 = pin15, output4 = pin13;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1_1, OUTPUT);
  pinMode(input1_2, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2_1, OUTPUT);
  pinMode(input2_2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3_1, OUTPUT);
  pinMode(input3_2, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4_1, OUTPUT);
  pinMode(input4_2, OUTPUT);
  pinMode(output4, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;

  // Test cases for AND gates (input combinations)
  int testCases[4][2] = {{LOW, LOW}, {LOW, HIGH}, {HIGH, LOW}, {HIGH, HIGH}};
  int expectedOutputs[4] = {LOW, LOW, LOW, HIGH};  // Expected AND outputs for each case

  // Test each gate with all input combinations
  for (int i = 0; i < 4; i++) {
    // Gate 1 Test
    digitalWrite(input1_1, testCases[i][0]);
    digitalWrite(input1_2, testCases[i][1]);
    delay(20);  // Slight delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2_1, testCases[i][0]);
    digitalWrite(input2_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3_1, testCases[i][0]);
    digitalWrite(input3_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4_1, testCases[i][0]);
    digitalWrite(input4_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P" : "F");
  lcd.setCursor(8, 0);
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P" : "F");
  lcd.setCursor(0, 1);
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P" : "F");
  lcd.setCursor(8, 1);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P" : "F");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7408 Test Done");
  Serial.println(F("IC 7408 test completed."));
}

void testIC_7432() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7432...");
  Serial.println(F("Testing IC 7432..."));
  delay(3000);

  // Define pin assignments for the 7432 IC gates
  int input1_1 = pin1, input1_2 = pin2, output1 = pin3;
  int input2_1 = pin4, input2_2 = pin5, output2 = pin6;
  int input3_1 = pin11, input3_2 = pin12, output3 = pin10;
  int input4_1 = pin14, input4_2 = pin15, output4 = pin13;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1_1, OUTPUT);
  pinMode(input1_2, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2_1, OUTPUT);
  pinMode(input2_2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3_1, OUTPUT);
  pinMode(input3_2, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4_1, OUTPUT);
  pinMode(input4_2, OUTPUT);
  pinMode(output4, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;

  // Test cases for OR gates (input combinations)
  int testCases[4][2] = {{LOW, LOW}, {LOW, HIGH}, {HIGH, LOW}, {HIGH, HIGH}};
  int expectedOutputs[4] = {LOW, HIGH, HIGH, HIGH};  // Expected OR outputs for each case

  // Test each gate with all input combinations
  for (int i = 0; i < 4; i++) {
    // Gate 1 Test
    digitalWrite(input1_1, testCases[i][0]);
    digitalWrite(input1_2, testCases[i][1]);
    delay(20);  // Slightly increased delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2_1, testCases[i][0]);
    digitalWrite(input2_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3_1, testCases[i][0]);
    digitalWrite(input3_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4_1, testCases[i][0]);
    digitalWrite(input4_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P" : "F");
  lcd.setCursor(8, 0);
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P" : "F");
  lcd.setCursor(0, 1);
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P" : "F");
  lcd.setCursor(8, 1);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P" : "F");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7432 Test Done");
  Serial.println(F("IC 7432 test completed."));
}

void testIC_7486() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7486...");
  Serial.println(F("Testing IC 7486..."));
  delay(3000);

  // Define pin assignments for the 7486 IC gates
  int input1_1 = pin1, input1_2 = pin2, output1 = pin3;
  int input2_1 = pin4, input2_2 = pin5, output2 = pin6;
  int input3_1 = pin11, input3_2 = pin12, output3 = pin10;
  int input4_1 = pin14, input4_2 = pin15, output4 = pin13;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(input1_1, OUTPUT);
  pinMode(input1_2, OUTPUT);
  pinMode(output1, INPUT);

  pinMode(input2_1, OUTPUT);
  pinMode(input2_2, OUTPUT);
  pinMode(output2, INPUT);

  pinMode(input3_1, OUTPUT);
  pinMode(input3_2, OUTPUT);
  pinMode(output3, INPUT);

  pinMode(input4_1, OUTPUT);
  pinMode(input4_2, OUTPUT);
  pinMode(output4, INPUT);

  // Variables to track test results for each gate
  bool gate1_pass = true;
  bool gate2_pass = true;
  bool gate3_pass = true;
  bool gate4_pass = true;

  // Test cases for XOR gates (input combinations)
  int testCases[4][2] = {{LOW, LOW}, {LOW, HIGH}, {HIGH, LOW}, {HIGH, HIGH}};
  int expectedOutputs[4] = {LOW, HIGH, HIGH, LOW};  // Expected XOR outputs for each case

  // Test each gate with all input combinations
  for (int i = 0; i < 4; i++) {
    // Gate 1 Test
    digitalWrite(input1_1, testCases[i][0]);
    digitalWrite(input1_2, testCases[i][1]);
    delay(20);  // Slightly increased delay for stability
    if (digitalRead(output1) != expectedOutputs[i]) {
      gate1_pass = false;
    }

    // Gate 2 Test
    digitalWrite(input2_1, testCases[i][0]);
    digitalWrite(input2_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output2) != expectedOutputs[i]) {
      gate2_pass = false;
    }

    // Gate 3 Test
    digitalWrite(input3_1, testCases[i][0]);
    digitalWrite(input3_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output3) != expectedOutputs[i]) {
      gate3_pass = false;
    }

    // Gate 4 Test
    digitalWrite(input4_1, testCases[i][0]);
    digitalWrite(input4_2, testCases[i][1]);
    delay(20);
    if (digitalRead(output4) != expectedOutputs[i]) {
      gate4_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("G1: ");
  lcd.print(gate1_pass ? "P" : "F");
  lcd.setCursor(8, 0);
  lcd.print("G2: ");
  lcd.print(gate2_pass ? "P" : "F");
  lcd.setCursor(0, 1);
  lcd.print("G3: ");
  lcd.print(gate3_pass ? "P" : "F");
  lcd.setCursor(8, 1);
  lcd.print("G4: ");
  lcd.print(gate4_pass ? "P" : "F");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7486 Test Done");
  Serial.println(F("IC 7486 test completed."));
}

void testIC_7483() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7483...");
  Serial.println(F("Testing IC 7483..."));
  delay(3000);

  // Define pins for A, B, and sum outputs (S) and carry output (C)
  int A[] = {pin10, pin8, pin3, pin1};  // A0 to A3 pins
  int B[] = {pin11, pin7, pin4, pin16};  // B0 to B3 pins
  int S[] = {pin9, pin6, pin2, pin15};  // S0 to S3 pins (sum outputs)
  int carryOut = pin14;  // C4 (Carry out pin)
  int carryIn = pin13;  // C0 (Carry in pin)
  int vcc = pin5 , gnd = pin12;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  for (int i = 0; i < 4; i++) {
    pinMode(A[i], OUTPUT);
    pinMode(B[i], OUTPUT);
    pinMode(S[i], INPUT);
  }
  pinMode(carryOut, INPUT);
  pinMode(carryIn, OUTPUT);

  // Variable to track test results
  bool testPassed = true;

  // Test all input combinations for A, B, and carry-in (C0)
  for (int a = 0; a < 16; a++) {  // 4-bit numbers, so 16 combinations (0 to 15)
    for (int b = 0; b < 16; b++) {
      for (int c0 = 0; c0 <= 1; c0++) {  // carry-in (C0) is either 0 or 1
        // Set input A, B, and C0 values
        for (int i = 0; i < 4; i++) {
          digitalWrite(A[i], (a >> i) & 1);  // Set A[i] bit
          digitalWrite(B[i], (b >> i) & 1);  // Set B[i] bit
        }
        digitalWrite(carryIn, c0);  // Set carry-in (C0)

        // Small delay to allow the IC to settle
        delay(10);

        // Read sum outputs (S3, S2, S1, S0) and carry-out (C4)
        int sum = 0;
        for (int i = 0; i < 4; i++) {
          sum |= (digitalRead(S[i]) << i);  // Combine the sum outputs into a single integer
        }
        int expectedSum = a + b + c0;  // Expected sum from adding A, B, and carry-in
        int expectedCarryOut = (expectedSum > 15) ? 1 : 0;  // If the sum exceeds 15, carry-out should be 1

        // Check if the sum and carry-out match expected results
        if (sum != expectedSum % 16 || digitalRead(carryOut) != expectedCarryOut) {
          testPassed = false;
          Serial.print(F("Test failed for A = "));
          Serial.print(a, BIN);
          Serial.print(F(", B = "));
          Serial.print(b, BIN);
          Serial.print(F(", C0 = "));
          Serial.print(c0);
          Serial.print(F(", Expected sum = "));
          Serial.print(expectedSum % 16, BIN);
          Serial.print(F(", Got sum = "));
          Serial.print(sum, BIN);
          Serial.print(F(", Expected carry-out = "));
          Serial.println(expectedCarryOut);
        }
      }
    }
  }

  // Display result on LCD
  lcd.clear();
  if (testPassed) {
    lcd.setCursor(0, 1);
    lcd.print("7483 Test Passed");
    Serial.println(F("IC 7483 test passed."));
  } else {
    lcd.setCursor(0, 1);
    lcd.print("7483 Test Failed");
    Serial.println(F("IC 7483 test failed."));
  }

  delay(2000);  // Hold the result for 2 seconds
  lcd.clear();
  lcd.print("7483 Test Done");
  Serial.println(F("IC 7483 test completed."));
}

void testIC_7490() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7490...");
  Serial.println(F("Testing IC 7490..."));
  delay(3000);

  // Define pins for the 7490 IC
  int clkPin = pin6;   // Clock input pin
  int resetPin = pin1; // Reset pin
  int enablePin = pin2; // Enable pin (active high)
  int Q[] = {pin4, pin5, pin7, pin15}; // Q0 to Q3 outputs
  int vcc = pin16, gnd = pin8;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set pin modes
  pinMode(clkPin, OUTPUT);
  pinMode(resetPin, OUTPUT);
  pinMode(enablePin, OUTPUT);
  for (int i = 0; i < 4; i++) {
    pinMode(Q[i], INPUT);  // Set Q0 to Q3 as inputs (to read the output)
  }

  // Enable the counter (ENP and ENT are both connected to HIGH for normal operation)
  digitalWrite(enablePin, HIGH);

  // Variable to track test result
  bool testPassed = true;

  // Test the 7490 counting from 0 to 9
  for (int count = 0; count < 10; count++) {
    // Trigger the clock to increment the counter
    digitalWrite(clkPin, HIGH);  // Set the clock pin HIGH
    delay(10);  // Small delay for stability
    digitalWrite(clkPin, LOW);   // Set the clock pin LOW
    delay(10);  // Small delay for stability

    // Read the output Q0 to Q3
    int actualCount = 0;
    for (int i = 0; i < 4; i++) {
      actualCount |= (digitalRead(Q[i]) << i);  // Combine the Q outputs into a single integer
    }

    // Check if the counter matches the expected count
    if (actualCount != count) {
      testPassed = false;
      Serial.print(F("Test failed for expected count: "));
      Serial.print(count);
      Serial.print(F(", Got count: "));
      Serial.println(actualCount);
    }

    delay(100);  // Wait for a moment before next count (to observe the result)
  }

  // After counting, reset the counter
  digitalWrite(resetPin, HIGH);  // Assert reset
  delay(10);  // Small delay for reset
  digitalWrite(resetPin, LOW);   // Deassert reset

  // Display result on LCD
  lcd.clear();
  if (testPassed) {
    lcd.setCursor(0, 1);
    lcd.print("7490 Test Passed");
    Serial.println(F("IC 7490 test passed."));
  } else {
    lcd.setCursor(0, 1);
    lcd.print("7490 Test Failed");
    Serial.println(F("IC 7490 test failed."));
  }

  delay(2000);  // Hold the result for 2 seconds
  lcd.clear();
  lcd.print("7490 Test Done");
  Serial.println(F("IC 7490 test completed."));
}

void testIC_7447() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7447...");
  Serial.println(F("Testing IC 7447..."));
  delay(3000);

  // Define pin assignments for the 7447 IC
  int inputA = pin7, inputB = pin1, inputC = pin2, inputD = pin6;
  int outputA = pin13, outputB = pin12, outputC = pin11, outputD = pin10, outputE = pin9, outputF = pin15, outputG = pin14;
  int vcc = pin16, gnd = pin8;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);  // Set GND to low
  delay(1000);

  // Set pin modes for inputs and outputs
  pinMode(inputA, OUTPUT);
  pinMode(inputB, OUTPUT);
  pinMode(inputC, OUTPUT);
  pinMode(inputD, OUTPUT);

  pinMode(outputA, INPUT);
  pinMode(outputB, INPUT);
  pinMode(outputC, INPUT);
  pinMode(outputD, INPUT);
  pinMode(outputE, INPUT);
  pinMode(outputF, INPUT);
  pinMode(outputG, INPUT);

  // Expected segment outputs for each BCD input (0-9) for common anode displays
  int expectedOutputs[10][7] = {
    {HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, LOW},  // 0
    {LOW, HIGH, HIGH, LOW, LOW, LOW, LOW},      // 1
    {HIGH, HIGH, LOW, HIGH, HIGH, LOW, HIGH},   // 2
    {HIGH, HIGH, HIGH, HIGH, LOW, LOW, HIGH},   // 3
    {LOW, HIGH, HIGH, LOW, LOW, HIGH, HIGH},    // 4
    {HIGH, LOW, HIGH, HIGH, LOW, HIGH, HIGH},   // 5
    {HIGH, LOW, HIGH, HIGH, HIGH, HIGH, HIGH},  // 6
    {HIGH, HIGH, HIGH, LOW, LOW, LOW, LOW},     // 7
    {HIGH, HIGH, HIGH, HIGH, HIGH, HIGH, HIGH}, // 8
    {HIGH, HIGH, HIGH, HIGH, LOW, HIGH, HIGH}   // 9
  };

  bool testPassed = true;

  // Test each BCD input (0-9)
  for (int i = 0; i < 10; i++) {
    // Set BCD inputs
    Serial.println(F("-----------------------------------------------------------------------"));
    digitalWrite(inputA, LOW); // LSB
    digitalWrite(inputB, HIGH);
    digitalWrite(inputC, HIGH);
    digitalWrite(inputD, LOW); // MSB

    

    delay(1000); // Small delay for stable output reading
  Serial.println(digitalRead(outputA));
  Serial.println(digitalRead(outputB));
  Serial.println(digitalRead(outputC));
  Serial.println(digitalRead(outputD));
  Serial.println(digitalRead(outputE));
  Serial.println(digitalRead(outputF));
  Serial.println(digitalRead(outputG));
  
  
  Serial.println(expectedOutputs[i][0]);
    // Read outputs and check against expected segment states
    if (digitalRead(outputA) != expectedOutputs[i][0] ||
        digitalRead(outputB) != expectedOutputs[i][1] ||
        digitalRead(outputC) != expectedOutputs[i][2] ||
        digitalRead(outputD) != expectedOutputs[i][3] ||
        digitalRead(outputE) != expectedOutputs[i][4] ||
        digitalRead(outputF) != expectedOutputs[i][5] ||
        digitalRead(outputG) != expectedOutputs[i][6]) {
      testPassed = false;
 
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("7447 Test: ");
  lcd.print(testPassed ? "Pass" : "Fail");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7447 Test Done");
  Serial.println(F("IC 7447 test completed."));
}

void testIC_7473() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7473...");
  Serial.println(F("Testing IC 7473..."));
  delay(3000);

  // Define pin assignments for the 7473 IC flip-flops
  int J1 = pin16, K1 = pin3, CLK1 = pin1, CLR1 = pin2, Q1 = pin14;
  int J2 = pin7, K2 = pin12, CLK2 = pin5, CLR2 = pin6, Q2 = pin11;
  int vcc = pin4, gnd = pin13;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);  // Set GND to low

  // Set pin modes
  pinMode(J1, OUTPUT);
  pinMode(K1, OUTPUT);
  pinMode(CLK1, OUTPUT);
  pinMode(CLR1, OUTPUT);
  pinMode(Q1, INPUT);

  pinMode(J2, OUTPUT);
  pinMode(K2, OUTPUT);
  pinMode(CLK2, OUTPUT);
  pinMode(CLR2, OUTPUT);
  pinMode(Q2, INPUT);

  // Variables to track test results for each flip-flop
  bool flipFlop1_pass = true;
  bool flipFlop2_pass = true;

  // Test cases for JK flip-flop
  int testCases[4][2] = {
    {LOW, LOW},  // J=0, K=0: No change
    {LOW, HIGH}, // J=0, K=1: Reset
    {HIGH, LOW}, // J=1, K=0: Set
    {HIGH, HIGH} // J=1, K=1: Toggle
  };

  // Initial reset of both flip-flops
  digitalWrite(CLR1, LOW); // Clear flip-flop 1
  digitalWrite(CLR2, LOW); // Clear flip-flop 2
  delay(20);
  digitalWrite(CLR1, HIGH); // Remove clear signal
  digitalWrite(CLR2, HIGH);

  int expectedQ = LOW; // Start with Q initially LOW

  // Test each flip-flop with all input combinations
  for (int i = 0; i < 4; i++) {
    // Flip-Flop 1 Test
    digitalWrite(J1, testCases[i][0]);
    digitalWrite(K1, testCases[i][1]);

    // Apply a clock pulse to test the flip-flop
    digitalWrite(CLK1, HIGH);
    delay(20);
    digitalWrite(CLK1, LOW);
    delay(20);

    // Determine expected output for the flip-flop behavior
    if (testCases[i][0] == LOW && testCases[i][1] == LOW) {
      // No change
    } else if (testCases[i][0] == LOW && testCases[i][1] == HIGH) {
      expectedQ = LOW; // Reset
    } else if (testCases[i][0] == HIGH && testCases[i][1] == LOW) {
      expectedQ = HIGH; // Set
    } else if (testCases[i][0] == HIGH && testCases[i][1] == HIGH) {
      expectedQ = !expectedQ; // Toggle
    }

    // Check the output
    if (digitalRead(Q1) != expectedQ) {
      flipFlop1_pass = false;
    }

    // Flip-Flop 2 Test
    digitalWrite(J2, testCases[i][0]);
    digitalWrite(K2, testCases[i][1]);

    // Apply a clock pulse to test the flip-flop
    digitalWrite(CLK2, HIGH);
    delay(20);
    digitalWrite(CLK2, LOW);
    delay(20);

    // Check the output
    if (digitalRead(Q2) != expectedQ) {
      flipFlop2_pass = false;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("FF1: ");
  lcd.print(flipFlop1_pass ? "Pass" : "Fail");
  lcd.setCursor(8, 0);
  lcd.print("FF2: ");
  lcd.print(flipFlop2_pass ? "Pass" : "Fail");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7473 Test Done");
  Serial.println(F("IC 7473 test completed."));
}

void testIC_7474() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 7474...");
  Serial.println(F("Testing IC 7474..."));
  delay(3000);

  // Define pin assignments for the 7474 IC flip-flops
  int D1 = pin2, CLK1 = pin3, PRE1 = pin4, CLR1 = pin1, Q1 = pin5, Q1_bar = pin6;
  int D2 = pin14, CLK2 = pin13, PRE2 = pin12, CLR2 = pin15, Q2 = pin11, Q2_bar = pin10;
  int vcc = pin16, gnd = pin7;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);  // Set GND to low

  // Set pin modes for inputs and outputs of both flip-flops
  pinMode(D1, OUTPUT);
  pinMode(CLK1, OUTPUT);
  pinMode(PRE1, OUTPUT);
  pinMode(CLR1, OUTPUT);
  pinMode(Q1, INPUT);
  pinMode(Q1_bar, INPUT);

  pinMode(D2, OUTPUT);
  pinMode(CLK2, OUTPUT);
  pinMode(PRE2, OUTPUT);
  pinMode(CLR2, OUTPUT);
  pinMode(Q2, INPUT);
  pinMode(Q2_bar, INPUT);

  // Variables to track test results for each flip-flop
  bool flipFlop1_pass = true;
  bool flipFlop2_pass = true;

  // Test clear and preset functionality for flip-flop 1
  digitalWrite(CLR1, LOW);  // Assert CLR (should set Q1 = 0, Q1_bar = 1)
  delay(20);
  if (digitalRead(Q1) != LOW || digitalRead(Q1_bar) != HIGH) {
    flipFlop1_pass = false;
  }
  digitalWrite(CLR1, HIGH);  // Release CLR

  digitalWrite(PRE1, LOW);  // Assert PRE (should set Q1 = 1, Q1_bar = 0)
  delay(20);
  if (digitalRead(Q1) != HIGH || digitalRead(Q1_bar) != LOW) {
    flipFlop1_pass = false;
  }
  digitalWrite(PRE1, HIGH);  // Release PRE

  // Test data latching on clock rising edge for flip-flop 1
  digitalWrite(D1, LOW);
  digitalWrite(CLK1, LOW);  // Clock low
  digitalWrite(CLK1, HIGH); // Rising edge
  delay(20);
  if (digitalRead(Q1) != LOW || digitalRead(Q1_bar) != HIGH) {
    flipFlop1_pass = false;
  }

  digitalWrite(D1, HIGH);
  digitalWrite(CLK1, LOW);  // Clock low
  digitalWrite(CLK1, HIGH); // Rising edge
  delay(20);
  if (digitalRead(Q1) != HIGH || digitalRead(Q1_bar) != LOW) {
    flipFlop1_pass = false;
  }

  // Test clear and preset functionality for flip-flop 2
  digitalWrite(CLR2, LOW);  // Assert CLR (should set Q2 = 0, Q2_bar = 1)
  delay(20);
  if (digitalRead(Q2) != LOW || digitalRead(Q2_bar) != HIGH) {
    flipFlop2_pass = false;
  }
  digitalWrite(CLR2, HIGH);  // Release CLR

  digitalWrite(PRE2, LOW);  // Assert PRE (should set Q2 = 1, Q2_bar = 0)
  delay(20);
  if (digitalRead(Q2) != HIGH || digitalRead(Q2_bar) != LOW) {
    flipFlop2_pass = false;
  }
  digitalWrite(PRE2, HIGH);  // Release PRE

  // Test data latching on clock rising edge for flip-flop 2
  digitalWrite(D2, LOW);
  digitalWrite(CLK2, LOW);  // Clock low
  digitalWrite(CLK2, HIGH); // Rising edge
  delay(20);
  if (digitalRead(Q2) != LOW || digitalRead(Q2_bar) != HIGH) {
    flipFlop2_pass = false;
  }

  digitalWrite(D2, HIGH);
  digitalWrite(CLK2, LOW);  // Clock low
  digitalWrite(CLK2, HIGH); // Rising edge
  delay(20);
  if (digitalRead(Q2) != HIGH || digitalRead(Q2_bar) != LOW) {
    flipFlop2_pass = false;
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("FF1: ");
  lcd.print(flipFlop1_pass ? "Pass" : "Fail");
  lcd.setCursor(8, 0);
  lcd.print("FF2: ");
  lcd.print(flipFlop2_pass ? "Pass" : "Fail");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("7474 Test Done");
  Serial.println(F("IC 7474 test completed."));
}

void testIC_74138() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 74138...");
  Serial.println(F("Testing IC 74138..."));
  delay(3000);

  // Define pin assignments for the 74138 IC
  int A = pin1, B = pin2, C = pin3;               // Inputs
  int G1 = pin6, G2A = pin5, G2B = pin4;           // Enable pins
  int outputs[8] = {pin15, pin11, pin13, pin9, pin14, pin10, pin12, pin7}; // Y0 to Y7 outputs
  int vcc = pin16, gnd = pin8;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);  // Set GND to low

  // Set input and enable pin modes
  pinMode(A, OUTPUT);
  pinMode(B, OUTPUT);
  pinMode(C, OUTPUT);
  pinMode(G1, OUTPUT);
  pinMode(G2A, OUTPUT);
  pinMode(G2B, OUTPUT);

  // Set all output pins to INPUT
  for (int i = 0; i < 8; i++) {
    pinMode(outputs[i], INPUT);
  }

  // Enable the 74138 IC by setting G1 high and G2A, G2B low
  digitalWrite(G1, HIGH);
  digitalWrite(G2A, LOW);
  digitalWrite(G2B, LOW);

  // Variable to track test result
  bool testPassed = true;

  // Test cases for the decoder
  int inputCombinations[8][3] = {
    {LOW, LOW, LOW}, {LOW, LOW, HIGH}, {LOW, HIGH, LOW}, {LOW, HIGH, HIGH},
    {HIGH, LOW, LOW}, {HIGH, LOW, HIGH}, {HIGH, HIGH, LOW}, {HIGH, HIGH, HIGH}
  };

  // Test each input combination and verify the corresponding output
  for (int i = 0; i < 8; i++) {
    // Set inputs A, B, and C according to the test case
    digitalWrite(A, inputCombinations[i][0]);
    digitalWrite(B, inputCombinations[i][1]);
    digitalWrite(C, inputCombinations[i][2]);
    delay(1000); // Short delay for signal stabilization
    Serial.print(digitalRead(outputs[0])); Serial.print(digitalRead(outputs[1])); Serial.print(digitalRead(outputs[2])); Serial.print(digitalRead(outputs[3])); Serial.print(digitalRead(outputs[4])); Serial.print(digitalRead(outputs[5])); Serial.print(digitalRead(outputs[6])); Serial.print(digitalRead(outputs[7]));
    Serial.println(F("--------------------------------------------"));
    delay(1000);
    // Check all outputs
    for (int j = 0; j < 8; j++) {
      int expectedOutput = (i == j) ? LOW : HIGH; // Only the selected output should be LOW
      
      if (digitalRead(outputs[j]) != expectedOutput) {
        testPassed = false;
        break;
      }
    }
    if(!testPassed){
      break;
    }
  }

  // Display test result
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("74138 Test:");
  lcd.setCursor(0, 1);
  lcd.print(testPassed ? "Pass" : "Fail");

  // Hold the result for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("74138 Test Done");
  Serial.println(F("IC 74138 test completed."));
}

void testIC_4017() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 4017...");
  Serial.println(F("Testing IC 4017..."));
  delay(3000);

  // Define pin assignments for the 4017 IC
  int clk = pin14, reset = pin15; // Clock and Reset pins
  int vcc = pin16, gnd = pin8;  // VCC and GND pins (adjust based on your wiring)
  int outputs[10] = {pin3, pin2, pin4, pin7, pin10, pin1, pin5, pin6, pin9, pin11}; // Output pins Q0 to Q9

  // Set the VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH);  // Set VCC to HIGH (5V)
  digitalWrite(gnd, LOW);   // Set GND to LOW (0V)

  // Set the clock and reset pins
  pinMode(clk, OUTPUT);
  pinMode(reset, OUTPUT);

  // Set all output pins as INPUT initially
  for (int i = 0; i < 10; i++) {
    pinMode(outputs[i], INPUT);
  }

  // Reset the counter (optional: ensures it's in a known state)
  digitalWrite(reset, LOW);
  delay(10);
  digitalWrite(reset, HIGH);
  delay(10);

  // Variable to track if the test passes
  bool testPassed = true;

  // Apply clock pulses and check the outputs
  for (int pulse = 0; pulse < 10; pulse++) {
    // Generate a clock pulse
    digitalWrite(clk, HIGH);
    delay(10);  // Wait for the pulse
    digitalWrite(clk, LOW);
    delay(10);  // Wait for the clock to return to LOW

    // Check if the correct output is high (only one output should be high at a time)
    bool outputCorrect = false;
    for (int i = 0; i < 10; i++) {
      if (i == pulse) {
        if (digitalRead(outputs[i]) != HIGH) {
          testPassed = false;
        }
      } else {
        if (digitalRead(outputs[i]) != LOW) {
          testPassed = false;
        }
      }
    }

    // If at any point the outputs are incorrect, break the loop
    if (!testPassed) {
      break;
    }
  }

  // Clear the display and show test results
  lcd.clear();

  // Show results
  lcd.setCursor(0, 0);
  lcd.print("4017 Test: ");
  lcd.print(testPassed ? "Pass" : "Fail");

  // Hold the results for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("4017 Test Done");
  Serial.println(F("IC 4017 test completed."));
}


void testIC_741() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 741...");
  Serial.println(F("Testing IC 741..."));
  delay(3000);

  // Define pin assignments for the 741 op-amp
  int vccPin = pin15;     // Positive supply (+Vcc)
  int negVccPin = pin4;  // Negative supply (-Vcc)
  int gndPin = pin2;     // Ground (GND)
  int inputPin = pin3;   // Non-inverting input (+)
  int outputPin = pin14;  // Output pin

  // Set VCC and GND pins (power supply)
  pinMode(vccPin, OUTPUT);
  pinMode(negVccPin, OUTPUT);
  pinMode(gndPin, OUTPUT);

  // Set power supply levels
  digitalWrite(vccPin, HIGH);  // Set +Vcc (e.g., +5V or +12V)
  digitalWrite(negVccPin, LOW); // Set -Vcc (e.g., -5V or -12V)
  digitalWrite(gndPin, LOW);   // Set GND to low (ground)

  // Set input and output pins for the op-amp
  pinMode(inputPin, OUTPUT);
  pinMode(outputPin, INPUT);

  // Test variables
  bool testPassed = true;

  // Define test input voltages and expected outputs
  int testVoltages[3] = {0, 512, 1023};  // Example test values (representing analog levels)
  int tolerance = 20; // Acceptable deviation due to op-amp and analog reading limitations

  for (int i = 0; i < 3; i++) {
    // Apply test input voltage
    analogWrite(inputPin, testVoltages[i]);
    delay(50); // Delay for signal stabilization

    // Read the output
    int outputValue = analogRead(outputPin);

    // Check if output value is within tolerance of the input value
    Serial.println(F("Current number: "));
    delay(1000);
    Serial.println(i);
    delay(1000);
    Serial.println(outputValue - testVoltages[i]);
    delay(1000);
    if (abs(outputValue - testVoltages[i]) > tolerance) {
      testPassed = false;
    }
  }

  // Display the result
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("741 Test: ");
  lcd.print(testPassed ? "Pass" : "Fail");
  Serial.println(testPassed ? F("IC 741 test passed.") : F("IC 741 test failed."));

  // Hold the result for viewing
  delay(4000);

  // Final message
  lcd.clear();
  lcd.print("741 Test Done");
  Serial.println(F("IC 741 test completed."));
}

void testIC_555() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 555...");
  Serial.println(F("Testing IC 555..."));
  delay(3000);

  // Define pin assignments for the 555 IC in Monostable and Astable mode
  int triggerPin = pin2;   // Trigger input for monostable mode
  int outputPin = pin3;    // Output pin for both modes
  int controlPin = pin5;   // Control pin (usually left unconnected)
  int resetPin = pin4;     // Reset pin (normally held HIGH for normal operation)
  int vccPin = pin8;       // VCC pin for powering the 555 timer
  int gndPin = pin1;       // GND pin for grounding the 555 timer

  // Set pin modes for monostable mode testing
  pinMode(triggerPin, INPUT);
  pinMode(outputPin, INPUT);  // Output to read pulse or square wave
  pinMode(controlPin, OUTPUT);
  pinMode(resetPin, OUTPUT);
  pinMode(vccPin, OUTPUT);    // Set VCC pin to OUTPUT to provide power
  pinMode(gndPin, OUTPUT);    // Set GND pin to OUTPUT to provide ground

  // Set VCC and GND pins
  digitalWrite(vccPin, HIGH);  // Set VCC to HIGH (e.g., 5V or 9V)
  digitalWrite(gndPin, LOW);   // Set GND to LOW (ground)

  // Initialize the reset pin to HIGH (normal operation)
  digitalWrite(resetPin, HIGH);

  // Test 2: Astable Mode
  lcd.clear();
  lcd.print("Astable Mode");
  Serial.println(F("Testing Astable Mode..."));

  // Assume R1 = R2 = 4.8kΩ and C1 = 10nF
  unsigned long squareWaveStart = millis();
  unsigned long squareWaveEnd = squareWaveStart;
  int squareWaveCount = 0;

  // Count pulses for 1 second
  while (squareWaveEnd - squareWaveStart < 1000) { // Measure for 1 second
    if (digitalRead(outputPin) == HIGH) {
      squareWaveCount++;
      delayMicroseconds(50); // Avoid counting the same pulse multiple times
    }
    squareWaveEnd = millis();
  }

  float frequency = squareWaveCount; // Pulses per second = frequency in Hz
  long expectedFrequency = 18000;    // 10 kHz expected frequency
  bool astablePass = abs(frequency - expectedFrequency) < 1000; // 1 kHz tolerance

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Astable:");
  lcd.setCursor(0, 1);
  lcd.print(astablePass ? "Pass" : "Fail");
  Serial.print(F("Astable Mode Frequency: "));
  Serial.print(frequency);
  Serial.println(F(" Hz"));
  Serial.println(astablePass ? "Astable Mode Pass" : "Astable Mode Fail");
  delay(2000);

  // Test Completed
  lcd.clear();
  lcd.print("555 Test Done");
  Serial.println(F("IC 555 Test Completed."));
}

void testIC_8255() {
  lcd.setCursor(0, 1);
  lcd.print("Testing 8255...");
  Serial.println(F("Testing IC 8255..."));
  delay(3000);

  // Define Arduino pins for the 8255 data pins (D0-D7, Port A, Port B, Port C)
  const int dataPins[8] = {9, 8, 3, 2, 4, 5, 6, 7};  // Arduino pins D0-D7

  // Define control signal pins
  const int wrPin = 10;    // Write signal (WR)
  const int rdPin = A0;    // Read signal (RD)
  const int csPin = A1;   // Chip Select (CS)
  const int vcc = A3 , gnd = 13;

  // Set VCC and GND pins
  pinMode(vcc, OUTPUT);
  pinMode(gnd, OUTPUT);
  digitalWrite(vcc, HIGH); // Set VCC to high
  digitalWrite(gnd, LOW);   // Set GND to low

  // Set all data pins as OUTPUT for writing data
  for (int i = 0; i < 8; i++) {
    pinMode(dataPins[i], OUTPUT);
  }

  // Set control pins as OUTPUT for controlling the IC
  pinMode(wrPin, OUTPUT);
  pinMode(rdPin, OUTPUT);
  pinMode(csPin, OUTPUT);

  // Activate the chip (CS = LOW)
  digitalWrite(csPin, LOW);

  // Test Port A (Write HIGH then LOW on all pins)
  Serial.println(F("Testing Port A..."));
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], HIGH);  // Set each pin HIGH
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal
  delay(100);  // Wait

  // Set Port A to LOW (turn off all outputs)
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], LOW);  // Set each pin LOW
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal

  // Test Port B (Write HIGH then LOW on all pins)
  Serial.println(F("Testing Port B..."));
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], HIGH);  // Set each pin HIGH
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal
  delay(100);  // Wait

  // Set Port B to LOW (turn off all outputs)
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], LOW);  // Set each pin LOW
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal

  // Test Port C (Write HIGH then LOW on all pins)
  Serial.println(F("Testing Port C..."));
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], HIGH);  // Set each pin HIGH
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal
  delay(100);  // Wait

  // Set Port C to LOW (turn off all outputs)
  for (int i = 0; i < 8; i++) {
    digitalWrite(dataPins[i], LOW);  // Set each pin LOW
  }
  digitalWrite(wrPin, LOW);  // Write signal active
  delay(100);  // Wait for the write to happen
  digitalWrite(wrPin, HIGH); // Deactivate Write signal

  // Switch all data pins to INPUT to simulate reading
  for (int i = 0; i < 8; i++) {
    pinMode(dataPins[i], INPUT);
  }

  // Activate the chip (CS = LOW)
  digitalWrite(csPin, LOW);

  // Read the values from Port A (simulating a read operation)
  Serial.println(F("Reading from Port A..."));
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Read Port A:");
  for (int i = 0; i < 8; i++) {
    int value = digitalRead(dataPins[i]);
    Serial.print("Port A Pin ");
    Serial.print(i);
    Serial.print(": ");
    Serial.println(value);
    lcd.print(value);
    delay(500);
  }

  // Read the values from Port B (simulating a read operation)
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Read Port B:");
  for (int i = 0; i < 8; i++) {
    int value = digitalRead(dataPins[i]);
    Serial.print("Port B Pin ");
    Serial.print(i);
    Serial.print(": ");
    Serial.println(value);
    lcd.print(value);
    delay(500);
  }

  // Read the values from Port C (simulating a read operation)
  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Read Port C:");
  for (int i = 0; i < 8; i++) {
    int value = digitalRead(dataPins[i]);
    Serial.print("Port C Pin ");
    Serial.print(i);
    Serial.print(": ");
    Serial.println(value);
    lcd.print(value);
    delay(500);
  }

  // Finished test
  delay(2000);  // Hold the result for 2 seconds
  lcd.clear();
  lcd.print("8255 Test Done");
  Serial.println(F("IC 8255 test completed."));
}
```

---

## 📈 Advantages
- **Ensures reliability** of ICs before use in circuits.
- **Reduces troubleshooting time** in lab sessions.
- **Enhances learning** for students working with digital systems.
- **Compact and customizable**, suitable for academic labs.

---

## ❌ Limitations
- Not suitable for high-speed or analog-only ICs.
- Needs manual IC insertion/removal.
- Limited to logic-level voltages (5V compatible).

---

## 🛠 Future Improvements
- Add auto-detection of IC type.
- Expand support for more IC families (e.g., memory, sensors).
- Build a fully soldered, portable hardware version with 3D printed casing.
- Use AI-based pattern learning to suggest IC issues.

---

## 📚 Applications
- Engineering college laboratories
- Hardware testing benches
- Educational kits for embedded/digital design courses
- DIY electronics workbenches

---

## 👨‍💻 Project Team
- **Shubham Kalra**  
- Ridhi  
- Pratigya  
- Sumit Garg  

Under the guidance of **Ms. Kusum Arora**, J.C. Bose University of Science and Technology, YMCA, Faridabad.

---

## 📄 License
This project is for academic and learning purposes. Free to use, share, and modify with attribution.

