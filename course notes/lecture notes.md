# Digital Systems Design Using Microcontrollers - Cornell

## Table of contents

- [Week 1 - course introduction](#week1)
    - [RP2040 specs](#rp2040-specs)
- [Week 2 - Hardware/Software Overview](#week2)
    - [Levels of abstraction of between hardware and software](#levels-abs)
    - [RP2040 Hardware](#rp2040-hw)
    - [GPIO ports](#gpio-ports)
    - [C SDK levels of abstraction](#c-sdk-abs)

<a id="week1"></a>

## Week 1 - course introduction

<a id="rp2040-specs"></a>

### RP2040 specs:
__dual ARM Cortex-M0+ cores__

__12 DMA channels (Direct Memory Access)__
- simple co-processors that you can use to move data from one place in memory to another place in memory with no CPU interaction

__2 UARTs__

__2 SPI channels__

__2 I2C channels__

__16 PWM channels__

__USB 1.1 controller__

__30 GPIO pins__

__12-bit ADC with 5-input MUX__

__8 PIO state machines__
- Programmable Input Output state machines
- co-processors that live inside the RP2040 that you can write programs for
- written in custom assembly language called PIO assembly (9 instructions)
- powerful for building custom high-speed comms interfaces from RP2040 to some other device

__8-cycle integer divider__
- hardware in the RP2040 that's been designed to speed up divide operations

__Interpolator__

__264kB on chip SRAM__

__QSPI interface to external flash__

<a id="week2"></a>

## Week 2 - Hardware/Software Overview

<a id="levels-abs"></a>

### Levels of abstraction of between hardware and software

![hw-sw abstraction](./images/hw-sw%20abstraction%20levels.png)

- all things in white box are controlled by manipulating the registers (place in memory)
- there are 1000s of registers, some coupled with others
- you can directly manipulate these registers but it's cumbersome
- the RP2040 C SDK abstracts these register manipulations into an API to make it more manageable
- this course will be working mainly at the SDK level
- protothreads is a lightweight threading library for the RP2040 (not developed by raspberry pi though), that allows you to set up RP2040 programs as concurrent threaded programs

<a id="rp2040-hw"></a>

### RP2040 Hardware

![RP2040 HW](./images/rp2040%20hw.png)

- two processors that share 26 interrupts
- both connected to SIO (single-cycle IO)
    - all the 'stuff' in the SIO, the CPUs can 'talk to' in 1 cycle (much faster than through the BUS). Includes but not limited to:
    - CPUID to keep track of what core
    - FIFO each way (32 bit wide 8 sample deep) used to push and pop data from one core to the other (good way to sync the cores incidentally)
    - 32 hardware spinlocks
        - provides a mechanism for one core to lock out the other core from a shared resource (global variable etc.)
        - spinlocks 0-25 already used (read datasheets)
- 12 DMA channels, each capable of 32 bit transaction per CPU cycle
- USB connection
- SRAM organised in 6 blocks
- on-chip ROM (containing bootloader etc. Won't be used on this course)
- on-board cache that communicates out through the QSPI interface to external flash memory holding our program code
- PIO ports
    - assembly ode can be written to directly manipulate GPIOs
- peripherals:
    - if green, can form a connection from these to the GPIO ports
    - ADC clock runs at 48MHz, a single sample always takes 96 of those, meaning 2 microseconds/sample ADC

<a id="gpio-ports"></a>

### GPIO Ports

![GPIO Ports](./images/GPIO%20ports.png)

- above shows which internal hardware peripherals can be mapped to which GPIO ports
- another pico can be used to debug the pico itself, using ports shown at bottom of image

<a id="c-sdk-abs"></a>

### C SDK levels of abstraction

The C SDK abstracts register manipulations to function calls

![C SDK abstraction](./images/C%20sdk%20levels%20of%20abstraction.png)

- arranged hierarchically
- hardware_regs: 
    - hardware_regs folder in library, has a #define that gives a name for every register
    - associates memory locations to something readable
- hardware_structs:
    - organises all the registers into C structs in such a way as to represent the memory-mapped layout of the RP2040
- hardware support libraries (most of the courses work will be done at this level):
    - provides an API for interacting with each piece of physical hardware
- higher-level libraries:
    - higher level abstractions for combining multiple hardware support library functions in one