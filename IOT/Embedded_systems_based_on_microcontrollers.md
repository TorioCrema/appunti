# Microcontrollers

## Microcontroller elements
- CPU
- Memory Unit
- Ports for general purpose Input/Output (GPIO)
- Analog/digital converters
- Timers
- Bus for serial comunication
- Clock
- Power circuit

## Arduino architecture

- Main components
	- MCU ATMega328P
		- 8 bit, 16 MHz
		- Flash memory: 32 KB
		- SRAM memory: 2 KB, EEPROM: 1 KB
	- 14 pins for digital input/output
		- 6 can be used as PWM outputs
	- 6 analog inputs
	- USB connector
	- power jack
	- ICSP header
	- reset button
- Other specs:
	- Operating voltage: 5V
	- Input voltage (recom.): 7-12V
	- DC current per I/O pin: 40 mA
	- DC current per 3.3V: 50 mA

### MCU ATMega328P

- AVR CPU 8 bits, 16 MHz
	- 32 registers
- memory - 16 bits
	- Flash memory: 32 KB,
		- 0.5 used by the bootloader
	- SRAM memory: 2 KB
	- EEPROM: 1 KB

## MCU programming

MCU programming is done by using an external host system (e.g. a PC), where programs are edited, compiled, creating the executable (or binary).
The executable is then uploaded to the MCU by means of specifc HW, or directly by means of the serial communication with the host PC.
The MCU software stack does not include any *operating system*, the executable is directly copied into MCU memory and executed by the CPU.

### In the Arduino case

Arduino programs are typically developed on a host PC and uploaded by means of USB cable, besides, in ICSP (In-Circuit Serial Programmer) interface is available for uploading the executable using a specific external programmer divice.
The upload via USB is done by means of a bootloader, a small program (0.5KiB) pre-stored (via ICSP).
Programming environment/IDE: Wiring e Arduino IDE:
- Wiring is an open-source C/C++ framework, available for different kinds of microcontrollers and SoC as well
- Arduino IDE is a simple but effective IDE for editing sources, compiling and uploading binaries
- based on GNU tools and other open source projects: avr-gcc and open-source libraries

## CPU

The CPU behaviour follows the Von Neumann machine model (fetch-decode-execute cycle).

### Havard achitecture

It's a variant of the Von Neumann architecture in which instructions and data are stored in physically different/separated memories, using distinct buses:
- the name comes from Havard Mark I relay-based computer, in which instructions were stored in punch cards and data in electro-mechanical devices.
- typically used in microcontrollers and DSP, with extensions and variants

Relevant aspects:
- the two memories have typically very different properties, for instance:
	- code in FLASH memory with fast read access, slow write access
- data in SRAM
	- fast read & write access, bigger power consumption & cost

### CPU Architecture

The CPU executes program instructions encoded in machine language.
Characterised by its own Instruction Set Architecture (ISA)
- set of possible instructions recognised by the CPU
- set of registers
	- general or programmable registers (GPR)
	- special purpose registers (SFR)
		- Program counter (PC)
		- Program Status Word (PS)

#### AVR8

AVR8 is the microprocessor inside the ATMega328P microcontroller:
- RISC architecture - code: 8 bits, data: 16 bits - 16MHz

CPU registers
- 32 general- purpose 8 bits registers (R0-R31)
	- all arithmetic and boolean operations involve these registers
	- only load and store operations access the RAM memory
- small set of special-purpose registers:
	- PC: 16 or 22 bit program counter
	- SP: 8 or 16 bit stack pointer
	- SREG: 8 bit status register

### Performance

Microprocessor clock frequency: 16 MHz - period: 0.0625 $\micro$s = 62.5 ns
Most of the instructions are executed using 1 o 2 clock cycles, i.e. ~60ns, ~125ns
This information allows to make accurate predictions about the execution time of programs or parts of them, in particular in the worst case

## Basic control architecture

### Super Loop

The super-loop is the simplest control architecture (a framework) adopted for microcontroller programming. It's very common and widespread since it does not require any specific HW support (such as interrupts).
Features:
- initialisation
- infinite loop, repeatedly executing some task

