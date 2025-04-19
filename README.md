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

---

## 🔧 Components Used
- **Arduino Uno / 8051 Microcontroller**
- **16x2 LCD Display with I2C**
- **Copper-clad PCB and Soldered Components**
- **Jumper Wires, Resistors, Sockets**
- **Power Supply (5V Regulated)**
- **Breadboard or Custom PCB**
- **I2C Converter Module**

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
- The Arduino sends specific input patterns to the IC.
- Expected outputs are compared against real-time results.
- The LCD displays whether the IC is **working** or **faulty**.
- Modular code allows easy extension to support more IC types.

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

