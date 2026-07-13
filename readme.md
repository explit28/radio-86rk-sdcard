# Адаптер SD-CARD 2.0 для Радио-86РК_SRAM или Радио-86РК от Сергея Киселева

Этот репозиторий является форком и модификацией от **andykarpov** -> andykarpov/radio-86rk-sdcard
который является развитием проекта адаптера SD-CARD  от **Vinxru**.
 


## Изменения в схеме и печатной плате
- Добавлен LDO регулятор LM1117 3v3
- Вместо резистивных делителей используется буфер HEF4050BT
- добавлен 26-контактный IDC разъем под кабель для сопряжения модуля с Радио-86РК_SRAM или Радио-86РК Сергея Киселева
- Добавлен 10-контактный разъем для программатора
- Вместо Atmega8L используется МК Atmega328P

## Фото готового устройства

<img width="456" height="585" alt="SD-Card_top" src="https://github.com/user-attachments/assets/35ecc62d-e6cc-46a9-ad95-043e64bb5ea0" />
<img width="538" height="636" alt="SD-Card_bottom" src="https://github.com/user-attachments/assets/4da28afd-f0cf-40e7-8ca3-3863e367f4ac" />


## BOM-list

Микросхемы:

- U1 - Atmega8L(A) (или Atmega328P, или аналогичный МК)

- U2 - HEF4050BT (или другого производителя, 4050 в SMD-корпусе)

- IC1 - LM1117-3.3V


Разъемы:

- CONN1 - 4UCon 19646 или аналогичный ([pdf](http://www.4uconnector.com/online/object/4udrawing/19608.pdf), [Sparkfun](https://www.sparkfun.com/products/12769), [noname разъем на ebay](http://www.ebay.com/itm/5-Pcs-SD-Memory-Card-Sockets-Connectors-Replacements-29-x-28-5-x-2-5mm-/291005194440?pt=UK_BOI_Electrical_Components_Supplies_ET&hash=item43c142e0c8))

- CONN2 - угловой 2x13-контактный IDC-разъем (шаг 2.54мм)

- CONN3 - вертикальный 2x5-контактный pin header (шаг 2.54мм)

Резисторы:

- R1, R2 - 470 Ом, корпус 1206

Конденсаторы:

- C1, C2 - 10 мкФ, танталовые полярные, корпус 1206

- C3, C4, C5 - 0.1 мкФ, керамические, корпус 1206

Светодиод:

- LED1, LED2 - цвет свечения какой нравится, корпус 1206


## Прошивка и подготовка SD-карты

Прошивать Atmega328P надо через AVR ISP програматор. 
Если у вас дешевый китайский USB AVR ISP програматор, то его надо сначало перепрошить самого чтобы он работал в правильном модусе usbasp.
В этой статье описанно как это сделать.
https://www.sciencetronics.com/greenphotons/?p=938

Прошивка микроконтроллера Atmega328P:
сначала мы смотрим какие Fuse bits уже прошиты в микросхеме:<br>
Правильные Fuse bits: (Internal 8 MHz RC, without bootloader reset)<br>

lfuse = 0xE2<br>
hfuse = 0xD9<br>
efuse = 0xFF<br> 

# читаем Fuses
```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:r:-:h -U hfuse:r:-:h -U efuse:r:-:h
```

# пишем Fuses
```bash
sudo avrdude -c usbasp -p m328p -B 100 \
  -U lfuse:w:0xE2:m -U hfuse:w:0xD9:m -U efuse:w:0xFF:m
```

# шьём прошивку атмеги
```bash
sudo avrdude -c usbasp -p m328p -B 100 -U flash:w:86rksd.hex:i
```

Автор оригинальной разработки подготовил файлы **boot.rk**, **sdbios.rk** и **shell.rk** - они должны быть в каталоге BOOT в корне SD-карты

# **shell.rk** 
в комплекте прошивки есть 2 файла shell.rk

shell.old - оригинальный shell.rk - управление буквами A, Q, P, O<br>
shell.rk - моя пропатченная версия - управление стрелками курсора + буквами A, Q, P, O,br>
Если вы хотите использовать оригинальную shell.rk - переменуйте shell.old в shell.rk<br>

## Полезные ссылки
- Обсуждение проекта на zx-pk.ru: [http://zx-pk.ru/showthread.php?t=24092](http://zx-pk.ru/showthread.php?t=24092)

- Тема в барахолке на zx-pk.ru: [http://zx-pk.ru/market/viewtopic.php?f=7&t=2567&p=24381](http://zx-pk.ru/market/viewtopic.php?f=7&t=2567&p=24381)

<a href="https://www.pcbway.com/project/shareproject/radio_86rk_sdcard_9226c23e.html"><img src="https://www.pcbway.com/project/img/images/frompcbway-1220.png" alt="PCB from PCBWay" /></a>
