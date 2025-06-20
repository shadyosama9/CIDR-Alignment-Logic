# 📘 CIDR Alignment Logic (Using /24 Blocks as Base)

This guide helps you determine:

* How many IPs are in a CIDR block
* How many `/24`s it spans
* Which octet is affected
* What the octet must be divisible by

---

## 🔹 Step 1: Calculate Total Hosts

```bash
Total Hosts = 2^(32 - CIDR)
```

> Total number of usable IP addresses in the block.

---

## 🔹 Step 2: Calculate Number of /24 Blocks

```bash
Number of /24s = Total Hosts / 256 = 2^(24 - CIDR)
```

> A `/24` contains 256 IPs, so divide total IPs by 256.

---

## 🔹 Step 3: Determine Affected Octet

Based on how many `/24`s the block spans:

| Condition                   | Affected Octet |
| --------------------------- | -------------- |
| `/24s <= 256`               | 3rd or 4th     |
| `/24s > 256` (≥ 1 full /16) | 2nd            |

> If you span more than 256 `/24`s, it crosses into multiple `/16`s, and thus the 2nd octet becomes affected.

---

## 🔹 Step 4: Determine Required Divisibility

### ✅ Rule

* If **/24 blocks ≤ 256**:

  * Use the number **as-is** (no division)
  * Applies to **3rd or 4th octet**

* If **/24 blocks > 256**:

  * **Divide by 256** to find the step size
  * Applies to **2nd octet**

### 🧮 Formula Summary

```bash
If /24s <= 256:
    Required_Divisor = Number of /24s

If /24s > 256:
    Required_Divisor = Number of /24s ÷ 256
```

---

## ✅ Examples

### `/19`

```bash
Hosts      = 2^13 = 8192
/24s       = 8192 / 256 = 32
Affected   = 3rd octet
Divisible  = by 32
```

### `/13`

```bash
Hosts      = 2^19 = 524,288
/24s       = 524,288 / 256 = 2048
Affected   = 2nd octet
Divisible  = 2048 / 256 = 8
```

### `/23`

```bash
Hosts      = 512
/24s       = 2
Affected   = 4th octet
Divisible  = by 2
```

---

## 🧠 TL;DR Table

| CIDR | Hosts     | /24s | Affected Octet | Divisible By |
| ---- | --------- | ---- | -------------- | ------------ |
| /24  | 256       | 1    | 4th            | 1            |
| /23  | 512       | 2    | 4th            | 2            |
| /20  | 4096      | 16   | 3rd            | 16           |
| /19  | 8192      | 32   | 3rd            | 32           |
| /16  | 65536     | 256  | 3rd            | 256          |
| /15  | 131072    | 512  | 2nd            | 2            |
| /13  | 524288    | 2048 | 2nd            | 8            |
| /11  | 2,097,152 | 8192 | 2nd            | 32           |
