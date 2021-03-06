# Interrupt request

## Introduction
In a computer, an interrupt request (or IRQ) is a hardware signal sent to the processor that temporarily stops a running program and allows a special program, an interrupt handler, to run instead. Hardware interrupts are used to handle events such as receiving data from a modem or network card, key presses, or mouse movements.

## Overview
When working with personal computer hardware, installing and removing devices, the system relies on interrupt requests. There are default settings that are configured in the system BIOS and recognized by the operating system. These default settings can be altered by advanced users. Modern plug and play technology has not only reduced the need for concern for these settings, but has also virtually eliminated manual configuration.

## Linux kernel

An IRQ is an interrupt request from a device.
Currently they can come in over a pin, or over a packet.
Several devices may be connected to the same pin thus sharing an IRQ.

An IRQ number is a kernel identifier used to talk about a hardware interrupt source. Typically this is an index into the global irq_desc array, but except for what linux/interrupt.h implements the details are architecture specific.

An IRQ number is an enumeration of the possible interrupt sources on a machine. Typically what is enumerated is the number of input pins on all of the interrupt controller in the system. In the case of ISA what is enumerated are the 16 input pins on the two i8259 interrupt controllers.

## References
 - [Wiki Interrupt_request](https://en.wikipedia.org/wiki/Interrupt_request_(PC_architecture))
 - [Linux Doc, IRQ](http://lxr.free-electrons.com/source/Documentation/IRQ.txt)
 - [Linux interrupt.h](http://lxr.free-electrons.com/source/include/linux/interrupt.h)
