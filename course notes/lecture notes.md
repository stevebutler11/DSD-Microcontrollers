# Digital Systems Design Using Microcontrollers - Cornell

## Table of contents

1.  [Week 1 - course introduction](#week1)
    1.  [RP2040 specs](#rp2040-specs)

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

