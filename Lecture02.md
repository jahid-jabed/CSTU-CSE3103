# 🧮 Lecture: Data Representation and Computer Arithmetic

## 📘 Topics Covered
1. Representation of Strings  
2. Binary and Hexadecimal Integer Representations and Conversions  
3. 2’s Complement Representation  
4. Computer Integer Arithmetic  
5. IEEE Floating-Point Representation and Arithmetic  

---

## 🧩 1. Representation of Strings

### 🧠 Concept Overview
A **string** is a sequence of characters stored in memory using **character encoding schemes**.  
Each character corresponds to a numeric code, typically from standards like **ASCII** or **Unicode (UTF-8)**.

---

### 🔹 ASCII Representation
ASCII (American Standard Code for Information Interchange) uses **7 bits** per character (values 0–127).

| Character | Decimal | Binary | Hex |
|------------|----------|--------|-----|
| A | 65 | 01000001 | 0x41 |
| B | 66 | 01000010 | 0x42 |
| a | 97 | 01100001 | 0x61 |
| 0 | 48 | 00110000 | 0x30 |
| Space | 32 | 00100000 | 0x20 |

---

### 🔹 Unicode / UTF-8
- **Unicode** supports characters from all languages.  
- **UTF-8** encodes characters in 1–4 bytes.  
- Example:  
  - `A` → 1 byte → 0x41  
  - `বাংলা অ` → 3 bytes per character  

---

### 💾 Example (C String Representation)
```c
char name[] = "HELLO";
```

| Address | Value (Hex) | ASCII |
|----------|--------------|--------|
| 1000 | 0x48 | H |
| 1001 | 0x45 | E |
| 1002 | 0x4C | L |
| 1003 | 0x4C | L |
| 1004 | 0x4F | O |
| 1005 | 0x00 | '\0' (null terminator) |

---

### ⚙️ String Characteristics
- Strings end with a **null character (`\0`)** in C/C++.  
- In higher-level languages, the length is often stored explicitly.

---

## 🔢 2. Binary and Hexadecimal Integer Representations and Conversions

### 🧠 Concept Overview
Computers use the **binary number system (base-2)** internally.  
For compact representation, binary numbers are often written in **hexadecimal (base-16)**.

---

### 🔹 Base Systems

| Base | Name | Digits | Example |
|------|------|---------|----------|
| 2 | Binary | 0,1 | 101101 |
| 8 | Octal | 0–7 | 55 |
| 10 | Decimal | 0–9 | 45 |
| 16 | Hexadecimal | 0–9, A–F | 2D |

---

### 🔹 Conversion Examples

**Binary → Decimal**
\[
(101101)_2 = 1×2^5 + 0×2^4 + 1×2^3 + 1×2^2 + 0×2^1 + 1×2^0 = 45_{10}
\]

**Decimal → Binary**
45 ÷ 2 = 22 R1  
22 ÷ 2 = 11 R0  
11 ÷ 2 = 5 R1  
5 ÷ 2 = 2 R1  
2 ÷ 2 = 1 R0  
1 ÷ 2 = 0 R1  

→ Binary: **101101**

**Binary ↔ Hexadecimal**

| Binary | Hex |
|--------|-----|
| 0000 | 0 |
| 0001 | 1 |
| 1010 | A |
| 1111 | F |

Example:  
`10110111₂ = B7₁₆`

---

### 💡 Tip
Group binary digits into **nibbles (4 bits)** for easy conversion to hex.

---

## 💻 3. 2’s Complement Representation

### 🧠 Concept Overview
Computers use **2’s complement** to represent **signed integers** efficiently.

It allows the same hardware to handle **addition and subtraction** seamlessly.

---

### 🔹 Representation Rules
For an *n-bit* number:
- Positive numbers → normal binary.  
- Negative numbers → invert bits and add 1.

Example for 8 bits:

| Decimal | Binary | 2’s Complement |
|----------|---------|----------------|
| +5 | 00000101 | 00000105 |
| -5 | Invert(00000101)+1 → 11111011 | 11111011 |

---

### 🔹 Range for n bits
\[
-2^{n-1} \text{ to } 2^{n-1}-1
\]

Example: For 8 bits → -128 to +127.

---

### 🔹 Example Conversion
Find 2’s complement of (-18):
1. 18 → `00010010`  
2. Invert bits → `11101101`  
3. Add 1 → `11101110`

Result: **11101110₂**

---

