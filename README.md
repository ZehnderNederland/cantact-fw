# CANable Firmware

This repository contains sources for the slcan CANable firmware, based off of the CANtact firwmare. This firmware should still compile and run on the CANtact.

## Supported Commands

- `O` - Open channel 
- `C` - Close channel 
- `S0` - Set bitrate to 10k
- `S1` - Set bitrate to 20k
- `S2` - Set bitrate to 50k
- `S3` - Set bitrate to 100k
- `S4` - Set bitrate to 125k
- `S5` - Set bitrate to 250k
- `S6` - Set bitrate to 500k
- `S7` - Set bitrate to 750k
- `S8` - Set bitrate to 1M
- `M0` - Set mode to normal mode (default)
- `M1` - Set mode to silent mode
- `A0` - Disable automatic retransmission 
- `A1` - Enable automatic retransmission (default)
- `TIIIIIIIILDD...` - Transmit data frame (Extended ID) [ID, length, data]
- `tIIILDD...` - Transmit data frame (Standard ID) [ID, length, data]
- `RIIIIIIIIL` - Transmit remote frame (Extended ID) [ID, length]
- `rIIIL` - Transmit remote frame (Standard ID) [ID, length]
- `V` - Returns firmware version and remote path as a string

Note: Channel configuration commands must be sent before opening the channel. The channel must be opened before transmitting frames.

This firmware currently does not provide any ACK/NACK feedback for serial commands.

## Building

### Requirements

#### Mingw-w64

To compile you will need mingw-w64:
http://mingw-w64.yaxm.org/doku.php/download - Use the MingW-W64-builds
-	Specify setup settings
  - Version -> newest (8.1.0)
  - Architecture -> x86_64
  - Threads -> posix
  - Exception -> she
  - Build revision -> 0
-	Install in: “C:\mingw-w64”, because mingw does not like spaces in its path (so you can’t put it in “Program Files).
-	Add the “bin” folder of “C:\mingw-w64” to the PATH variable.
  - Control panel -> System -> Advanced system settings
  - Advanced -> Environment Variables
  - User variables for <account> -> Path
  - “New”
  - Fill in the path to the ”bin” folder, most likely: “C:\mingw-w64\mingw64\bin”.


#### gcc-arm-none-eabi (cross compiler for stm32)

https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm - Use gcc-arm-none-eabi-8-2019-q3-update-win32-sha2.exe (or the latest equivalent).

-	Same as Mingw-W64, no spaces in the installation allowed. So put it in “C:/arm-none-eabi-gcc”
-	On the last installer screen enable “Add path to environment variable”.

### Build commands

Open a command prompt in the directory with the Makefile.

- If you have a CANable device, compile with `mingw32-make INTERNAL_OSCILLATOR=1`.
- If you have a CANtact device, you can compile using `mingw32-make`. 

## Flashing with the Bootloader

- Set the Boot jumper away from the screw terminals.
- Plug in the CanAble.
- Open STM32CubeProgrammer.
- Select "USB"
- Refresh and select for example Port: "USB1".
- Connect.
- Flash the .hex file.
- Reset the Boot jumper to the position closest to the screw terminals.
- Disconnect and re-connect the CanAble.

## Debugging

Debugging and flashing can be done with any STM32 Discovery board as a
programmer, or an st-link. You can also use other tools that support SWD.

To use an STM32 Discovery, run [OpenOCD](http://openocd.sourceforge.net/) using
the stm32f0x.cfg file: `openocd -f fw/stm32f0x.cfg`.

With OpenOCD running, arm-none-eabi-gdb can be used to load code and debug.

## Contributors

This code was forked from the open source project: https://github.com/normaldotcom/cantact-fw
Contributors to this were:
- [Ethan Zonca](https://github.com/normaldotcom) - Makefile fixes and code size optimization, updates for CANable
- [onejope](https://github.com/onejope) - Fixes to extended ID handling
- Phil Wise - Added dfu-util compatibility to Makefile

## License

See LICENSE.md
