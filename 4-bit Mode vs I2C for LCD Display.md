# Speed Comparison: 4-bit Mode vs I2C for LCD Display

## Assumptions:
- 16 MHz microcontroller for both interfaces.
- We are sending **1 byte (8 bits)** of data to the LCD for each update.
- For the **4-bit mode**, each byte is sent in **two 4-bit nibbles**.
- For **I2C**, we assume standard mode at **100 kHz**.
- Overheads such as control signals (RS, E) are included where necessary.

---

## 1. 4-bit Mode (Parallel Interface) Calculation:

### Data Transmission in 4-bit Mode:
- Each byte is split into **two 4-bit nibbles**.
- For each nibble, we need to:
  - Set the **RS** pin (instruction/data).
  - Toggle the **Enable (E)** pin.
  - Write the 4-bit data to the bus.
  
- We assume it takes **10 cycles** per nibble.

#### Cycles per byte:
- **Cycles per nibble** = 10 cycles.
- Since there are 2 nibbles per byte:
  - **Total cycles per byte** = 2 * 10 = 20 cycles.

### Clock Time:
- At **16 MHz**, the time for **1 clock cycle** is:
  \[
  \text{Time per cycle} = \frac{1}{16 \, \text{MHz}} = 62.5 \, \text{ns}
  \]
  
- For 20 cycles (1 byte transmission):
  \[
  \text{Time per byte} = 20 \times 62.5 \, \text{ns} = 1.25 \, \mu \text{s}
  \]

Thus, the time to send **1 byte in 4-bit mode** is **1.25 µs**.

---

## 2. I2C Mode (100 kHz) Calculation:

### Data Transmission in I2C:
- In I2C, **8 bits** are sent serially along with overhead (start/stop bits, address, acknowledgments).
- For simplicity, assume we focus only on the **8-bit data transmission**.

- The **I2C clock** is **100 kHz**.
- The time for **1 clock cycle** in I2C is:
  \[
  \text{Time per I2C clock} = \frac{1}{100 \, \text{kHz}} = 10 \, \mu \text{s}
  \]

- To send 1 byte (8 bits), we need **8 clock cycles**:
  \[
  \text{Time per byte} = 8 \times 10 \, \mu \text{s} = 80 \, \mu \text{s}
  \]

Thus, the time to send **1 byte via I2C at 100 kHz** is **80 µs**.

---

## Comparison:

- **4-bit mode time per byte**: **1.25 µs**.
- **I2C mode time per byte**: **80 µs**.

### Conclusion:
- **4-bit mode** is approximately **64 times faster** than I2C mode at **100 kHz** for a 16 MHz microcontroller.

