################## Tutorial Link ##################
Link: https://www.youtube.com/watch?v=_1u7IOnivnM
FUll Tutorial: https://www.youtube.com/watch?v=qWqlkCLmZoE&list=PLERTijJOmYrDiiWd10iRHY0VRHdJwUH4g
###################################################

################## Download Link ##################
openocd: https://gnutoolchains.com/arm-eabi/openocd/
					or
you can use the esp32 openocd
path: C:\Users\{user_name}\.espressif\tools\openocd-esp32\v0.12.0-esp32-20230419\openocd-esp32\bin\openocd.exe					
###################################################
***************************************************
################## For ESP32 Board ################
openocd_command: openocd -f board/esp32-wrover-kit-3.3v.cfg
Program or Erase using Openocd ==>
Command: program filename [preverify] [verify] [reset] [exit] [offset]
example: 
openocd -f board/esp32-wrover-kit-3.3v.cfg -c "flash init; init; flash erase_sector 0 1 last"
openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program blink.bin reset 0x10000"
openocd -f board/esp32-wrover-kit-3.3v.cfg -c "program_esp blink.bin 0x10000 verify exit"
------------------------------------------------------------------
# Open Command-Prompt with this .elf file from th location.
gdb_command:  xtensa-esp32-elf-gdb blink.elf
#####################################################
			
################# For STM-32 Blue-Pill Module #################
openocd_command: 
openocd -f board/stm32f3discovery.cfg
			or
openocd -f interface/stlink-v2.cfg -f target/stm32f1x.cfg	==> to use this have to change the "stm32f1x.cfg" file. (Follow the Note with * mark)
------------------------------------------------------------------
# Open Command-Prompt with this .elf file from th location.
gdb_command:	arm-none-eabi-gdb stm32_blink.ino.elf
################################################################

################ connect gdb with Openocd #####################
# After gdb initiate connect openocd with this command make sure the port must be '3333' for gdb.
gdb_command: target remote localhost:3333
				or
gdb_command: target extended-remote localhost:3333
################################################################

################ connect Telnet with Openocd ###################
# Open Telnet Connection using putty
open putty ==> select "other" & set "Telnet" ==> Set Port "4444" ==> set host "localhost" ==> click open
################################################################

################# openocd commands from gdb #####################
# If gdb is used then must specify the 'monitor' keyword before the openocd command
monitor reset init	==> to reset the board
monitor reset halt	==> to halt the board after reset
monitor flash erase_address 0x10000 0x2000	==> to erase the flash from a region to a region
monitor flash write_image erase stm32_blink.ino.elf	==> to erase & flash the device using .elf file
monitor flash write_image blink.bin 0x10000	==> to flash the device using .bin file from a specific address
-----------------------------------------------
shell cls	==> to clear the screen from (gdb)
-----------------------------------------------
lay next ==> only works in xtensa-esp32-elf-gdb
b loop	==> breakpoint in the loop()	[First have to set breakpoint at loop or it will take another fle if write b <Line_No>]
b blink_example_main.c:19	==> b [file_name]:[line_no] {for ESP32 board}
b stm32_blink.ino:8	==> {for STM32 board}
info b 	==> list all breakpoints
d loop ==> delete loop() break point
d ==> delete all break point
c ==> continiue	(Press Ctrl+C to exit from Continiue)
#############################################

################# some extra openocd commands #####################
# Here are some commonly used OpenOCD commands:

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

**********************************************************************************
* Note For Changing the configuration of stm32 board for chinese blue-pill module:
1. Open cmd and type ==> where openocd

2. Go to this openocd location and search for ==> \share\openocd\scripts\target\stm32f1x.cfg
exaple: C:\Users\shuva\.espressif\tools\openocd-esp32\v0.12.0-esp32-20230419\openocd-esp32\share\openocd\scripts\target\stm32f1x.cfg

3. Change the line no 44 from "0x1ba01477" to "0x2ba01477" [Or make it '0' so it will not check any chip-id]
example: 	
				*From this
	  # this is the SW-DP tap id not the jtag tap id
      set _CPUTAPID 0x1ba01477
				*to
	  # this is the SW-DP tap id not the jtag tap id
      set _CPUTAPID 0x2ba01477