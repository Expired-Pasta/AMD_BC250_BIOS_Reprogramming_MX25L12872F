# AMD BC-250 BIOS Recovery & Reprogramming Guide (MXIC - MX25L12872F)

Hardware recovery and reflashing of the **MX25L12872F** SPI flash chipset — not documented in the official AMD BC250 docs ([elektricm/amd-bc250-docs](https://elektricm.github.io/amd-bc250-docs/)).

---

## 1. Background

* **Cause**: A sudden power loss occurred during a BIOS update on an otherwise working BC-250 system.
* **Symptom**: Failure to boot (black screen / no POST / bricked).
* **Fix**: Reflash the ROM directly (external flashing) by connecting an external USB programmer to the board's onboard SPI header (`J4004`).

---

## 2. Hardware Info

### 2.1 Motherboard & Header

* **Board model**: AMD BC-250
* **BIOS SPI header**: `J4004`

<img width="1671" height="1443" alt="photo_1_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/9374bac7-e7ba-413b-a770-093ca94575f8" />
<img width="1244" height="1194" alt="photo_2_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/02a8577a-8d79-4853-87ac-b28e0f9c032d" />

### 2.2 ROM Chip

* **Chip**: Macronix **MX25L12872F** (128 Mbit / 16 MB SPI NOR flash, 3.3 V)
<img width="1206" height="1110" alt="IMG_8313" src="https://github.com/user-attachments/assets/3eb64e91-abdc-4a6f-b4b5-9c4b3c9543de" />

* **Datasheet reference**: Macronix MX25L12872F datasheet (voltage range: 2.7 V–3.6 V, nominal 3.3 V)
<img width="781" height="1038" alt="스크린샷 2026-09-04 153341" src="https://github.com/user-attachments/assets/036ec67d-b315-4c74-b6f0-dba74d3e13bc" />

---

## 3. Required Tools

1. **USB BIOS programmer** — e.g. a CH341T (black/green) board, widely available on AliExpress.
2. **Jumper wires (Dupont cables)** — female-to-female or male-to-female.
3. **Software** — the programmer manufacturer's tool or a compatible one (e.g. AsProgrammer, NeoProgrammer, CH341 Programmer, etc.)

<img width="552" height="540" alt="스크린샷 2026-09-04 171402" src="https://github.com/user-attachments/assets/f446d290-282f-40b6-89b6-8031210ccf61" />
2.54mm Jump cable

<img width="1047" height="657" alt="스크린샷 2026-09-04 164040" src="https://github.com/user-attachments/assets/5dcde53e-34ca-4b26-b9cf-17e1445d662b" />
<img width="458" height="467" alt="스크린샷 2026-09-04 113140" src="https://github.com/user-attachments/assets/112790cb-0a6d-4c30-890b-4376b1dd71f2" />

<img width="905" height="736" alt="스크린샷 2026-09-04 164347" src="https://github.com/user-attachments/assets/f7cd3efb-3aa4-4c06-b57c-b24ab99a4428" />
<img width="795" height="579" alt="639fccb16ea5a" src="https://github.com/user-attachments/assets/9bbc1e62-41db-4484-953c-49f9f7079d4c" />

App Download URL: [http://www.yaojiedianzi.com/](http://www.yaojiedianzi.com/index.php?m=Product&a=show&id=19)

---

## 4. Pinout & Wiring

### 4.1 Reference Point & Pinout (J4004 vs Programmer)

Pin 1 of J4004 on the board (marked with a silkscreen indicator or dot) must be aligned with Pin 1 (VCC/CS, etc.) on the programmer.

| J4004 Pin (BC250) | Signal | Programmer Pin | MX25L12872F Pin Description |
| :--- | :--- | :--- | :--- |
| Pin 1 | CS# | CS / CE | Chip Select |
| Pin 2 | MISO / SO | MISO / SO | Serial Data Output |
| Pin 3 | WP# | WP (or VCC pull-up) | Write Protect |
| Pin 4 | GND | GND | Ground |
| Pin 5 | MOSI / SI | MOSI / SI | Serial Data Input |
| Pin 6 | SCK / CLK | CLK | Serial Clock |
| Pin 7 | HOLD# / RESET# | HOLD (or VCC pull-up) | Hold / Reset |
| Pin 8 | 3.3V (VCC) | 3.3V | Supply Voltage |

*(Caution: work must be done with the board powered off, and the 3.3 V voltage spec must be respected.)*

<img width="4284" height="5712" alt="IMG_8308" src="https://github.com/user-attachments/assets/e789bb03-ea91-4cc5-9cbe-083e9c709871" />

<img width="1500" height="1125" alt="photo_3_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/88475cb9-72bd-4462-9682-549e1171228b" />

<img width="1500" height="1125" alt="photo_3_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/623e3b6b-c92a-45a6-84e1-10cbb2f972c5" />

---

## 5. Flashing Procedure

### Step 1: Detect

1. After wiring is complete, connect the programmer to a PC USB port.
2. In the software, press **Detect** to confirm that **MX25L12872F** (or a compatible MX25L128 series chip) is correctly recognized.
<img width="544" height="350" alt="스크린샷 2026-09-04 171542" src="https://github.com/user-attachments/assets/f22165a1-37f1-4e7d-bfb4-b0e09d2ea5a6" />

### Step 2: Read / Backup

* Even on a bricked chip, run **Read** first and save a `.bin` backup in case any unique data can still be recovered.

### Step 3: Erase

* Press **Erase** to wipe the entire flash memory region.
* Run a **Blank Check** to confirm the erase completed correctly.

### Step 4: Write & Verify

1. Open a known-good BC250 BIOS ROM file (`.bin` / `.rom`).
2. Click **Program / Write** to flash the ROM.
3. Once writing finishes, always run **Verify** to confirm there is no data mismatch.

BC250_3.00.ROM and BC250_3.00_CHIPSETMENU.ROM
https://gitlab.com/TuxThePenguin0/bc250-bios/-/commit/6d20835942f0c51ebf155ae2eb3831a11b3182fd
This file was applied directly.

---

## 6. Verification & Boot

* Disconnect the cables and reassemble the board.
* Power it on and confirm normal video output and that it boots into the BIOS setup screen.
