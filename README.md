# stm32flash

Open source cross platform flash program for the STM32 ARM microcontrollers using the built-in ST serial bootloader over UART or I2C

stm32flash 0.5 was released 2016-02-11 and is available at
https://sourceforge.net/projects/stm32flash/files/

The 0.5 version adds

- support for multiple bank sizes
- improved serial port support on Windows
- improved erase functionality
- improved hex parser
- many new devices and device info corrections

thanks to Antonio Borneo and many other contributors (see git log and AUTHORS file)

The 0.4 version is the work of Antonio Borneo and includes support for programming over I2C. See the included I2C.txt for more information. The code has been refactored to make it easier to add other transports.

# Features

- UART and I2C transports supported
- device identification
- write to flash/ram
- read from flash/ram
- auto-detect Intel HEX or raw binary input format with option to force binary
- flash from binary file
- save flash to binary file
- verify & retry up to N times on failed writes
- start execution at specified address
- software reset the device when finished if -R is specified
- resume already initialized connection (for when reset fails, UART only)
- GPIO signalling to enter bootloader mode (hardware dependent)

# Usage

See the included manual page for up-to-date, complete usage instructions ([PDF version](http://stm32flash.sourceforge.net/stm32flash-manual.pdf))

Usage synopsis:

    stm32flash [-bvngfhc] [-[rw] filename] [tty_device | i2c_device]
        -a bus_address  Bus address (e.g. for I2C port)
        -b rate     Baud rate (default 57600)
        -m mode     Serial port mode (default 8e1)
        -r filename Read flash to file (or - stdout)
        -w filename Write flash from file (or - stdout)
        -C      Compute CRC of flash content
        -u      Disable the flash write-protection
        -j      Enable the flash read-protection
        -k      Disable the flash read-protection
        -o      Erase only
        -e n        Only erase n pages before writing the flash
        -v      Verify writes
        -n count    Retry failed writes up to count times (default 10)
        -g address  Start execution at specified address (0 = flash start)
        -S address[:length] Specify start address and optionally length for
                            read/write/erase operations
        -F RX_length[:TX_length]  Specify the max length of RX and TX frame
        -s start_page   Flash at specified page (0 = flash start)
        -f      Force binary parser
        -h      Show this help
        -c      Resume the connection (don't send initial INIT)
                *Baud rate must be kept the same as the first init*
                This is useful if the reset fails
        -i GPIO_string  GPIO sequence to enter/exit bootloader mode
                GPIO_string=[entry_seq][:[exit_seq]]
                sequence=[-]n[,sequence]
        -R      Reset device at exit.
    
    Examples:
        Get device information:
            stm32flash /dev/ttyS0
          or:
            stm32flash /dev/i2c-0
    
        Write with verify and then start execution:
            stm32flash -w filename -v -g 0x0 /dev/ttyS0
    
        Read flash to file:
            stm32flash -r filename /dev/ttyS0
    
        Read 100 bytes of flash from 0x1000 to stdout:
            stm32flash -r - -S 0x1000:100 /dev/ttyS0
    
        Start execution:
            stm32flash -g 0x0 /dev/ttyS0
    
        GPIO sequence:
        - entry sequence: GPIO_3=low, GPIO_2=low, GPIO_2=high
        - exit sequence: GPIO_3=high, GPIO_2=low, GPIO_2=high
            stm32flash -R -i -3,-2,2:3,-2,2 /dev/ttyS0
# Bug reports and patches

Please file bugs and post patches in the official tracker https://sourceforge.net/p/stm32flash/tickets/

# Help and hints

Find help on our [[Hints]](https://sourceforge.net/p/stm32flash/wiki/Hints/) page about

Linux and CH340 USB-serial adapters
MacOSX and PL2303 USB-serial adapters

# Building from source

On most platforms just run "make" in the source folder. For building on Windows see also [[Build\]](https://sourceforge.net/p/stm32flash/wiki/Build/).

# Authors

stm32flash was originally written by Geoffrey McRae and is since 2012 maintained by Tormod Volden. See the git log for the many other contributors.

Related

[Wiki: Build](https://sourceforge.net/p/stm32flash/wiki/Build/)
[Wiki: Hints](https://sourceforge.net/p/stm32flash/wiki/Hints/)


