# AMD BC-250 BIOS Recovery & Reprogramming Guide (MXIC - MX25L12872F)

공식 AMD BC250 문서([elektricm/amd-bc250-docs](https://elektricm.github.io/amd-bc250-docs/))에 기재되지 않은 **MX25L12872F** SPI 플래시 롬 칩셋의 하드웨어 복구 및 재플래싱

---

## 1. 개요 및 복구 사유 (Background)
* **발생 원인**: 정상 작동하던 BC-250 시스템에서 BIOS 업데이트 도중 예기치 못한 전원 차단이 발생.
* **증상**: 부팅 불가(블랙 스크린 / No POST / 브릭 상태).
* **해결 방안**: 보드 내장 SPI 헤더(`J4004`)와 외부 USB 프로그래머를 연결하여 롬을 직접 재작성(외부 플래싱).

---

## 2. 하드웨어 정보 (Hardware Info)

### 2.1 메인보드 및 헤더
* **보드 모델**: AMD BC-250
* **바이오스 SPI 헤더**: `J4004`

<img width="1671" height="1443" alt="photo_1_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/9374bac7-e7ba-413b-a770-093ca94575f8" />
<img width="1244" height="1194" alt="photo_2_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/02a8577a-8d79-4853-87ac-b28e0f9c032d" />



### 2.2 롬 칩셋 (ROM Chip)
* **칩 모델명**: Macronix **MX25L12872F** (128M-bit / 16MB SPI NOR Flash, 3.3V)
<img width="1206" height="1110" alt="IMG_8313" src="https://github.com/user-attachments/assets/3eb64e91-abdc-4a6f-b4b5-9c4b3c9543de" />

* **데이터시트 참조**: Macronix MX25L12872F Datasheet (전압 레벨: 2.7V ~ 3.6V / 기본 3.3V 구동)
<img width="781" height="1038" alt="스크린샷 2026-09-04 153341" src="https://github.com/user-attachments/assets/036ec67d-b315-4c74-b6f0-dba74d3e13bc" />



---

## 3. 준비물 (Required Tools)

1. **USB BIOS 프로그래머**: 알리익스프레스(AliExpress) 등에서 구입 가능한 프로그래머 (예: CH341T Black/Green 등)
2. **점퍼 와이어(듀퐁선)**: 암-암 또는 수-암 케이블
3. **소프트웨어**: 프로그래머 제조사 전용 소프트웨어 또는 호환 툴 (예: AsProgrammer, NeoProgrammer, CH341 Programmer 등)

<img width="552" height="540" alt="스크린샷 2026-09-04 171402" src="https://github.com/user-attachments/assets/f446d290-282f-40b6-89b6-8031210ccf61" />
2.54mm Jump cable

<img width="1047" height="657" alt="스크린샷 2026-09-04 164040" src="https://github.com/user-attachments/assets/5dcde53e-34ca-4b26-b9cf-17e1445d662b" />
<img width="458" height="467" alt="스크린샷 2026-09-04 113140" src="https://github.com/user-attachments/assets/112790cb-0a6d-4c30-890b-4376b1dd71f2" />

<img width="905" height="736" alt="스크린샷 2026-09-04 164347" src="https://github.com/user-attachments/assets/f7cd3efb-3aa4-4c06-b57c-b24ab99a4428" />
<img width="795" height="579" alt="639fccb16ea5a" src="https://github.com/user-attachments/assets/9bbc1e62-41db-4484-953c-49f9f7079d4c" />


App Download URL : [http://www.yaojiedianzi.com/](http://www.yaojiedianzi.com/index.php?m=Product&a=show&id=19)





---

## 4. 핀 맵 및 배선 연결 (Pinout & Wiring)

### 4.1 기준점 및 핀아웃 (J4004 vs Programmer)
보드 상의 J4004 1번 핀(각인 또는 점 표시)과 프로그래머의 1번 핀(VCC/CS 등) 기준점을 반드시 일치시켜야 합니다.

| J4004 Pin (BC250) | 신호 이름 (Signal) | 프로그래머 핀 (Programmer) | MX25L12872F 핀 설명 |
| :--- | :--- | :--- | :--- |
| Pin 1 | CS# | CS / CE | Chip Select |
| Pin 2 | MISO / SO | MISO / SO | Serial Data Output |
| Pin 3 | WP# | WP (또는 VCC 풀업) | Write Protect |
| Pin 4 | GND | GND | Ground |
| Pin 5 | MOSI / SI | MOSI / SI | Serial Data Input |
| Pin 6 | SCK / CLK | CLK | Serial Clock |
| Pin 7 | HOLD# / RESET# | HOLD (또는 VCC 풀업) | Hold / Reset |
| Pin 8 | 3.3V (VCC) | 3.3V | Supply Voltage |

*(주의: 보드 전원을 끈 상태에서 작업해야 하며, 3.3V 전압 규격을 준수해야 합니다.)*

<img width="4284" height="5712" alt="IMG_8308" src="https://github.com/user-attachments/assets/e789bb03-ea91-4cc5-9cbe-083e9c709871" />

<img width="1500" height="1125" alt="photo_3_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/88475cb9-72bd-4462-9682-549e1171228b" />

<img width="1500" height="1125" alt="photo_3_2026-09-04_15-49-12" src="https://github.com/user-attachments/assets/623e3b6b-c92a-45a6-84e1-10cbb2f972c5" />

* J4004. 1번 ----- programmer socket 8번
* J4004. 2번 ----- programmer socket 4번
* J4004. 3번 ----- programmer socket 1번
* J4004. 4번 ----- programmer socket 6번
* J4004. 5번 ----- programmer socket 2번
* J4004. 6번 ----- programmer socket 5번
* J4004. 7번 ----- programmer socket X
* J4004. 8번 ----- programmer socket X

---

## 5. 복구 및 플래싱 절차 (Flashing Procedure)

### Step 1: 칩 인식 (Detect)
1. 배선 완료 후 프로그래머를 PC USB 포트에 연결합니다.
2. 소프트웨어에서 `Detect` 버튼을 눌러 **MX25L12872F** (또는 호환되는 MX25L128 시리즈)가 올바르게 감지되는지 확인합니다.
<img width="544" height="350" alt="스크린샷 2026-09-04 171542" src="https://github.com/user-attachments/assets/f22165a1-37f1-4e7d-bfb4-b0e09d2ea5a6" />
<img width="904" height="734" alt="스크린샷 2026-09-04 175258" src="https://github.com/user-attachments/assets/b500004f-d7e6-4f0f-9aa0-16e6a14eb18a" />


### Step 2: 기존 롬 읽기 및 백업 (Read / Backup)
* 벽돌 상태라도 혹시 모를 고유 데이터 복구를 위해 먼저 **Read**를 실행하고 `.bin` 파일로 백업합니다.

### Step 3: 롬 삭제 (Erase)
* **Erase** 버튼을 눌러 플래시 메모리 전체 영역을 초기화합니다.
* Blank Check(빈 칩 확인)를 실행하여 정상 삭제되었는지 점검합니다.

### Step 4: 정상 바이오스 쓰기 및 검증 (Write & Verify)
1. 공식 BC250 정상 BIOS 롬 파일(`.bin` / `.rom`)을 엽니다.
2. **Program / Write**를 클릭하여 롬을 기록합니다.
3. 쓰기가 끝나면 반드시 **Verify**를 진행하여 데이터 불일치(Mismatch)가 없는지 확인합니다.

BC250_3.00.ROM and BC250_3.00_CHIPSETMENU.ROM
https://gitlab.com/TuxThePenguin0/bc250-bios/-/commit/6d20835942f0c51ebf155ae2eb3831a11b3182fd
이 파일을 바로 적용함.


---

## 6. 결과 및 부팅 확인 (Verification & Boot)
* 케이블을 분리하고 보드를 기본 조립합니다.
* 전원을 인가하여 정상적으로 화면 출력이 이루어지고 BIOS 설정 화면으로 진입하는지 확인합니다.
