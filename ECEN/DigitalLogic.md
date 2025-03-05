# Digital Logic Reference Guide

## Table of Contents

1.  [Number Systems: Binary & Hexadecimal](number-systems)
2.  [Logic Gates](logic-gates)
3.  [Transistors: The Building Blocks](transistors)
4.  [Truth Tables: Mapping Logic](truth-tables)
5.  [Karnaugh Maps: Minimizing Logic Expressions](karnaugh-maps)
6.  [DeMorgan's Laws: Simplifying Logic](demorgans-laws)
7.  [Verilog: Hardware Description Language](verilog)
8.  [Components: Building Blocks of Digital Systems](components)
9.  [Analog to Digital and Digital to Analog Conversion](ad-da-conversion)

## 1\. Number Systems: Binary & Hexadecimal <a name="number-systems"></a>

### 1.1 Binary (Base-2)

#### Key Characteristics

  - Uses only two digits: 0 and 1.
  - Each position represents a power of 2.
  - Fundamental to digital systems as transistors are either ON (1) or OFF (0).

#### Conversion: Decimal to Binary

**Method: Repeated Division by 2**

1.  Divide the decimal number by 2.
2.  Record the remainder (0 or 1).
3.  Divide the quotient from the previous step by 2.
4.  Repeat steps 2 and 3 until the quotient is 0.
5.  The binary representation is the sequence of remainders read in reverse order.

**Example: Convert 42 to binary**

```
42 ÷ 2 = 21 remainder 0
21 ÷ 2 = 10 remainder 1
10 ÷ 2 =  5 remainder 0
 5 ÷ 2 =  2 remainder 1
 2 ÷ 2 =  1 remainder 0
 1 ÷ 2 =  0 remainder 1

Reading remainders in reverse: 101010
```

Thus, 42 in decimal is 101010 in binary.

#### Conversion: Binary to Decimal

**Method: Sum of Powers of 2**

1.  Identify each digit's place value (power of 2), starting from 2<sup>0</sup> on the rightmost digit.
2.  Multiply each binary digit by its place value.
3.  Sum the products to get the decimal equivalent.

**Example: Convert 110111 to decimal**

```
Binary:   1  1  0  1  1  1
Position: 2⁵ 2⁴ 2³ 2² 2¹ 2⁰
Values:   32 16  0  4  2  1

Sum = (1*32) + (1*16) + (0*8) + (1*4) + (1*2) + (1*1) = 32 + 16 + 0 + 4 + 2 + 1 = 55
```

Thus, 110111 in binary is 55 in decimal.

### 1.2 Hexadecimal (Base-16)

#### Key Characteristics

  - Uses 16 digits: 0-9 and A-F (A=10, B=11, C=12, D=13, E=14, F=15).
  - Each position represents a power of 16.
  - Convenient shorthand for binary as each hexadecimal digit represents 4 binary bits (2<sup>4</sup>=16).

#### Conversion: Decimal to Hexadecimal

**Method: Repeated Division by 16**

1.  Divide the decimal number by 16.
2.  Record the remainder (0-15, convert 10-15 to A-F).
3.  Divide the quotient from the previous step by 16.
4.  Repeat steps 2 and 3 until the quotient is 0.
5.  The hexadecimal representation is the sequence of remainders read in reverse order.

#### Conversion: Hexadecimal to Decimal

**Method: Sum of Powers of 16**

1.  Identify each digit's place value (power of 16), starting from 16<sup>0</sup> on the rightmost digit.
2.  Convert hexadecimal digits A-F to their decimal values (10-15).
3.  Multiply each hexadecimal digit's decimal value by its place value.
4.  Sum the products to get the decimal equivalent.

#### Conversion: Hexadecimal to Binary and Binary to Hexadecimal

**Hexadecimal to Binary:**

  - Replace each hexadecimal digit with its 4-bit binary equivalent.
      - 0x0 = 0000, 0x1 = 0001, 0x2 = 0010, ..., 0x9 = 1001, 0xA = 1010, ..., 0xF = 1111

**Binary to Hexadecimal:**

  - Group binary digits into sets of 4, starting from the right. Pad with leading zeros if necessary to make complete groups of 4.
  - Convert each 4-bit group to its hexadecimal equivalent.

**Example: What is 0x12 in binary? In decimal?**

**0x12 in Binary:**

  - 0x1 = 0001 in binary
  - 0x2 = 0010 in binary
  - 0x12 = 0001 0010 in binary (or simply 10010, leading zeros can be omitted if needed)

**0x12 in Decimal:**

```
Hex:     1  2
Position: 16¹ 16⁰
Values:   16  1

Sum = (1*16) + (2*1) = 16 + 2 = 18
```

Thus, 0x12 in hexadecimal is 10010 in binary and 18 in decimal.

## 2\. Logic Gates <a name="logic-gates"></a>

Logic gates are fundamental building blocks of digital circuits. They perform basic logical operations on one or more binary inputs and produce a single binary output.

### Common Logic Gates

| Gate | Symbol (ANSI) | Truth Table | Description |
|---|---|---|---|
| **AND** |  ![AND Gate](about:sanitized) |  | Output is HIGH (1) only if **both** inputs are HIGH (1). |
|   |   |  |  |
|   |   | A | B | Output (A AND B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 0 |
|   |   | 0 | 1 | 0 |
|   |   | 1 | 0 | 0 |
|   |   | 1 | 1 | 1 |
| **OR** | ![OR Gate](about:sanitized) |  | Output is HIGH (1) if **at least one** input is HIGH (1). |
|   |   |  |  |
|   |   | A | B | Output (A OR B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 0 |
|   |   | 0 | 1 | 1 |
|   |   | 1 | 0 | 1 |
|   |   | 1 | 1 | 1 |
| **NOT (Inverter)** | ![NOT Gate](about:sanitized) |  | Output is the **inverse** of the input. HIGH (1) input yields LOW (0) output, and vice versa. |
|   |   |  |  |
|   |   | A | Output (NOT A) |
|   |   |---|---|---|
|   |   | 0 | 1 |
|   |   | 1 | 0 |
| **XOR (Exclusive OR)** | ![XOR Gate](about:sanitized) |  | Output is HIGH (1) if inputs are **different**. |
|   |   |  |  |
|   |   | A | B | Output (A XOR B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 0 |
|   |   | 0 | 1 | 1 |
|   |   | 1 | 0 | 1 |
|   |   | 1 | 1 | 0 |
| **NAND (NOT AND)** | ![NAND Gate](about:sanitized) |  | Output is LOW (0) only if **both** inputs are HIGH (1). It's the negation of AND. |
|   |   |  |  |
|   |   | A | B | Output (A NAND B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 1 |
|   |   | 0 | 1 | 1 |
|   |   | 1 | 0 | 1 |
|   |   | 1 | 1 | 0 |
| **NOR (NOT OR)** | ![NOR Gate](about:sanitized) |  | Output is HIGH (1) only if **neither** input is HIGH (1). It's the negation of OR. |
|   |   |  |  |
|   |   | A | B | Output (A NOR B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 1 |
|   |   | 0 | 1 | 0 |
|   |   | 1 | 0 | 0 |
|   |   | 1 | 1 | 0 |
| **XNOR (Exclusive NOR)** | ![XNOR Gate](about:sanitized) |  | Output is HIGH (1) if inputs are the **same**. It's the negation of XOR. |
|   |   |  |  |
|   |   | A | B | Output (A XNOR B) |
|   |   |---|---|---|
|   |   | 0 | 0 | 1 |
|   |   | 0 | 1 | 0 |
|   |   | 1 | 0 | 0 |
|   |   | 1 | 1 | 1 |

### Universal Gates: NAND and NOR

NAND and NOR gates are called universal gates because any logic function can be implemented using only NAND gates (or only NOR gates). This property is crucial in digital circuit design as it simplifies manufacturing and reduces component types.

**Example: Draw an equivalent circuit for (a AND b) using only NAND gates**

```
a AND b  is equivalent to  NOT(NOT(a AND b))

Using NAND:
NOT(x) is equivalent to (x NAND x)
```

Therefore, to get (a AND b) using only NAND gates:

1.  Calculate (a NAND b).
2.  Take the output of step 1 and NAND it with itself.  This effectively inverts the NAND output, resulting in AND.

**Circuit Diagram (a AND b) using NAND gates:**

```
     -----      -----
a ---|     |----|     |--- (a AND b)
    -| NAND |--| NAND |
b ---|     |----|     |---
     -----      -----
```

**Gate-level circuit for (A XOR B) AND (A OR C)**

To draw a gate-level circuit for `(A XOR B) AND (A OR C)`, we'll break it down:

1.  **A XOR B**: This requires AND, OR, and NOT gates, or can be built using NAND gates as: `(A AND NOT B) OR (NOT A AND B)`.  Let's use standard gates for clarity first.
2.  **A OR C**:  A simple OR gate.
3.  **Result of (A XOR B) AND (A OR C)**: An AND gate combining the outputs of step 1 and step 2.

**Circuit Diagram: (A XOR B) AND (A OR C)**

```
     -----       -----
A ---|     |-----|     |-------
    -| XOR |     | AND |--- (A XOR B) AND (A OR C)
B ---|     |-----|     |-------
     -----       -----
           \     -----
C ----------\---|     |-------
             ----| OR  |
                 -----
```

## 3\. Transistors: The Building Blocks <a name="transistors"></a>

Transistors are semiconductor devices used to switch or amplify electronic signals and power. In digital logic, transistors are primarily used as switches.  We'll focus on MOSFETs (Metal-Oxide-Semiconductor Field-Effect Transistors), specifically NMOS (N-channel MOS) and PMOS (P-channel MOS).

### NMOS Transistor

  - **Behavior:**  Acts like a switch that is **ON** when the Gate (G) input is HIGH (typically VDD or logic '1') and **OFF** when the Gate input is LOW (typically GND or logic '0').
  - **Pull-down Network:** NMOS transistors are commonly used in pull-down networks to connect the output to ground (LOW) under certain input conditions.

### PMOS Transistor

  - **Behavior:** Acts like a switch that is **ON** when the Gate (G) input is LOW (logic '0') and **OFF** when the Gate input is HIGH (logic '1').
  - **Pull-up Network:** PMOS transistors are commonly used in pull-up networks to connect the output to VDD (HIGH) under certain input conditions.

### CMOS Logic (Complementary MOS)

CMOS logic utilizes both NMOS and PMOS transistors to create efficient and low-power logic gates.  For every logic gate, a pull-down network (PDN) made of NMOS transistors and a pull-up network (PUN) made of PMOS transistors are designed to work in a complementary fashion.

**Given a pull-down network, draw the pull-up network**

The key principle of CMOS design is **complementarity**.  If NMOS transistors in the PDN are in series, PMOS transistors in the PUN should be in parallel, and vice versa.

**Example:**

**Given Pull-down Network (PDN) for NAND gate:**

```
       a        b
       |        |
       n1       n2
       |        |
       -----    -----
Output-| NMOS |--| NMOS |-GND
       -----    -----
```

NMOS transistors `n1` and `n2` are in series. Output is pulled down to GND if both `a` AND `b` are HIGH.

**Pull-up Network (PUN) for NAND gate (complementary to PDN):**

```
      VDD
       |
       p1     p2
       |      |
       -----  -----
Output-|PMOS|--|PMOS|----Output
       -----  -----
       a        b (Gates are still controlled by a and b)
```

PMOS transistors `p1` and `p2` are in parallel. Output is pulled up to VDD if either `a` OR `b` is LOW.

Together, PDN and PUN form a CMOS NAND gate.

**Given a transistor diagram, determine what gate it implements**

To determine the gate from a transistor diagram, analyze the conditions under which the output is pulled HIGH and LOW.

**Example:**

**Transistor Diagram:**

```
      VDD
       |
       p1     p2
       |      |
       -----  -----
Output-|PMOS|--|PMOS|-------Output
       -----  -----          |
       a        b           |
       |                    |
       n1             -----
       |--------------| NMOS|----GND
                   -----
                     c

```

**Analysis:**

  - **Pull-up Network (PUN):** PMOS `p1` and `p2` in parallel. Output is pulled HIGH if `a` is LOW OR `b` is LOW.
  - **Pull-down Network (PDN):** NMOS `n1` and `c` in series. Output is pulled LOW if `a` is HIGH AND `c` is HIGH.

**Truth Table Construction:**

| a | b | c | PUN Condition (Output High if True) | PDN Condition (Output Low if True) | Output | Gate? |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | True (a' OR b') | False (a AND c) | 1 |  |
| 0 | 0 | 1 | True (a' OR b') | False (a AND c) | 1 |  |
| 0 | 1 | 0 | True (a' OR b') | False (a AND c) | 1 |  |
| 0 | 1 | 1 | True (a' OR b') | False (a AND c) | 1 |  |
| 1 | 0 | 0 | True (a' OR b') | False (a AND c) | 1 |  |
| 1 | 0 | 1 | True (a' OR b') | True (a AND c) | 0 |  |
| 1 | 1 | 0 | False (a' OR b') | False (a AND c) | 1 |  |
| 1 | 1 | 1 | False (a' OR b') | True (a AND c) | 0 |  |

Looking at the truth table, it seems to be similar to  **(NOT(a AND c)) AND (NOT(a AND b))**  or **NOT((a AND c) OR (a AND b))** or **NOT(a AND (b OR c))** - let's double-check boolean expression derived directly.

Output is High if (a' OR b') AND NOT(a AND c).  Simplifying this boolean algebra expression:

(a' + b') \* (a' + c') = a'a' + a'c' + b'a' + b'c' = a' + a'c' + a'b' + b'c' = a'(1 + c' + b') + b'c' = a' + b'c'

Let's create a truth table for  `a' + b'c'` :

| a | b | c | a' | b' | b'c' | a' + b'c' |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| 0 | 0 | 1 | 1 | 1 | 1 | 1 |
| 0 | 1 | 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 1 | 0 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| 1 | 0 | 1 | 0 | 1 | 1 | 1 |  \<- Mismatch here
| 1 | 1 | 0 | 0 | 0 | 0 | 0 |
| 1 | 1 | 1 | 0 | 0 | 0 | 0 |

Let's re-analyze the circuit condition. Output is LOW when **(a AND c)** is TRUE. Output is HIGH otherwise. And also when **(a' OR b')** is true (PUN condition), i.e., if a is low or b is low.

Output is LOW if **(a AND c)** is TRUE, meaning Output = NOT(a AND c) when **(a' OR b')** is disregarded. However, PUN is always trying to pull up if **(a' OR b')** is true.

Let's re-evaluate output = `a' + b' + NOT(a AND c)`  = `a' + b' + (a'+ c')` = `a' + b' + c'` which is `NOT(a AND b AND c) = (a NAND b NAND c)`. This is actually a NOR of (a AND c) and (a AND b).

The gate is more complex than a basic AND/OR/NOT gate, it's a combination. It implements the function **NOT( (a AND c) OR (a AND b) ) = NOT( a AND (b OR c) ) = (a NAND (b OR c))**.

## 4\. Truth Tables: Mapping Logic <a name="truth-tables"></a>

A truth table is a tabular representation of a logic function. It lists all possible combinations of input values and the corresponding output value for each combination.

**Draw the truth table for (a AND b) NAND c**

To create a truth table for `(a AND b) NAND c`, we need to consider all combinations of inputs a, b, and c.

| a | b | c | (a AND b) | (a AND b) NAND c |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 1 |
| 0 | 0 | 1 | 0 | 1 |
| 0 | 1 | 0 | 0 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 0 | 1 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 1 | 1 | 0 |

**Prove two equations are the same using truth tables**

To prove two boolean equations are equivalent, we can construct truth tables for both equations. If the output columns are identical for all input combinations, then the equations are equivalent.

**Example: Prove DeMorgan's Law: NOT(a AND b) = NOT(a) OR NOT(b)**

**Truth Table for NOT(a AND b):**

| a | b | (a AND b) | NOT(a AND b) |
|---|---|---|---|
| 0 | 0 | 0 | 1 |
| 0 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 0 |

**Truth Table for NOT(a) OR NOT(b):**

| a | b | NOT(a) | NOT(b) | NOT(a) OR NOT(b) |
|---|---|---|---|---|
| 0 | 0 | 1 | 1 | 1 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 0 | 0 | 0 |

Since the output columns "NOT(a AND b)" and "NOT(a) OR NOT(b)" are identical for all input combinations, we have proven that **NOT(a AND b) = NOT(a) OR NOT(b)** using truth tables.

**How many rows will a truth table have?**

A truth table with *n* input variables will have **2<sup>n</sup> rows**. This is because each input variable can have two possible values (0 or 1), and with *n* variables, there are 2 multiplied by itself *n* times possible combinations.

**Construct a circuit from a given truth table**

We can construct a circuit from a truth table using **Sum-of-Products (SOP)** or **Product-of-Sums (POS)** techniques.

### Sum-of-Products (SOP)

1.  **Identify rows where the output is 1.**
2.  **For each such row, create an AND term.**
      - If an input variable is 0 in that row, use its complement (NOT).
      - If an input variable is 1 in that row, use the variable itself.
3.  **OR together all the AND terms** created in step 2.

### Product-of-Sums (POS)

1.  **Identify rows where the output is 0.**
2.  **For each such row, create an OR term.**
      - If an input variable is 0 in that row, use the variable itself.
      - If an input variable is 1 in that row, use its complement (NOT).
3.  **AND together all the OR terms** created in step 2.

**Example: Construct a circuit from the truth table of (a XOR b)**

**Truth Table for (a XOR b):**

| a | b | (a XOR b) |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

**Using SOP:**
Rows with output 1 are row 2 (a=0, b=1) and row 3 (a=1, b=0).

  - Row 2 AND term:  (NOT a) AND b  (a' AND b)
  - Row 3 AND term:  a AND (NOT b) (a AND b')
    SOP Expression: **(a' AND b) OR (a AND b')**

**Circuit Diagram (SOP for XOR):**

```
    -----     -----
a --| NOT |---| AND |-------
    -----   /   -----       \
             \               \
b ---------\---| OR  |------- (a XOR b)
             ----|     |       /
    -----     -----   /
a -------| AND |---/
b --| NOT |---|
    -----     -----
```

## 5\. Karnaugh Maps: Minimizing Logic Expressions <a name="karnaugh-maps"></a>

A Karnaugh Map (K-map) is a graphical tool used to simplify Boolean algebra expressions. It's particularly useful for expressions with up to four input variables.

**How to use a Karnaugh Map to minimize/optimize logic expressions of up to four input variables**

**Steps for K-map Minimization (for Sum of Products):**

1.  **Construct the K-map:**

      - For *n* variables, the K-map is a 2<sup>n</sup> cell grid.
      - For 2 variables, it's 2x2. For 3 variables, 2x4. For 4 variables, 4x4.
      - Label rows and columns with input variables, using Gray code ordering to ensure only one variable changes between adjacent cells.

2.  **Populate the K-map:**

      - Transfer the output values from the truth table to the corresponding cells in the K-map. Place '1's for rows where the function output is 1, and '0's (or leave blank) for output 0.

3.  **Group the '1's:**

      - Group adjacent '1's into the largest possible rectangular groups. Groups must be of size 1, 2, 4, 8, 16... (powers of 2).
      - Groups can wrap around edges of the K-map (toroidal).
      - Each '1' must be included in at least one group. Overlapping groups are allowed to maximize group size.

4.  **Determine the minimized term for each group:**

      - For each group, identify the variables that remain constant within the group.
      - For a variable that is constant at '1', use the variable itself.
      - For a variable that is constant at '0', use its complement (NOT).
      - Variables that change within the group are eliminated.
      - AND together the constant variables (or their complements) for each group.

5.  **Form the minimized SOP expression:**

      - OR together the minimized terms derived from each group.

**Example: Minimize expression from truth table:**

Let's consider a 3-variable function with the following truth table and minimize it using a K-map.

| a | b | c | Output (F) |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 |
| 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 1 |
| 1 | 0 | 0 | 1 |
| 1 | 0 | 1 | 1 |
| 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 0 |

**3-variable K-map (for variables a, b, c, typically with 'a' varying across columns and 'bc' across rows using Gray Code):**

```
     bc\a | 0 | 1 |
     ----|---|---|
     00  | 0 | 1 |
     01  | 1 | 1 |
     11  | 1 | 0 |
     10  | 0 | 1 |
```

**Grouping '1's:**

  - We can group the four '1's in the right column (a=1). This group covers rows 00, 01, 10, 11 of bc for a=1. In this group, 'b' and 'c' vary, while 'a' is always 1.  Term: **a**
  - We can group the two '1's in the top row (bc=01). This group covers a=0 and a=1 for bc=01. In this group 'a' varies, while 'b' is always 0 and 'c' is always 1. Term: **b'c**

**Minimized SOP expression: F = a OR (b' AND c)**

## 6\. DeMorgan's Laws: Simplifying Logic <a name="demorgans-laws"></a>

DeMorgan's Laws are a pair of transformation rules that are fundamental in Boolean algebra and digital logic simplification. They provide a way to convert between AND and OR operations by inverting the inputs and the operation itself.

**DeMorgan's Laws:**

1.  **NOT(a AND b) = NOT(a) OR NOT(b)**  (NAND equivalent)
    *   Boolean Algebra:  **(a ⋅ b)' = a' + b'**
    *   Gate Equivalent: A NAND gate is equivalent to an OR gate with inverted inputs.
2.  **NOT(a OR b) = NOT(a) AND NOT(b)**   (NOR equivalent)
    *   Boolean Algebra:  **(a + b)' = a' ⋅ b'**
    *   Gate Equivalent: A NOR gate is equivalent to an AND gate with inverted inputs.

These laws are invaluable for:

*   Simplifying complex Boolean expressions.
*   Converting between different gate types (NAND/NOR to AND/OR and vice-versa).
*   Designing circuits with fewer gate types (especially when using universal gates like NAND or NOR).

**Simplify NOT(a AND b) OR NOT(a AND c)**

Using DeMorgan's Law on each term:

  - NOT(a AND b) = NOT(a) OR NOT(b) = a' OR b'
  - NOT(a AND c) = NOT(a) OR NOT(c) = a' OR c'

Substitute back into the original expression:
(a' OR b') OR (a' OR c') = a' OR b' OR a' OR c'

Since  (X OR X) = X and OR is associative and commutative, we can simplify:
a' OR a' OR b' OR c' = a' OR b' OR c'

So, **NOT(a AND b) OR NOT(a AND c)  simplifies to  NOT(a) OR NOT(b) OR NOT(c)**, or in boolean notation, **a' + b' + c'**.  This is equivalent to **NOT(a AND b AND c)** or **(a NAND b NAND c)** in gate terms.

**More Examples of Simplifying Logic Expressions using DeMorgan's Laws:**

**Example 1: Simplify NOT(NOT(x) OR y)**

1.  Apply DeMorgan's Law to NOT(NOT(x) OR y), using the second law:  NOT(A OR B) = NOT(A) AND NOT(B), where A = NOT(x) and B = y.
    *   NOT(NOT(x) OR y) = NOT(NOT(x)) AND NOT(y)

2.  Simplify NOT(NOT(x)).  Double negation cancels out: NOT(NOT(x)) = x.

3.  Substitute back:
    *   NOT(NOT(x)) AND NOT(y) = x AND NOT(y)

    Therefore, **NOT(NOT(x) OR y) simplifies to x AND NOT(y)**.

**Example 2: Simplify NOT((a OR b) AND (NOT(c) OR d))**

1.  Apply DeMorgan's Law to the outer NOT operation, using the first law: NOT(A AND B) = NOT(A) OR NOT(B), where A = (a OR b) and B = (NOT(c) OR d).
    *   NOT((a OR b) AND (NOT(c) OR d)) = NOT(a OR b) OR NOT(NOT(c) OR d)

2.  Apply DeMorgan's Law again to each term:
    *   NOT(a OR b) = NOT(a) AND NOT(b) = a' AND b'
    *   NOT(NOT(c) OR d) = NOT(NOT(c)) AND NOT(d) = c AND d'

3.  Substitute the simplified terms back into the expression:
    *   (a' AND b') OR (c AND d)

    Therefore, **NOT((a OR b) AND (NOT(c) OR d)) simplifies to (NOT(a) AND NOT(b)) OR (c AND NOT(d))** or **(a' ⋅ b') + (c ⋅ d')**.

**Example 3: Simplify (NOT(x) AND y) OR NOT(NOT(x) AND y)**

1.  Let P = (NOT(x) AND y).  The expression becomes P OR NOT(P).

2.  Recall the Boolean identity:  X OR NOT(X) = 1 (always TRUE).

3.  Therefore, **(NOT(x) AND y) OR NOT(NOT(x) AND y) simplifies to 1 (TRUE)**, regardless of the values of x and y. This means the circuit always outputs HIGH.

**Example 4: Convert an expression to use only NAND gates:  a OR (b AND c)**

1.  We know that OR and AND can be constructed from NAND gates. DeMorgan's Laws are key to this conversion.  Specifically,  a OR b = NOT(NOT(a) AND NOT(b)), which is built with NANDs as ( (a NAND a) NAND (b NAND b) ). And a AND b = NOT(NOT(a AND b)), which is built with NANDs as ( (a NAND b) NAND (a NAND b) ).

2.  Let's first express  (b AND c) using NAND:
    *   (b AND c) = NOT(NOT(b AND c)) = (b NAND c) NAND (b NAND c)

3.  Now replace (b AND c) in the original expression:  a OR (b AND c)  becomes  a OR [ (b NAND c) NAND (b NAND c) ].

4.  Next, express the outer OR operation using NAND:  X OR Y = NOT(NOT(X) AND NOT(Y)) = (NOT(X) NAND NOT(Y)). Here X = a, and Y = [ (b NAND c) NAND (b NAND c) ].
    *   a OR [ (b NAND c) NAND (b NAND c) ] =  NOT(NOT(a)) NAND NOT([ (b NAND c) NAND (b NAND c) ])
    *   NOT(a) is (a NAND a).
    *   NOT([ (b NAND c) NAND (b NAND c) ]) is  ( [ (b NAND c) NAND (b NAND c) ] NAND [ (b NAND c) NAND (b NAND c) ] ) - but actually we just need NOT([ (b NAND c) NAND (b NAND c) ]) which is already a NAND operation applied to (b NAND c) NAND (b NAND c).  Let's correct our OR in NAND conversion.

    Correct conversion of OR using NANDs:  a OR b = (a' NAND b')' = (a NAND a) NAND (b NAND b) all NANDed together.  So a OR b =  ((a NAND a) NAND (b NAND b)).

5.  Applying the correct OR using NAND conversion:  a OR [ (b NAND c) NAND (b NAND c) ]  becomes:
    *   ( (a NAND a) NAND  [ ((b NAND c) NAND (b NAND c)) NAND ((b NAND c) NAND (b NAND c)) ] )

    This expression now uses only NAND operations and is equivalent to `a OR (b AND c)`.  While this looks complex, it demonstrates the principle of universal gate realization using DeMorgan's Laws and NAND gate equivalences. Circuit diagrams can help visualize this more clearly.


**Example 5: Simplify a more complex expression:  NOT((x OR y) AND NOT(x AND NOT(y)))**

1.  Apply DeMorgan's Law to the outer NOT:  NOT(A AND B) = NOT(A) OR NOT(B), where A = (x OR y) and B = NOT(x AND NOT(y)).
    *   NOT((x OR y) AND NOT(x AND NOT(y))) = NOT(x OR y) OR NOT(NOT(x AND NOT(y)))

2.  Simplify NOT(NOT(x AND NOT(y))). Double negation cancels: NOT(NOT(x AND NOT(y))) = (x AND NOT(y)).

3.  Apply DeMorgan's Law to NOT(x OR y): NOT(x OR y) = NOT(x) AND NOT(y) = x' AND y'.

4.  Substitute the simplified terms back:
    *   (x' AND y') OR (x AND NOT(y))

5.  Let's examine if further simplification is possible. Expand the expression:  (x' ⋅ y') + (x ⋅ y').  We can factor out y':
    *   y' ⋅ (x' + x)

6.  Recall the Boolean identity: X OR NOT(X) = 1 (always TRUE), so (x' + x) = 1.

7.  Substitute back:
    *   y' ⋅ 1 = y' = NOT(y)

    Therefore, **NOT((x OR y) AND NOT(x AND NOT(y))) simplifies to NOT(y)**.

These examples illustrate how DeMorgan's Laws are applied step-by-step to simplify and manipulate Boolean expressions, making them essential tools in digital logic design.

## 7\. Verilog: Hardware Description Language <a name="verilog"></a>

Verilog is a powerful Hardware Description Language (HDL) used to model, design, simulate, and verify electronic systems, primarily digital circuits. It's an industry-standard language that allows designers to describe hardware functionality at various levels of abstraction, from high-level behavioral descriptions down to detailed gate-level and transistor-level implementations. Verilog is crucial for FPGA (Field-Programmable Gate Array) and ASIC (Application-Specific Integrated Circuit) design flows.

### Levels of Abstraction in Verilog

Verilog supports different levels of abstraction, allowing designers to choose the most appropriate level based on the design stage and complexity:

1.  **Behavioral Level:** This is the highest level of abstraction. Designs are described in terms of algorithms and functional behavior, without detailing the hardware implementation. It focuses on *what* the design does, not *how* it is implemented in hardware.

    ```verilog
    // Behavioral description of a simple adder
    module behavioral_adder (
      input [7:0] a,
      input [7:0] b,
      output [7:0] sum
    );
      always @(*) begin
        sum = a + b;
      end
    endmodule
    ```

2.  **Register Transfer Level (RTL):** RTL describes the data flow between registers and the operations performed on the data. It is more detailed than behavioral level and specifies the registers, combinational logic, and their interconnections. RTL is the primary level used for synthesis into hardware.

    ```verilog
    // RTL description of a D flip-flop
    module d_flip_flop (
      input clk,
      input reset,
      input d,
      output reg q
    );
      always @(posedge clk or posedge reset) begin
        if (reset) begin
          q <= 1'b0;
        end else begin
          q <= d;
        end
      end
    endmodule
    ```

3.  **Gate Level:** At this level, the design is described in terms of logic gates (AND, OR, NOT, NAND, NOR, XOR, XNOR) and their interconnections. This level is closer to the physical implementation but is less common for initial design due to complexity. Gate-level descriptions are often generated by synthesis tools from RTL code.

    ```verilog
    // Gate-level description of an AND gate
    module and_gate_gate_level (
      input in1,
      input in2,
      output out
    );
      and g1 (out, in1, in2); // Instantiation of primitive 'and' gate
    endmodule
    ```

4.  **Switch Level (Transistor Level):** This is the lowest level of abstraction, describing the design in terms of transistors, resistors, and capacitors. It's mainly used for custom ASIC design and detailed analysis of circuit behavior.

### Key Verilog Constructs

Understanding these basic constructs is essential for writing Verilog code:

*   **Modules:** The fundamental building block in Verilog. A module encapsulates a design entity and defines its inputs, outputs, and internal behavior.

    ```verilog
    module module_name ( // Module declaration
      input input_signal,    // Input declaration
      output output_signal  // Output declaration
    );
      // ... internal logic ...
    endmodule
    ```

*   **Inputs and Outputs:** Ports that define the interface of a module for communication with other modules or the external environment.

    ```verilog
    input  clk;           // Single-bit input
    input [7:0] data_in;  // 8-bit bus input
    output enable;         // Single-bit output
    output [15:0] data_out;// 16-bit bus output
    ```

*   **Wires:** Used to connect different components within a module. `wire` represents a physical wire and is used for combinational logic connections.  Wires do not store values; they just pass signals.

    ```verilog
    wire internal_signal;
    wire [3:0] bus_wire;
    assign internal_signal = input1 & input2; // Assign value using combinational logic
    ```

*   **Reg (Registers):**  Used to model registers or memory elements that store values. `reg` variables are used in sequential logic (within `always` blocks triggered by clock edges).

    ```verilog
    reg q;
    reg [7:0] count;
    always @(posedge clk) begin
      count <= count + 1; // Non-blocking assignment for sequential logic
    end
    ```

    **Important Note on `reg` vs. `wire`**: `reg` does *not* necessarily mean a physical register in hardware. It's a data type for variables that are assigned values within procedural blocks (`always`, `initial`).  For combinational assignments (`assign`), `wire` is typically used.

*   **Assign Statements:** Used for continuous assignment, typically for combinational logic. Whenever the right-hand side expression changes, the left-hand side `wire` is updated immediately.

    ```verilog
    assign output_wire = (input_a & input_b) | input_c;
    ```

*   **Always Blocks:** Used to describe sequential or combinational logic behavior that is triggered by events.

    *   `always @(posedge clk)`:  **Sequential Logic (Clocked Behavior)** - Executes on the rising edge of the `clk` signal. Used for flip-flops, registers, state machines, etc.
    *   `always @(*)`: **Combinational Logic** - Executes whenever any input in the sensitivity list (`*` means all inputs used in the block) changes. Can also be used for level-sensitive sequential logic, but `@(posedge clk)` is more typical for registers in synchronous designs.

    ```verilog
    // Sequential always block (D flip-flop)
    always @(posedge clk) begin
      if (reset)
        data_reg <= 8'b0;
      else
        data_reg <= data_in;
    end

    // Combinational always block (2-to-1 multiplexer)
    always @(*) begin
      if (select)
        mux_out = input1;
      else
        mux_out = input0;
    end
    ```

*   **Operators:** Verilog supports a rich set of operators, including:
    *   **Arithmetic:** `+`, `-`, `*`, `/`, `%`, `**` (power)
    *   **Logical:** `&&` (AND), `||` (OR), `!` (NOT) - Used in conditional expressions, evaluate to 1-bit boolean result.
    *   **Bitwise:** `&` (AND), `|` (OR), `~` (NOT), `^` (XOR), `~^` or `^~` (XNOR) - Operate bit by bit on vectors.
    *   **Reduction:** `&`, `|`, `^`, `~&`, `~|`, `~^` - Operate on all bits of a vector to produce a 1-bit result (e.g., `&vector` is AND reduction, returns 1 if all bits are 1).
    *   **Relational:** `==`, `!=`, `<`, `<=`, `>`, `>=` - Compare values, result is 1-bit boolean.
    *   **Shift:** `<<`, `>>`, `<<<` (arithmetic left shift), `>>>` (arithmetic right shift)
    *   **Conditional:** `? :` (ternary operator)
    *   **Concatenation and Replication:** `{}`, `{{}}`

### Simulation and Synthesis

*   **Simulation:**  Verifying the functional correctness of a Verilog design before hardware implementation. Simulators execute the Verilog code and allow designers to observe signal waveforms, check behavior against specifications, and debug logic errors. Simulation is crucial for catching design flaws early in the design cycle.
*   **Synthesis:** The process of converting RTL Verilog code into a gate-level netlist that can be implemented in hardware (FPGA or ASIC). Synthesis tools take the RTL description and target technology libraries (e.g., FPGA architecture, ASIC standard cell libraries) to generate an optimized gate-level implementation. Synthesis tools also perform logic optimization, technology mapping, and generate timing reports.

### Verilog with Vivado Xilinx

Vivado Design Suite is a comprehensive FPGA design environment from Xilinx. It supports a complete design flow from Verilog/VHDL coding and simulation to synthesis, implementation, and bitstream generation for Xilinx FPGAs.

**Using Verilog in Vivado:**

1.  **Project Creation:**
    *   Launch Vivado and create a new project.
    *   Select "RTL Project" as the project type.
    *   Choose the target FPGA device (specific Xilinx FPGA part number).

2.  **Adding Verilog Source Files:**
    *   In the Vivado Project Manager, under "Project Manager" pane, click "Add Sources".
    *   Select "Add or create design sources" and click "Next".
    *   Choose "Create File" or "Add Files..." to add your Verilog (`.v`) source files.
    *   Specify the module name and define input/output ports when creating a new file or add existing Verilog files.

3.  **Writing Verilog Code for Xilinx FPGAs:**
    *   Write Verilog code adhering to good RTL coding practices for synthesis.
    *   Utilize Xilinx-specific primitives and libraries if needed (e.g., for instantiation of specific FPGA resources like block RAMs, DSP blocks, etc.).  While standard Verilog constructs are generally portable, leveraging FPGA-specific features often improves performance or resource utilization for Xilinx devices.

    ```verilog
    // Example: Instantiating a Xilinx Block RAM (BRAM) - (Simplified)
    module bram_example (
      input clk,
      input [9:0] addr,
      input [31:0] data_in,
      input we, // Write enable
      output [31:0] data_out
    );

      (* RAM_STYLE = "BLOCK_RAM" *) // Attribute to infer Block RAM
      reg [31:0] bram_mem [0:1023]; // Array to represent memory

      always @(posedge clk) begin
        if (we) begin
          bram_mem[addr] <= data_in;
        end
        data_out <= bram_mem[addr];
      end

    endmodule
    ```

4.  **Simulation in Vivado:**
    *   **Adding Testbench:** Create a Verilog testbench file (`_tb.v`) to stimulate your design. Add it to the project as a simulation source.
    *   **Running Simulation:** In the "Flow Navigator", under "SIMULATION", click "Run Simulation" -> "Run Behavioral Simulation".
    *   **Waveform Viewing:** Vivado simulator opens, and you can add signals to the waveform viewer to observe their behavior over time. Use simulation controls to run, pause, step, and analyze simulation results.

5.  **Synthesis and Implementation:**
    *   **Synthesis:** In the "Flow Navigator", under "SYNTHESIS", click "Run Synthesis". Vivado synthesis tool will translate your Verilog RTL code into a gate-level netlist optimized for the target Xilinx FPGA.
    *   **Implementation:** In the "Flow Navigator", under "IMPLEMENTATION", click "Run Implementation". Implementation includes steps like:
        *   **Placement:** Placing the synthesized logic components onto the FPGA fabric.
        *   **Routing:** Interconnecting the placed components according to the netlist.
        *   **Bitstream Generation:** Generating the final bitstream file (`.bit`) that is used to program the Xilinx FPGA.

6.  **Constraints (XDC Files - Xilinx Design Constraints):**
    *   **Creating Constraints File:** In the "Project Manager", under "Project Settings", go to "Constraints" -> "Create Constraints File". Choose "XDC" as the file type.
    *   **Defining Constraints:** XDC files are used to specify design constraints, including:
        *   **Pin Assignments (Location Constraints):** Mapping module ports to specific FPGA pins.
            ```xdc
            set_property PACKAGE_PIN Y16 [get_ports clk_in]
            set_property PACKAGE_PIN W15 [get_ports {data_out[0]}]
            set_property PACKAGE_PIN V17 [get_ports {data_out[1]}]
            # ... and so on for other signals and pins
            ```
        *   **Timing Constraints:** Specifying clock periods, setup and hold time requirements.
            ```xdc
            create_clock -period 10.000 [get_ports clk_in] ; # 100 MHz clock
            ```
        *   **I/O Standards:** Defining voltage levels and other electrical characteristics for I/O pins.
            ```xdc
            set_property IOSTANDARD LVCMOS33 [get_ports clk_in]
            set_property IOSTANDARD LVCMOS33 [get_ports {data_out[*]}]
            ```
    *   **Applying Constraints:** Vivado uses the XDC file during implementation to ensure the design meets specified requirements and maps correctly to the FPGA hardware.

7.  **Bitstream Generation and FPGA Programming:**
    *   After successful implementation, in the "Flow Navigator", under "PROGRAM AND DEBUG", click "Generate Bitstream".
    *   Once the bitstream is generated, click "Open Hardware Manager" -> "Open Target" -> "Auto Connect". Ensure your FPGA board is connected and powered on.
    *   In the Hardware Manager, click "Program Device" and select your FPGA device. Vivado will program the bitstream onto the FPGA, configuring it to implement your Verilog design.

### Advanced Verilog Topics (Brief Overview)

For more complex designs, Verilog offers features such as:

*   **Parameters:** For defining constants and making modules configurable.
*   **Functions and Tasks:** For creating reusable blocks of code and improving code organization.
*   **System Tasks:** Built-in functions for simulation control, file I/O, and display (`$display`, `$monitor`, `$fopen`, `$fwrite`, etc.).
*   **User-Defined Primitives (UDPs):** For creating custom gate-level primitives.
*   **Specify Blocks:** For defining timing constraints and path delays for more accurate timing simulations.
*   **Assertions:** For formal verification and runtime checking of design properties.
*   **Interfaces:** For defining communication protocols between modules and enhancing design reusability and modularity (SystemVerilog adds more advanced interface features).

Verilog, especially in conjunction with tools like Vivado, provides a comprehensive environment for digital hardware design, enabling engineers to create, simulate, and implement complex digital systems on FPGAs and ASICs.

## 8\. Components: Building Blocks of Digital Systems <a name="components"></a>

Digital systems are built from interconnected components that perform specific, reusable functions.

### Decoders

  - **Definition:** A decoder is a combinational circuit that converts a binary code of *n* inputs into a maximum of 2<sup>n</sup> unique outputs.  For an *n*-to-2<sup>n</sup> decoder, for each input combination, exactly one output is asserted (HIGH), while all others are deasserted (LOW).
  - **Example: 2-to-4 Decoder Truth Table:**

| Input (in[1:0]) | Output (out[3:0]) |
|---|---|
| in[1] in[0] | out[3] out[2] out[1] out[0] |
|---|---|
| 0   0     | 0    0    0    1    | (Output 0 selected)
| 0   1     | 0    0    1    0    | (Output 1 selected)
| 1   0     | 0    1    0    0    | (Output 2 selected)
| 1   1     | 1    0    0    0    | (Output 3 selected)

### Encoders

  - **Definition:** An encoder is a combinational circuit that performs the inverse operation of a decoder. It converts 2<sup>n</sup> (or fewer) inputs into an *n*-bit binary code output.  Typically, only one input is active (HIGH) at any time, and the encoder outputs the binary code corresponding to the active input.
  - **Example: 4-to-2 Encoder Truth Table (Priority Encoder - handles multiple inputs being high):**

| Input (in[3:0]) | Output (out[1:0]) |  Valid Output |
|---|---|---|
| in[3] in[2] in[1] in[0] | out[1] out[0] |  |
|---|---|---|
| 0   0   0   1   | 0    0    | 1 (Input 0 priority) |
| 0   0   1   x   | 0    1    | 1 (Input 1 priority) |
| 0   1   x   x   | 1    0    | 1 (Input 2 priority) |
| 1   x   x   x   | 1    1    | 1 (Input 3 priority) |
| 0   0   0   0   | x    x    | 0 (No valid input) |

### Adders

Adders are circuits that perform binary addition.

**Half Adder:**

  - **Function:** Adds two single-bit binary numbers (A and B) and produces a Sum (S) and a Carry-out (Cout).
  - **Truth Table:**

| A | B | Sum (S) | Carry-out (Cout) |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

  - **Logic Equations:**
      - S = A XOR B
      - Cout = A AND B

**Full Adder:**

  - **Function:** Adds three single-bit binary numbers: two inputs (A and B) and a Carry-in (Cin) from a previous stage. Produces a Sum (S) and a Carry-out (Cout) to the next stage.
  - **Truth Table:**

| Cin | A | B | Sum (S) | Carry-out (Cout) |
|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 1 | 0 |
| 0 | 1 | 0 | 1 | 0 |
| 0 | 1 | 1 | 0 | 1 |
| 1 | 0 | 0 | 1 | 0 |
| 1 | 0 | 1 | 0 | 1 |
| 1 | 1 | 0 | 0 | 1 |
| 1 | 1 | 1 | 1 | 1 |

  - **Logic Equations (can be derived or minimized using K-maps):**
      - S = (A XOR B) XOR Cin
      - Cout = (A AND B) OR (Cin AND (A XOR B)) = (A AND B) OR (A AND Cin) OR (B AND Cin)

**4-bit Adder:**

  - A 4-bit adder is built by cascading four full adders. The Carry-out of each full adder stage is connected to the Carry-in of the next more significant stage.

<!-- end list -->

```
       +-----+    +-----+    +-----+    +-----+
Cin -->| FA 0|-->| FA 1|-->| FA 2|-->| FA 3|--> Cout (Final Carry)
      +-----+    +-----+    +-----+    +-----+
      A0 B0      A1 B1      A2 B2      A3 B3
      |  |       |  |       |  |       |  |
      S0 |       S1 |       S2 |       S3 |
         ----------------------------------
              4-bit Sum Output (S[3:0])
```

### Communicating with Hex Displays / LEDs / Switches

**Hex Displays (7-Segment Displays):**

  - Used to display hexadecimal digits (0-9, A-F).
  - Typically consist of 7 LEDs arranged in segments to form digits, and often a decimal point LED.
  - Require a decoder circuit to convert a binary/hexadecimal input to the correct segment patterns to display a digit.

**LEDs (Light Emitting Diodes):**

  - Semiconductor devices that emit light when current flows through them.
  - Used as indicators (on/off states).
  - Typically controlled by driving them with a logic signal through a current-limiting resistor.

**Switches:**

  - Mechanical devices used to provide binary inputs.
  - Can be configured to output logic '0' or logic '1' depending on their position (e.g., open or closed).
  - Often require debouncing circuits to avoid multiple transitions when a switch is flipped.

### 9\. Analog-to-Digital and Digital-to-Analog Conversion <a name="ad-da-conversion"></a>

**Analog-to-Digital Converters (ADCs):**

  - **Function:** Convert continuous analog signals (e.g., voltage, temperature) into discrete digital representations (binary numbers).
  - **Purpose:** Enable digital systems to process and interact with real-world analog signals.
  - **Examples:**  Microphone input to a computer (sound to digital data), sensor readings, etc.

**Digital-to-Analog Converters (DACs):**

  - **Function:** Convert discrete digital signals (binary numbers) into continuous analog signals (e.g., voltage).
  - **Purpose:** Allow digital systems to control analog outputs.
  - **Examples:**  Computer audio output to speakers (digital audio data to sound), controlling motor speed, etc.

**Basic Concept:**

  - **Sampling (ADC):** Analog signal is sampled at discrete time intervals and converted into digital values.
  - **Quantization (ADC):** The continuous range of analog values is divided into a finite number of discrete levels, and each sampled value is mapped to the closest level.
  - **Reconstruction (DAC):** Digital values are converted back into analog signals, often using techniques to smooth the output and approximate the original analog waveform.

ADCs and DACs are essential interfaces between the digital and analog worlds, allowing digital systems to interact with and control analog environments.
