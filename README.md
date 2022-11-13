# AVR USART
Implements bidirectional serial communication via USART, and allows the data packages to be stored internally into non-volatile memory such as EEPROM. 
It supports all the basic AVR microcontrollers, but is mainly designed to work with **Atmega128** and **Atmega328/P**.

### References
  * [Atmega328P Datasheet](https://www.sparkfun.com/datasheets/Components/SMD/ATMega328.pdf)
  * [AVR GCC Libraries](https://www.nongnu.org/avr-libc/user-manual/)
  * [Using Arduino Boards in Atmel Studio](http://www.microdigitaled.com/AVR/Hardware/Arduino/UsingArduinoBoardsInAtmelStudio.pdf)

## Requirements
#### Board
* Interrupts 
* USART Interface
* EEPROM

#### Softwares and libraries
* [Atmel Studio 7.0](https://www.microchip.com/mplab/avr-support/atmel-studio-7)
* [avrdude](https://www.nongnu.org/avrdude/)
* [realterm](https://sourceforge.net/projects/realterm/)


## Configuration
Make sure to update the following data in order to make the program work with the other boards.
```cpp
#define F_CPU           16000000    // clock speed
#define USART_BAUDRATE  9600        // baudrate
#define RX_BUFFER_SIZE  512         // buffer size
#define RX_LINE_SIZE    128         // word size

UCSR0C|=(1<<UCSZ01)|(1<<UCSZ01);  // no parity, 1 stop bit, 8-bit data
```


## Build
#### On Windows
* Clone repository
* Open Atmel Studio 7.0
* New Atmel project
* Import main.c
* Build project
* Launch from project path
  ```
  avrdude -C "/path/to/avrdude.conf" -p atmega328p -c arduino -P {PORT} \
  -b 115200 -U flash:w:"$(ProjectDir)Debug\$(TargetName).hex":i 
  ```
* Open and configure **realterm**
* Start communication

#### On Linux
```bash
# install required packages
$ sudo apt install avrdude gcc-avr gcc gcc-multilib g++-multilib avr-libc gcc-avr

# open Makefile and configure data
$ nano Makefile

# build and flash
$ git clone https://github.com/fhivemind/serial-atmega.git
$ make
$ make flash
```
---

## Demo
Once the program has been successfully flashed onto the controller, and the serial port opened, we can initiate the communication. When opening the realterm (or any other serial monitor), after properly configuring it first, you should see following:
```bash
Commands: 
 '/save' - save all results sent via UART to EEPROM
 '/load' - load saved results from EEPROM
 '/all' - show buffer data 
 '{DATA}' - send data
 ```
 #### Tests
 We will send following data in packages (`\n` indicates end of transmission): 
 
`hello world\n`, `Oh, hi Mark\n`, `all\n`, `save\n`, `load\n`
 
And obtain the result as:
 ```bash
 # input 1
-> input: hello world
 # input 2
-> input: Oh, hi Mark
 # command '/all'
-> all results: 
-> hello world
-> Oh, hi Mark
 # command '/save'
-> Data saved to EEPROM.
-> ----- DATA -----
-> hello world
-> Oh, hi Mark
-> ----------------
 # command '/load'
-> Data loaded from EEPROM.
-> ----- DATA -----
-> hello world
-> Oh, hi Mark
-> ----------------
```

***
Author: [Ramiz Polic](https://github.com/fhivemind)
