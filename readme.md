# SD-CARD 2.0 Adapter for Radio-86RK_SRAM / Sergey Kiselev Radio-86RK

**Languages / Языки:** [English](#english) | [Русский](#русский)

---

## English

This repository is a fork and modification of [andykarpov/radio-86rk-sdcard](https://github.com/andykarpov/radio-86rk-sdcard), which itself is based on the SD-CARD adapter project by Vinxru.

The adapter is intended for **Radio-86RK_SRAM** or the **Radio-86RK by Sergey Kiselev**. This fork modifies the hardware and uses an **ATmega328P** instead of the original ATmega8L.

## Schematic and PCB changes

- Added an **LM1117 3.3 V LDO regulator**.
- Replaced resistor voltage dividers with a **HEF4050BT buffer**.
- Added a **26-pin IDC connector** for connection to the Radio-86RK.
- Added a **10-pin AVR ISP programming connector**.
- Replaced **ATmega8L** with **ATmega328P**.

## PCB
![SD-Card top](radio-86rk-SRAM-sdcard.png)
![SD-Card bottom](radio-86rk-SRAM-sdcard-b.png)
<br>
## Photos of the assembled device
<img width="960" height="1280" alt="Radio-86RK_SD-Card_Adapter_Top" src="https://github.com/user-attachments/assets/9dd84975-a413-4cf2-a6da-686220c3d3d4" />
<img width="960" height="1280" alt="Radio-86RK_SD-Card_Adapter_Bottom" src="https://github.com/user-attachments/assets/749f7dad-c299-4a94-8488-ad285a299627" />
<img width="960" height="1280" alt="Radio-86RK_SD-Card_Adapter_Connected" src="https://github.com/user-attachments/assets/770fe97a-7e0b-4800-a030-978ad6be3dd2" />


## BOM list

### ICs

- **U1** — ATmega8L(A), ATmega328P, or compatible microcontroller. In this fork, **ATmega328P** is used.
- **U2** — HEF4050BT or compatible 4050 buffer in an SMD package.
- **IC1** — LM1117-3.3V.

### Connectors

- **CONN1** — 4UCon 19646 or compatible SD-card connector.
- **CONN2** — 2×13-pin IDC connector, 2.54 mm pitch. It may be male or female, vertical or angled, depending on your Radio-86RK connector.
- **CONN3** — vertical 2×5-pin header, 2.54 mm pitch, for AVR ISP programming.

### Resistors

- **R1, R2** — 470 Ω, 1206 package.

### Capacitors

- **C1, C2** — 10 µF tantalum polarized capacitors, 1206 package.
- **C3, C4, C5** — 0.1 µF ceramic capacitors, 1206 package.

### LEDs

- **LED1, LED2** — any preferred color, 1206 package.

## Firmware and SD-card preparation

Program the **ATmega328P** using an AVR ISP programmer.

If you use a cheap Chinese USB AVR ISP programmer, it may be first needed to be reprogrammed, so it works correctly in **USBasp** mode. 
One possible reference is: <https://www.sciencetronics.com/greenphotons/?p=938>

### Fuse bits

First read the current fuse bits:

```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:r:-:h -U hfuse:r:-:h -U efuse:r:-:h
```

Correct fuse bits for this fork:

```text
lfuse = 0xE2
hfuse = 0xD9
efuse = 0xFF
```

Meaning: **internal 8 MHz RC oscillator**, clock divider disabled, no bootloader reset.

Write the fuse bits:

```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:w:0xE2:m -U hfuse:w:0xD9:m -U efuse:w:0xFF:m
```

Flash the ATmega328P firmware:

```bash
sudo avrdude -c usbasp -p m328p -B 100 -U flash:w:86rksd.hex:i
```

## SD-card file layout

The SD card must contain the required files in the root directory.

The original project provides these files:

```text
/BOOT/boot.rk
/BOOT/sdbios.rk
/BOOT/shell.rk
```

The usual directory structure is:

```text
/BOOT
/GAMES
/SOFT
```

Use a simple FAT-formatted SD card. FAT16 or FAT32 is recommended. Avoid exFAT.

## Starting the SD-card interface on Radio-86RK

After inserting the SD card and connecting the adapter, start the loader from the Radio-86RK monitor:

```text
R0,200
G
```

The SD-card shell/menu should appear. From there you can start Radio-86RK programs and games.

## shell.rk

The firmware package contains 3 shell variants:

- **shell.1st** — original `shell.rk`from Vinxru; navigation with curser keys -> produces weird characters in the commandline
- **shell.2nd** — patched `shell.rk` from andykarpov; navigation uses the keys `A`, `Q`, `P`, `O`.
- **shell.rk** — patched `shell.rk` from explit; navigation works with the cursor keys and also with the original `A`, `Q`, `P`, `O` keys.

If you want to use the old shell.rk, rename:

```text
shell.1st -> shell.rk or shell.2nd -> shell.rk
```

<a href="https://www.pcbway.com/project/shareproject/radio_86rk_sdcard_9226c23e.html"><img src="https://www.pcbway.com/project/img/images/frompcbway-1220.png" alt="PCB from PCBWay" /></a>

---

## Русский

Этот репозиторий является форком и модификацией проекта [andykarpov/radio-86rk-sdcard](https://github.com/andykarpov/radio-86rk-sdcard), который является развитием проекта адаптера SD-CARD от Vinxru.

Адаптер предназначен для **Радио-86РК_SRAM** или **Радио-86РК от Сергея Киселёва**. В этом форке изменена схема и используется **ATmega328P** вместо оригинального ATmega8L.

## Изменения в схеме и печатной плате

- Добавлен LDO-регулятор **LM1117 3.3 V**.
- Вместо резистивных делителей используется буфер **HEF4050BT**.
- Добавлен **26-контактный IDC-разъём** для подключения.
- Добавлен **10-контактный разъём AVR ISP** для программирования.
- Вместо **ATmega8L** используется **ATmega328P**.


## BOM-list

### Микросхемы

- **U1** — ATmega8L(A), ATmega328P или совместимый микроконтроллер. В этом форке используется **ATmega328P**.
- **U2** — HEF4050BT или аналогичный буфер 4050 в SMD-корпусе.
- **IC1** — LM1117-3.3V.

### Разъёмы

- **CONN1** — 4UCon 19646 или аналогичный разъём для SD-карты.
- **CONN2** — 2×13-контактный IDC-разъём, шаг 2.54 мм. Может быть «мамой» или «папой», вертикальным или угловым, в зависимости от разъёма на вашем Радио-86РК.
- **CONN3** — вертикальный 2×5-контактный pin header, шаг 2.54 мм, для AVR ISP.

### Резисторы

- **R1, R2** — 470 Ом, корпус 1206.

### Конденсаторы

- **C1, C2** — 10 мкФ, танталовые полярные, корпус 1206.
- **C3, C4, C5** — 0.1 мкФ, керамические, корпус 1206.

### Светодиоды

- **LED1, LED2** — цвет свечения любой, корпус 1206.

## Прошивка и подготовка SD-карты

Прошивать **ATmega328P** нужно через AVR ISP-программатор.

Если используется дешёвый китайский USB AVR ISP-программатор, то возможно его сначала нужно будет перепрошить, чтобы он работал в правильном режиме **USBasp**. 
Одна из инструкций: <https://www.sciencetronics.com/greenphotons/?p=938>

### Fuse bits

Сначала читаем текущие fuse bits:

```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:r:-:h -U hfuse:r:-:h -U efuse:r:-:h
```

Правильные fuse bits для этого форка:

```text
lfuse = 0xE2
hfuse = 0xD9
efuse = 0xFF
```

Значение: **внутренний RC-генератор 8 MHz**, делитель частоты отключён, сброс в bootloader отключён.

Записываем fuse bits:

```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:w:0xE2:m -U hfuse:w:0xD9:m -U efuse:w:0xFF:m
```

Прошиваем firmware в ATmega328P:

```bash
sudo avrdude -c usbasp -p m328p -B 100 -U flash:w:86rksd.hex:i
```

## Структура SD-карты

На SD-карте должны находиться необходимые файлы в корне карты.

Автор оригинальной разработки подготовил файлы:

```text
/BOOT/boot.rk
/BOOT/sdbios.rk
/BOOT/shell.rk
```

Обычная структура каталогов:

```text
/BOOT
/GAMES
/SOFT
```

Рекомендуется использовать SD-карту с простой FAT-разметкой. FAT16 или FAT32 подходят. exFAT лучше не использовать.

## Запуск SD-card интерфейса на Радио-86РК

После установки SD-карты и подключения адаптера загрузчик запускается из монитора Радио-86РК:

```text
R0,200
G
```

После этого должно появиться меню/shell SD-карты. Из него можно запускать программы и игры для Радио-86РК.

## shell.rk

Пакет прошивки содержит 3 варианта оболочки:

shell.1st — оригинальный файл shell.rk от Vinxru; навигация выполняется клавишами управления курсором, но при этом в командной строке появляются странные символы.<br>
shell.2nd — модифицированный файл shell.rk от andykarpov; для навигации используются клавиши A, Q, P, O.<br>
shell.rk — модифицированный файл shell.rk от explit; навигация работает как клавишами управления курсором, так и оригинальными клавишами A, Q, P, O.<br>

Чтобы использовать старую версию shell.rk, переименуйте нужный файл:
```text
shell.1st -> shell.rk или shell.2nd -> shell.rk
```

## Полезные ссылки

- Обсуждение оригинального проекта на zx-pk.ru: <http://zx-pk.ru/showthread.php?t=24092>
- Тема в барахолке на zx-pk.ru: <http://zx-pk.ru/market/viewtopic.php?f=7&t=2567&p=24381>

<a href="https://www.pcbway.com/project/shareproject/radio_86rk_sdcard_9226c23e.html"><img src="https://www.pcbway.com/project/img/images/frompcbway-1220.png" alt="PCB from PCBWay" /></a>
