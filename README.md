# openocd_and_gdb
This README provides a comprehensive guide on using OpenOCD for programming and debugging ESP32 and STM32 microcontroller boards. It includes links to video tutorials and provides download links for OpenOCD. The guide covers essential commands for both ESP32 and STM32 boards, including programming, erasing, and debugging using OpenOCD and GDB. Additionally, it offers instructions on connecting GDB and Telnet with OpenOCD for efficient debugging. Commonly used OpenOCD commands and tips for modifying configurations are also included.


## Tutorial Links
- [Youtube Video Tutorial](https://www.youtube.com/watch?v=_1u7IOnivnM)
- [Full Tutorial Playlist](https://www.youtube.com/watch?v=qWqlkCLmZoE&list=PLERTijJOmYrDiiWd10iRHY0VRHdJwUH4g)

## Download Links
[![](https://img.shields.io/badge/OpenOCD-blue?style=for-the-badge)](https://gnutoolchains.com/arm-eabi/openocd/)

[![](https://img.shields.io/badge/STM32_GDB-blue?style=for-the-badge)](https://developer.arm.com/downloads/-/gnu-rm)
</br>
#### Alternatively, you can use the ESP32 OpenOCD where you got the ESP32 gdb also:
#### Path: `C:\Users\{user_name}\.espressif\tools\openocd-esp32\v0.12.0-esp32-20230419\openocd-esp32\bin\openocd.exe`

##  For ESP32 Board 
### OpenOCD Commands
- Command: `openocd -f board/esp32-wrover-kit-3.3v.cfg`
- Programming or Erasing using OpenOCD: 
${\textsf{\color{red}program filename [preverify] [verify] [reset] [exit] [offset]}}$
  - Example Commands:
    - `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "flash init; init; flash erase_sector 0 1 last"`
    - `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program blink.bin reset exit 0x10000"`
    - `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program_esp blink.bin 0x10000 verify exit"`
### GDB Commands
- Command: `xtensa-esp32-elf-gdb blink.elf`

## For STM-32 Blue-Pill Module
### OpenOCD Commands
- Command: `openocd -f board/stm32f3discovery.cfg`
- Alternative Command:
  - `openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg` (Note: Requires modification of "stm32f1x.cfg" file)
### GDB Commands
- Command: `arm-none-eabi-gdb stm32_blink.ino.elf`

## Connecting GDB with OpenOCD
- After initiating GDB, connect to OpenOCD using:
  - Command: `target remote localhost:3333` or `target extended-remote localhost:3333`

## Connecting Telnet with OpenOCD
- Open a Telnet Connection using Putty:
  - Set Port: `4444`
  - Set Host: `localhost`

## OpenOCD Commands from GDB
- Commands:
  - `monitor reset init`: Reset the board
  - `monitor reset halt`: Halt the board after reset
  - `monitor flash erase_address 0x10000 0x2000`: Erase flash memory region
  - `monitor flash write_image erase stm32_blink.ino.elf`: Erase and flash the device using .elf file
  - `monitor flash write_image blink.bin 0x10000`: Flash the device using .bin file from a specific address
  - `shell cls`: Clear the screen from GDB
  - `lay next`: Only works in `xtensa-esp32-elf-gdb`
  - `b loop`: Set breakpoint in the loop()
  - `b blink_example_main.c:19`: Set breakpoint in specific file for ESP32 board
  - `b stm32_blink.ino:8`: Set breakpoint in specific file for STM32 board
  - `info b`: List all breakpoints
  - `d loop`: Delete loop() breakpoint
  - `d`: Delete all breakpoints
  - `c`: Continue execution
