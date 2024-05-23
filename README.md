# openocd_and_gdb
This README provides a comprehensive guide on using OpenOCD for programming and debugging ESP32 and STM32 microcontroller boards. It includes links to video tutorials and provides download links for OpenOCD. The guide covers essential commands for both ESP32 and STM32 boards, including programming, erasing, and debugging using OpenOCD and GDB. Additionally, it offers instructions on connecting GDB and Telnet with OpenOCD for efficient debugging. Commonly used OpenOCD commands and tips for modifying configurations are also included.


## Tutorial Links
- [Youtube Video Tutorial](https://www.youtube.com/watch?v=_1u7IOnivnM)
- [Full Tutorial Playlist](https://www.youtube.com/watch?v=qWqlkCLmZoE&list=PLERTijJOmYrDiiWd10iRHY0VRHdJwUH4g)

## Download Links
[![](https://img.shields.io/badge/OpenOCD-blue?style=for-the-badge)](https://gnutoolchains.com/arm-eabi/openocd/)

[![](https://img.shields.io/badge/Arm_GNU_Toolchain-blue?style=for-the-badge)](https://developer.arm.com/downloads/-/gnu-rm)
</br>
#### Alternatively, you can use the ESP32 OpenOCD where you got the ESP32 gdb also (if esp-idf installed in your system):
#### Path: `C:\Users\{user_name}\.espressif\tools\openocd-esp32\v0.12.0-esp32-20230419\openocd-esp32\bin\openocd.exe`

##  For ESP32 Board
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/ESP32_ESP-PROG_CONNECTION.png" width="500"/></br>
### Replace Driver of ESP-PROG using Zadig Software, So follow this steps after install Zadig Software.
[![](https://img.shields.io/badge/Download_Zadig_Software-blue?style=for-the-badge)](https://zadig.akeo.ie/)
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/how_to_replace_driver_esp-prog.png" width="1000"/></br>
### OpenOCD Commands
- **Command:** `openocd -f board/esp32-wrover-kit-3.3v.cfg`
- **Programming or Erasing using OpenOCD:**
  - **Example Commands:**
    - **Erase Full Flash:** `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "flash init; init; halt; flash erase_sector 0 1 last; exit"`    
    - **Program Flash:** ${\textsf{\color{red}program filename [preverify] [verify] [reset] [exit] [offset]}}$</br>
      `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program blink.bin reset 0x10000; halt"`
      </br>or</br>
      `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program bootloader/bootloader.bin reset 0x1000; program partition_table/partition-table.bin reset 0x8000; program blink.bin reset 0x10000; halt"`      
      or</br>
    - `openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program_esp blink.bin 0x10000 verify exit"`
### GDB Commands
- **Command:** `xtensa-esp32-elf-gdb blink.elf`

## For STM-32 Blue-Pill Module
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/stlink-with-bluepill.jpeg" width="1000"/></br>
### OpenOCD Commands
- **Command:** `openocd -f board/stm32f3discovery.cfg`
- **Alternative Command:**
  - `openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg` **(Note: Requires modification of "stm32f1x.cfg" file)**
### GDB Commands
> [!TIP]
>  ${\textsf{\color{blue}open command-prompt with this .elf file location.}}$
- **Command:** `arm-none-eabi-gdb stm32_blink.ino.elf`

## Connecting GDB with OpenOCD
- **After initiating GDB, connect to OpenOCD using:**
  - Command: `target remote localhost:3333` or `target extended-remote localhost:3333`

## Connecting Telnet with OpenOCD
- **Open a Telnet Connection using Putty:**
> [!NOTE]
> open putty 🠊 select "other" & set "Telnet" 🠊 Set Port "4444" 🠊 set host "localhost" 🠊 click open
> 
  - **Set Port:** `4444`
  - **Set Host:** `localhost`

# Follow this steps for Debug
## Step-1: To debug a program, open the command prompt at the location of your build file
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/openocd_gdb_connection/pic-1.png" width="800"/></br>

## Step-2: Execute openocd as instructed by the provided figure
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/openocd_gdb_connection/pic-2.png" width="800"/></br>

## Step-3: Open another terminal and execute the gdb with the *.elf file, After gdb initiate, run the connect command to communicate gdb with openocd
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/openocd_gdb_connection/pic-3.png" width="800"/></br>

## Step-4: Now the gdb is ready to accept your debugging commands
<img src="https://github.com/shuvabratadey/openocd_and_gdb/blob/main/Pictures/openocd_gdb_connection/pic-4.png" width="800"/></br>

## Openocd and GDB Commands
> [!TIP]
>  ${\textsf{\color{blue}If gdb is used then must specify the}}$ ${\textsf{\color{red}'monitor'}}$ ${\textsf{\color{blue}keyword before the openocd command}}$

- **Commands:**
  - `monitor reset init`: Reset the board
  - `monitor reset halt`: Halt the board after reset
  - `monitor flash erase_address 0x10000 0x2000`: Erase flash memory region
  - `monitor flash write_image erase stm32_blink.ino.elf`: Erase and flash the device using .elf file
  - `monitor flash write_image blink.bin 0x10000`: Flash the device using .bin file from a specific address
  - `shell cls`: Clear the screen from GDB
  - `lay next` or `lay prev`: Only works in `xtensa-esp32-elf-gdb`
  - `b loop`: Set breakpoint in the loop()
  - `b blink_example_main.c:19`: Set breakpoint in specific file for ESP32 board  ${\textbf{\color{red}Format:}}$ ${\textsf{\color{blue}b [file-name]:[line no]}}$
  - `b stm32_blink.ino:8`: Set breakpoint in specific file for STM32 board
  - `info b`: List all breakpoints
  - `d loop`: Delete loop() breakpoint
  - `d`: Delete all breakpoints
  - `c`: Continue execution (Press Ctrl+C to exit execution)
  - `s`: or `step` (Step In) executes the next line of source code, entering any function calls encountered.
  - `n`: or `next`  (Step Over) executes the next line of source code without entering any function calls encountered.

## Here are some commonly used OpenOCD commands:
1. **help**: Displays a list of available commands and their descriptions.
   
2. **init**: Initializes OpenOCD.

3. **targets**: Lists the available targets on the connected hardware.

4. **reset**: Resets the target device.

5. **halt**: Halts the execution of the target device.

6. **resume**: Resumes the execution of the target device.

7. **flash write**: Writes data to the flash memory of the target device.

8. **flash erase**: Erases a specified region of the flash memory.

9. **flash protect**: Sets or clears the protection on flash sectors.

10. **program**: Programs the target device with a specified file.

11. **verify**: Verifies the programming of the target device against a specified file.

12. **gdb_port**: Sets the port for GDB (GNU Debugger) connection.

13. **jtag scan_chain**: Scans the JTAG chain and displays the detected devices.

14. **jtag newtap**: Creates a new JTAG tap.

15. **jtag_rclk**: Sets the JTAG RCLK frequency.

16. **adapter_khz**: Sets the JTAG adapter speed.

17. **transport select**: Selects the transport used to communicate with the target device (e.g., jtag, swd).

18. **poll off/on**: Turns on or off the target state polling.

19. **reset_config srst_only/none/hard/hard_and_srst**: Configures the reset behavior.

20. **adapter driver**: Sets the JTAG adapter driver.

----------------------------------------------------------------------------
> [!NOTE]
> ## For Changing the configuration of stm32 board for chinese blue-pill module:
1. After installing openocd, open cmd and type 🠊 ```"where openocd"```

2. Go to this openocd location and search for 🠊 ```\share\openocd\scripts\target\stm32f1x.cfg```</br>
**Exaple:** ```C:\Users\{user_name}\.espressif\tools\openocd-esp32\v0.12.0-esp32-20230419\openocd-esp32\share\openocd\scripts\target\stm32f1x.cfg```

3. Change the line no 44 from ```"0x1ba01477"``` to ```"0x2ba01477"```  From this ```set _CPUTAPID 0x1ba01477``` to ```set _CPUTAPID 0x2ba01477``` [Or make it '0' so it will not check any chip-id].