```c
#include "X.h"
void main(void) {
	X_init(); //Prepare for task X
	while (1) { // for ever 'super-loop'
		X(); //Perform the task
	}
}
```

Pros:
- simplicity (no OS required to work)
- potability
- reliability and safety
- efficiency

Cons:
- timing not accurate
- fragility and low flexibility
- the basic version: 'full-power' $\implies$ high power consuption

#### In Wiring

Reference language: C/C++
super-loop control architecture:
- `setup()` procedure and `loop()` procedure (must be implemented by the programmer)
- there is a hidden `main()` calling these functions

Also provides a set of basic API
- Classes, procedures and functions in C/C++

Source compilation and executable creation by means of a C/C++ compiler/linker: avr-gcc (GNU)

## GPIO

Every microcontroller features a set of pins that can be directly used to drive input/output
Pins are typically *general-purpose*: can be programmed to work as input or output depending on the need.
Pins con be either analog ar digital
- a digital pin can assume two values only, HIGH or LOW (1 or 0)
- an anolog pin can assume any value in a specific range ($0\dots5$ volts)

### Arduino GPIO

14 digital pins, configurable either as input or output
6 analog pins, input only
Some pins are multiplexed, i.e. serving multiple functions. (pin 0 and pin 1 are also used as serial port)

### Operating Parameters

Reference operating parameters for pins:
- voltage (in Volt)
	- to be applied on the pin (input case) or produced by the pin (output case)
	- Arduino 5V, 3.3V ESP
- current (in Ampere)
	- that can receive (input) or that can be produced (output)
	- Arduino 40 mA, ESP8266 12mA

Either presence or absence of *pull-up circuits*, that allwo to fix the voltage at the max value (referred as +VCC) even when the pin - configured as input - is not connected to any circuit.

### Ports

The MCU interacts with the pins (and then with external divices) by means of ports, as for the pins, a port can be configured to be either input or output.
A port is managed by one or multiple special purpose registers (SRF) connected to the pin that carries the digital or analog signal, sent or recieved by the external device. The register keeps the state (current value) and the current configuration (input/output).
One port can manage more than one pin.

#### ATMega328P Ports

The ATMega328P has 23 I/O lines grouped by 3 8-bit ports named B, C and D.
Each pin is identified by a label such as PD0, PC1, PB2.
Each port is managed by $3$ registers: DDRx, PORTx, PINDx, where the x is the port name.
- DDRx
	this register is both Read/Write and stores the configured **direction** for the pins (bit set to 0 $\implies$ pin configured as INPUT; bit set to 1 $\implies$ pin configured as OUTPUT)
- PORTx
	this register is both Read/Write and stores the pin **state**, which changes depending on the pin configuration:
	- for INPUT pins a bit set to 1 activates the PULL-UP resistor, while a bit set to 0 deactivates it;
	- for OUTPUT pins, a bit set to 1 represents a state set to HIGH on the corresponding pin, while a bit set to 0 represents a state set to LOW
- PINx
	- this register is read-only, and stores, for INPUT pins, the signal read on the pin: 1 if the signal value is HIGH; 0 if the signal value is LOW.

To configure an OUTPUT pin to the HIGH value, we must:
- write on the DDRx register to set te direction (OUTPUT)
- write on the PORTx register to set the state (HIGH)

In the ATMega318 MCU I/O registers are mapped into the memory, so that they can be uniformly accessed/manipulated by writing/reading bytes/words at the specific memory addresses.

## Pins Wiring API

```c
pinMode(pin, mode) // sets the pin direction
digitalWrite(pin, value) // sets the value of an OUTPUT pin
digitalRead(pin) // reads the value of an INPUT pin
delay(millis) // busy waiting for the amount of time
millis() // called by delay, returns the number of milliseconds since the power on of the device
```

## PWM

Pulse-width modulation is a method used to emulate an analog output signal on digital pins, through the generation of a periodic square wave.
The output voltage emulated is defined by the duty cycle of the signal (percentage of time in which the signal is HIGH over the time in which the signal is LOW).
The average voltage value is equal to the duty-cycle $\times$ VCC (a value in $0\dots$VCC) of e.g. 50% duty cycle $\implies$ $0.5 \times$VCC $= 2.5V$ for VCC$=5V$
Such an emulation approch doesn't work with every output analog device/actuator (it works on LEDS, direct current motors).
In Arduino pins 3, 5, 6, 9, 10 and 11 can be used for PWM output (8 bit) through the `analogWrite()` function.

