# 🔌 7-Bit Ripple Carry Adder (Hardware Build)

A **7-bit Ripple Carry Adder** built entirely on breadboard using digital logic ICs — no simulator, no FPGA, pure hardware. This project adds two 7-bit binary numbers and produces a correct 7-bit sum plus a final carry-out, verified live using LED indicators and DIP switches.

---

## 📖 Overview

A Ripple Carry Adder is built by chaining multiple **Full Adder** circuits together, where the **carry-out** of each stage feeds into the **carry-in** of the next stage — like a ripple moving down a chain. This project implements a 7-bit version entirely in hardware to understand how binary addition actually happens at the gate/IC level, instead of just simulating it.

**Inputs:** Two 7-bit binary numbers (A6…A0 and B6…B0), set using DIP switches
**Outputs:** 7-bit Sum (S6…S0) + final Carry-Out (Cout), shown on LEDs

---

## 🧰 Tools & Components Used

| Component | Purpose / Qty |
|---|---|
| IC 7483 (4-bit Binary Full Adder) | 2x, cascaded to build 7-bit adder (1 bit unused) |
| Breadboard | Base circuit platform |
| DIP switches | Input bits (A and B, 7 bits each) |
| LEDs + resistors (220Ω) | Sum output & carry-out display |
| Jumper wires | Interconnections |
| 5V power supply | IC power |
| Multimeter | Debugging & voltage checks |

---

## 🧩 Block Diagram

```
        A0 B0        A1 B1        A2 B2        A3 B3        A4 B4        A5 B5        A6 B6
         │  │          │  │          │  │          │  │          │  │          │  │          │  │
         ▼  ▼          ▼  ▼          ▼  ▼          ▼  ▼          ▼  ▼          ▼  ▼          ▼  ▼
Cin=0 ─▶[FA0]─Cout─▶[FA1]─Cout─▶[FA2]─Cout─▶[FA3]─Cout─▶[FA4]─Cout─▶[FA5]─Cout─▶[FA6]─▶ Cout(final)
         │            │            │            │            │            │            │
         ▼            ▼            ▼            ▼            ▼            ▼            ▼
         S0           S1           S2           S3           S4           S5           S6
```

Each `FA` block is one Full Adder stage inside the 7483 ICs. The **carry ripples left to right**, stage by stage — this ripple delay is exactly what makes it a "Ripple Carry" Adder (and its main real-world limitation, see below).

---

## ✅ Truth Table — Single Full Adder Stage

Each stage of the chain follows this standard Full Adder logic (`A`, `B` = input bits, `Cin` = carry in, `S` = sum, `Cout` = carry out):

| A | B | Cin | Sum (S) | Cout |
|---|---|-----|---------|------|
| 0 | 0 |  0  |    0    |  0   |
| 0 | 0 |  1  |    1    |  0   |
| 0 | 1 |  0  |    1    |  0   |
| 0 | 1 |  1  |    0    |  1   |
| 1 | 0 |  0  |    1    |  0   |
| 1 | 0 |  1  |    0    |  1   |
| 1 | 1 |  0  |    0    |  1   |
| 1 | 1 |  1  |    1    |  1   |

`Cout` of every stage is wired directly into `Cin` of the next stage — chain this 7 times and you get the full 7-bit adder.

**Example run:**
```
  A = 1011010   (90)
+ B = 0110101   (53)
-----------------
  S = 10001111  (143)   → 7-bit sum = 0001111, final Cout = 1
```

---

## ⚙️ How It Works

1. Set input bits `A6…A0` and `B6…B0` using the DIP switches.
2. `Cin` of the first (LSB) stage is tied to 0 (ground).
3. Each 7483 IC internally computes 4 Full Adder stages in parallel logic, but the **carry-out of each bit still has to physically propagate** to the next bit before that stage's sum is valid.
4. Sum bits `S6…S0` light up on the LEDs in real time.
5. The final `Cout` LED lights up if the addition overflows past 7 bits.

---

## 🚧 Challenges & What I Learned

- **Carry propagation delay:** Since it's a *ripple* adder, the last bit's sum isn't valid until the carry has rippled through all previous stages — I could actually see this delay on the multimeter/LEDs when testing worst-case inputs (e.g., all 1s + 1). This is what motivated me to read about faster designs like **Carry Look-Ahead Adders**.
- **Breadboard noise & loose connections:** A few "wrong" outputs early on turned out to be bad jumper contacts, not logic errors — taught me to debug systematically stage-by-stage instead of assuming the design was wrong.
- **Power distribution:** Making sure both 7483 ICs and all LEDs shared a clean, stable ground was essential — floating grounds gave inconsistent LED brightness and false carry readings.
- **Using an unused bit:** Since two 4-bit adders give 8 bits total, I had to consciously ignore/ground the 8th bit's inputs to keep it a clean 7-bit adder.

---

## 🔮 Possible Improvements

- Extend to a full 8-bit or 16-bit adder
- Add a 7-segment display for decimal output instead of raw LEDs
- Build a Carry Look-Ahead Adder version to compare speed
- Recreate the same design in Logisim/Verilog for side-by-side comparison with the physical build

---

---

### 🧑‍💻 Built by [Ravi Kant](https://github.com/RAVIKANT-IITM) — BS Electronic Systems, IIT Madras