### 🧮 Example Addition (Overflow Demo)
```
  0100  (4)
+ 1100  (-4)
--------
1 0000  (Carry ignored → 0)
```
→ Result = 0 ✅  

---

### ⚙️ Advantages
- Only one representation for zero.  
- Hardware-friendly for arithmetic operations.  

---

## ➕ 4. Computer Integer Arithmetic

### 🧠 Integer Arithmetic Operations
Performed using **fixed-width binary arithmetic** — overflow may occur when result exceeds representable range.

---

### 🔹 Addition and Subtraction
Same as binary logic rules with carries.

| A | B | Carry In | Sum | Carry Out |
|---|---|-----------|-----|------------|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 0 | 1 |

---

### 🔹 Example
```
  0110 (6)
+ 1011 (-5)
---------
1 0001 (ignore carry)
Result = 1
```

---

### ⚠️ Overflow Detection
- If carry **into** MSB ≠ carry **out of** MSB → overflow occurred.

Example:
```
  0111 (7)
+ 0101 (5)
---------
 1100 (-4 with overflow)
```

Overflow flag = 1 ❗

---

### 🔹 Multiplication and Division
Performed via:
- **Shift-and-add** (for multiplication)
- **Shift-and-subtract** (for division)

---

### 🧮 Example: Binary Multiplication
```
   1010 (10)
×  0011 (3)
-----------
   1010
+ 1010
-----------
  11110 (30)
```

---

## 🌡️ 5. IEEE Floating-Point Representation and Arithmetic

### 🧠 Concept Overview
Floating-point numbers represent **real (fractional)** values using **scientific notation** in binary.

---

### 🔹 IEEE 754 Single-Precision Format (32-bit)

| Field | Bits | Description |
|--------|------|--------------|
| Sign (S) | 1 | 0 = positive, 1 = negative |
| Exponent (E) | 8 | Biased exponent (bias = 127) |
| Mantissa (M) | 23 | Fractional part (normalized) |

\[
\text{Value} = (-1)^S × (1.M) × 2^{(E - 127)}
\]

---

### 🔹 Example: Represent 12.25 in IEEE 754 (Single Precision)
1. 12.25₁₀ = 1100.01₂ = 1.10001 × 2³  
2. Sign = 0  
3. Exponent = 127 + 3 = 130 → `10000010`  
4. Mantissa = `10001000000000000000000`  

Final:
```
0 | 10000010 | 10001000000000000000000
```
→ `0x41440000`

---

### 🔹 IEEE 754 Double-Precision Format (64-bit)

| Field | Bits | Description |
|--------|------|--------------|
| Sign | 1 |  |
| Exponent | 11 | Bias = 1023 |
| Mantissa | 52 |  |

---

### ⚙️ Floating-Point Arithmetic Rules
- Align exponents before addition/subtraction.  
- Round result to fit mantissa precision.  
- Handle **overflow**, **underflow**, and **NaN** cases.

---

### 🧮 Example: Floating-Point Addition
\[
(1.1101 × 2^3) + (1.0010 × 2^2)
\]
→ Align exponents:  
\[
(1.1101 × 2^3) + (0.1001 × 2^3) = (10.0110 × 2^3)
\]
→ Normalize: **1.00110 × 2⁴**

---

### ⚠️ Rounding Modes
1. Round to nearest (default)  
2. Round toward 0  
3. Round up (∞)  
4. Round down (-∞)

---

### 🧩 Floating-Point Special Values
| Exponent | Mantissa | Meaning |
|-----------|-----------|----------|
| 00000000 | 000...000 | 0 |
| 00000000 | nonzero | Denormalized number |
| 11111111 | 000...000 | ∞ |
| 11111111 | nonzero | NaN (Not a Number) |

---

### 🔹 Comparison: Integer vs Floating Point

| Feature | Integer | Floating Point |
|----------|----------|----------------|
| Range | Limited by bits | Very large dynamic range |
| Speed | Faster | Slower (more complex) |
| Precision | Exact | Approximate |
| Use case | Counting, indices | Real numbers, scientific computing |

---

## 🧾 Summary Table
| Topic | Key Idea |
|--------|-----------|
| Strings | Stored as character codes (ASCII/Unicode) |
| Binary & Hex | Used for compact data representation |
| 2’s Complement | Signed integer encoding for arithmetic |
| Integer Arithmetic | Binary addition, subtraction, overflow |
| IEEE 754 | Standard for floating-point representation |

---