## Analog signals

A digital signal on a (digital) pin can assume only two logical values: HIGH (VCC) or LOW (GND), that is 1 or 0.
An analog signal on an (analog) pin can assume any value in the voltage range GND-VCC (0-5 V).
So in order to process an analog signal with a digital computer it must first be converted.
This conversion is made by an electronic component called ADC (Analog-to-Digital Converter), that maps a voltage continuous value in GND-VCC into a discrete value in a certain range (e.g. 0-1023), depending on the number of bits (resolution) used by the ADC. The resolution defines the precision of the ADC.
Arduino can manage up to 6 input analog signals by means of 6 analog pins, each with a 10-bit ADC.
`analogRead(pin)` is the Wiring function to read an analog signal with the ADC.
`map(value, in_low, in_high, out_low, out_high)` maps the value in the range `in_low-in_high` to the range `out_low-out_high`.

## Interrupts

Interrupts are the fundamental mechanism used by the microcontroller to react to events triggered by external devices, including sensors perceiving the physical world, timers.
Generally speaking, a CPU provides one or multiple pins - called IRQ (Interrupt Request) - to receive interrupt signals.
- when an interrupt request is received, the CPU suspends the execution of current instructions (e.g. the super-loop) it saves on a stack the address of the next instruction to be executed, and jumps to the routine (procedure) configured to react to the event - called interrupt handler or interrupt service routine (ISR).
- the ISR address is stored into a table called interrupt table or interrupt vector

In microcontrollers, IRQs are typically connected to one or multilpe GPIOs.

The ATMega328P has 2 pins - pin 2 and pin 3 - that can be configured to generate an interrupt on different conditions:
- when there is a RISING or FALLING edge
- when there is a CHANGE
- when the signal is LOW

In Wiring it is possible to directly specify and attach in ISR by means of the routine: `attachInterrupt(intNum, ISR, mode)`:
- `intNum`: interrupt number (int)
- `ISR`: pointer to the interrupt handler (`void (*func)(void)`)
- `mode`: the condition that raises the interrupt `CHANGE/FALLING/RISING/LOW/HIGH`

`digitalPinToInterrupt(numPin)` makes it possible to get the number of the interrupt given the interrupt pin.

To disable interrupts: `noInterrupts()`, to resume them: `interrupts()`

Inturrupt handlers should be short, they cannot block or execute infinite loops, we want low interrupt latency (amount of time used to react to an interrupt) to minimise the time period in which interrupts are disabled.

## Timers

Used to:
- measure time intervals
- create PWM signals
- create timeouts and alarms
- create multi-tasking based on preempive scheduling

It can be represented as a simple counter which is incremented at the HW level at some frequency.

The ATMega328P has 3 internal timers:
- timer0
- timer1
- timer2

Each timer has a counter which is incremented at each clock cycle (of the timer)
- timer0 and timer2 have 8-bit counters
- timer1 has a 16-bit counter

CTC (Clear Timer on Compare Match): in this mode timer interrupts are generated when the counter reaches a certain value, stored in a register called compare mach register. When the timer reaches this value, the counter is reset and starts counting again. By choosing the value of the CTC register, we specify interrupts frequency.

Counters are updated at 16MHz so each tick occurs every 1/16,000,000 of a second (~63ns).
timer0 and timer2 are 8-bit $\implies$ they take $256\times 63 = 16128 ns = 16.1 \micro s$ to go in overflow (and restart at $0$)
timer1 is 16-bit $\implies$ it takes $65536\times 63 = 4128768 ns \approx 4129 \micro s\approx 4.1ms$ 
This frequency can be modulated by specifying a prescaler value, which essentially funcitons as a divider of the original frequency (16MHz):
$$\text{timer speed (Hz)} = (\text{Arduino clock speed (16MHz)}) / \text{prescaler}$$
If the prescaler is $1$ the counter increments at $16$MHz, if the prescaler is $8$ the counter increments at $2$MHz and so on.
The value of the prescaler can be $1$, $8$, $64$, $256$ and $1024$.
Given the prescaler, then the interrupt generation frequency is:
$$\text{desired interrupt freq (Hz)}=\frac{16,000,000Hz}{(\text{prescaler}\times(\text{compare match register + 1}))}$$
the $+1$ is there because te value $0$ in the CTC is the first significant value
Then solving with the CTC:
$$\text{compare match register}= \frac{16,000,000Hz}{\text{prescaler}\times\text{desired interrupt freq}}-1$$
we then check if this value is less than $256$ if so we use timer0 and timer2, if it's less than $65536$ we use timer1. If both of these conditions aren't met we have to increase the prescaler.

