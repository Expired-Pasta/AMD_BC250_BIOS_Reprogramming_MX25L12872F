# AMD BC-250 BIOS Recovery & Reprogramming Guide (MXIC - MX25L12872F)

Hardware recovery and reflashing of the **MX25L12872F** SPI flash chipset — not documented in the official AMD BC250 docs ([elektricm/amd-bc250-docs](https://elektricm.github.io/amd-bc250-docs/)).

---

## 1. Background

- **Cause**: A sudden power loss occurred during a BIOS update on an otherwise working BC-250 system.
- **Symptom**: Failure to boot (black screen / no POST / bricked).
- **Fix**: Reflash the ROM directly (external flashing) by connecting an external USB programmer to the board's onboard SPI header (`J4004`).

---

## 2. Hardware Info

### 2.1 Motherboard & Header

- **Board model**: AMD BC-250
- **BIOS SPI header**: `J4004`

### 2.2 ROM Chip

- **Chip**: Macronix **MX25L12872F** (128 Mbit / 16 MB SPI NOR flash, 3.3 V)
- **Datasheet reference**: Macronix MX25L12872F datasheet (voltage range: 2.7 V–3.6 V, nominal 3.3 V)

---

## 3. Required Tools

1. **USB BIOS programmer** — e.g. a CH341T (black/green) board, widely available on AliExpress.
2. **Jumper wires (Dupont cables)** — female-to-female or male-to-female.
3. **Software** — the programmer manufacturer's tool or a compatible one (e.g. AsProgrammer, NeoProgrammer, CH341 Programmer, etc.)

App download URL: [http://www.yaojiedianzi.com/](http://www.yaojiedianzi.com/index.php?m=Product&a=show&id=19)

---

## 4. Pinout & Wiring

### 4.1 Reference Point & Pinout (J4004 vs Programmer)

Pin 1 of J4004 on the board (marked with a silkscreen indicator or dot) must be aligned with Pin 1 (VCC/CS, etc.) on the programmer.

| J4004 Pin (BC250) | Signal          | Programmer Pin  | MX25L12872F Pin Description |
| ------------------ | --------------- | ---------------- | ---------------------------- |
| Pin 1               | CS#              | CS / CE           | Chip Select                   |
| Pin 2               | MISO / SO        | MISO / SO         | Serial Data Output            |
| Pin 3               | WP#              | WP (or VCC pull-up) | Write Protect               |
| Pin 4               | GND              | GND                | Ground                        |
| Pin 5               | MOSI / SI        | MOSI / SI          | Serial Data Input             |
| Pin 6               | SCK / CLK        | CLK                | Serial Clock                  |
| Pin 7               | HOLD# / RESET#   | HOLD (or VCC pull-up) | Hold / Reset               |
| Pin 8               | 3.3V (VCC)       | 3.3V               | Supply Voltage                |

*(Caution: work must be done with the board powered off, and the 3.3 V voltage spec must be respected.)*

---

## 5. Flashing Procedure

### Step 1: Detect

1. After wiring is complete, connect the programmer to a PC USB port.
2. In the software, press **Detect** to confirm that **MX25L12872F** (or a compatible MX25L128 series chip) is correctly recognized.

### Step 2: Read / Backup

- Even on a bricked chip, run **Read** first and save a `.bin` backup in case any unique data can still be recovered.

### Step 3: Erase

- Press **Erase** to wipe the entire flash memory region.
- Run a **Blank Check** to confirm the erase completed correctly.

### Step 4: Write & Verify

1. Open a known-good BC250 BIOS ROM file (`.bin` / `.rom`).
2. Click **Program / Write** to flash the ROM.
3. Once writing finishes, always run **Verify** to confirm there is no data mismatch.

This guide used `BC250_3.00.ROM` and `BC250_3.00_CHIPSETMENU.ROM` directly, from: <https://gitlab.com/TuxThePenguin0/bc250-bios/-/commit/6d20835942f0c51ebf155ae2eb3831a11b3182fd>

---

## 6. Verification & Boot

- Disconnect the cables and reassemble the board.
- Power it on and confirm normal video output and that it boots into the BIOS setup screen.