In Wiring for Arduino timers are used to realise PWM output signals:
- timer0 is used for pins 5 and 6
- timer1 is used for pins 9 and 10
- timer2 is used for pins 11 and 3

### Watchdog

The watchdog timer is a timer that counts until some specified value, after which a reset signal is produced to reboot the system
- in normal functioning, the watchdog periodically receives a signal before reaching the threshold, resetting the counter
- if no signal is received on time, then it means that the microprocessor encountered a problem and is is reset.

## Power Management

A very important functionality supported by most microcontrollers is power management, with the possibility to exploit low power consuption mode (sleep mode).
ATMega328P Example:
- 5 sleep modes (with increasing power reduction):
	- Idle mode
	- ADC Noise Reduction Mode
	- Power-save mode
	- Standby mode
	- Power-down mode

- Idle mode:
	Stops the CPU but allows the SPI, USART, Analog Comparator, ADC, Two-wire Serial Interface, Timer/Counters, Watchdog, and the interrupt system to continue operating. This sleep mode basically halts clkCPU adn clkFLASH, while allowing the other clocks to run.
- ADC Noise Reduction Mode
	Stops the CPU but allows the ADC, the external interrupts, the Two-wire Serial Interface address watch, Timer/Counter2 and the Watchdog to continue operating (if enabled). This sleep mode basically halts clkI/O, clkCPU, and clkFLASH, while allowing the other clocks to run.
- Power-save Mode:
	This mode is identical to Power-down, with one exception: if Timer/Counter2 is clocked asynchronously, Timer/Counter2 will run during sleep.
- Standby mode
	This mode is identical to Power-down with the exception that the Oscillator is kept running. From Standby mode, the device wakes up in 6 clock cycles.
- Power-down mode:
	The External Oscillator is stopped, while the external interrupts, the Two-wire Serial Interface address watch, and the Watchdog continue operating (if enabled). Only an External Reset, a external level interrupt on INT0 or INT1, can wake up the MCU. This sleep mode basically halts all generated clocks, allowing operation of asynchronous modules only.
	When waking up from Power-down mode, there is a delay between the moment the wake-up contion occurs and when it becomes effective. This allows the clock to restart and become stable after having been stopped.

## Buses and protocols

Protocols to exchange data through single pins can become complex depending on the external communicating I/O divice.
For this purpose, specific communication interfaces, buses and protocols have been introduced.
Two main kinds: **serial** and **parallel**:
- parallel interfaces allow to transfer multiple bits at the same time, by means of multiple lines (8, 16, 32 bit bus)
- serial interfaces send and receive streams of bits in a sequential way, using a single line/channel.

### Serial interfaces

Two main categories:
- asynchronous:
	- no clock line is used for synchronisation
	- two lines: TX to transmit, RX to receive
	- examples: USB, RS-232 and TTL
	- oldest and simpler protocol
- synchronous:
	- a clock signal is used to synchronise the communication among the parts, this allows to achieve higher speed than asynchronous serial
	- examples: I2C and SPI synchronous serial buses

#### Asynchronous serial

In an asynchronous serial interface, since there is not a specific clock line, the synchronisation is achieved through a protocol, to ensure an error-free and robust bit transmission.
The protocol can be configured by specifying some parameters, that must be adopted by both communicating parts:
- baud rate
- data frame
- synchronous bits
- parity bit

##### Baud rate

Transmission speed in bits per second
A typical value used is 9600bps. Other standard values are 1200, 2400, 4800, 19200, 38400, 57600 and 115200.
The higher the baud rate, the higher the speed, however there is a limit (typically 115200), related to microcontrollers capabilities, using a higher value could lead to transmission errors.

##### Data frame

Each data block - typically one byte - is transmitted inside a (bit) packet called frame. These frames are created by adding a synchronisation bit and a parity bit to the data block.
The data chunk (block) represents the real data transmitted: each packet can have a data chunk from 5 to 9 bits (usually 8).
Part of the protocol includes agreeing on the *endianess* of data, i.e. the order in which to send the bits. Little endian by default.
Synchronisation bits include a start bit and a stop bit, to mark the beginning and the end of the packet.

##### UART

A main component of a serial system is UART (universal asynchronous receiver/transmitter), that is the circuit block that manages the data to and from the serial interface.
It functions as a converter between the parallel interface (data bus) and the serial interface.
On one side of the UART there is a 8 bit (or more) data bus (plus some control pins), on the other side are the 2 serial lines.
UARTs exist also as stand-alone integrated circuits, however typically they are embedded inside microcontrollers.

##### On Arduino

Arduino UNO has one serial port hardware TTL, mutiplexed on GPIO 0 and 1.
- pin 0 (RX) is used to receive, pin 1 (TX) is used to transmit
- these pins are connected to the corresponding ATMega8U2 Usb-to-TTL-serial chip pins (this converts a serial TTL into a USB and viceversa)
- these pins are used for more than one function
	- serial and GPIO
	- they are mutually exclusive
- Serial library/class in Wiring
	- full access and control of the (async) serial
	- can be used to send and receive messages and raw data to the system connected via USB

#### Synchronous serial

Synchronous serial interfaces use a clock signal to synchronise one or multiple lines to send and/or receive data. They are more complex than asynchronous serial lines, but usually function at higher speeds. Some examples are SPI and I2C.

##### I2C

I$^2$C is a standard bus/protocol
- good speed, minimising the amount of pins required: only 2
- introduced by Philips in the '80s
- also called "Two-wire"

Specs:
- 7-bit or 10-bit bus
- bus speed of 100kbit/s in standard mode and 10kbit/s in low-speed mode
- newer versions are capable of 400kbit/s on fast-mode, 1Mbit/s on fast-mode plus and 3.4Mbit/s on High-Speed mode

It uses a master-slave architecture:
- the bus is controlled by a master device, usually the microcontroller
- it includes one or multiple slave devices that receive info from the master
- All devices share 2 lines:
	- a signal clock (Serial Clock Line, SCL), necessary to synchronise the communication
	- a bidirectional data line (Serial Data Line, SDA) to send and receive data.

The communication is always initiated by the master and slaves can only respond.
Commands sent by the master on the bus are received by all slaves
- each slave has its own 7 bit unique ID, which is sent by the master when it starts the communication
- only the target slave reacts to the message sent by the master
A typical communication schema:
- master sends the start bit
- master sends the 7-bit id of the slave device
- master sends read (0) or write (1) bit, depeding on whether it wants to write data to the divice registers or read from them
- the slave responds with an ack
- in write mode, the master sends data as a sequence of bytes and the slave replies with ACKs. In read mode, the master receives a stream of bytes, sending an ACK after each byte received
- when the communication is finished the master sends a STOP bit.

Arduino/ATMega328P has native support for I2C on the A4 and A5 pins

##### SPI

Originally introduced by Motorola, the SPI bus implements a full-duplex (both devices can send data at the same time) serial protocol that allows for a bidirectional communication between the master and the slaves.
It's a serial protocol, based on the master-slave schema, but it uses different lines for transmitting and receiving data, and a specific line to select the slave to communicate with.
It is not an official formal standard.
3 lines used to communicate:
- a shared clock signal (Shared Serial Clock, SCK) used to coordinate the communication
- a shared Master Out Slave In (MOSI) line used to send data from the master to the slave
- a shared Master In Slave Out (MISO) line used to send data from the slave to the master
- a shared Slave Select (SS) line used to select the slave

Arduino/ATMega328P has native support for SPI:
- pin 10 for SS line
- pin 11 for MOSI line
- pin 12 for MISO line
- pin 13 for SCK