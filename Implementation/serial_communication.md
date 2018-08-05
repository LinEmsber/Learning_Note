# Serial communication

# Introduction
  * Rules of Serial:
The asynchronous serial protocol has a number of built-in rules - mechanisms that help ensure robust
and error-free data transfers.These mechanisms, which we get for eschewing the external clock signal, are Data bits, Synchronization bits, Parity bits, and Baud rate.
  * Baud Rate:
The baud rate specifies how fast data is sent over a serial line. It’s usually expressed in units of
bits-per-second (bps).
  * Framing the data:
Each block (usually a byte) of data transmitted is actually sent in a packet or frame of bits. Frames are
created by appending synchronization and parity bits to our data.
  * Data chunk:
The real meat of every serial packet is the data it carries. We ambiguously call this block of data a chunk.
Certainly, the standard data size is your basic 8-bit byte, but other sizes have their uses. A 7-bit data
chunk can be more efficient than 8, especially if you’re just transferring 7-bit ASCII characters.

After agreeing on a character-length, both serial devices also have to agree on the endianness of their data.

  * Synchronization bits:
The synchronization bits are two or three special bits transferred with each chunk of data. They are the
start bit and the stop bit(s). These bits mark the beginning and end of a packet.

  * Parity bits:
Parity is a form of very simple, low-level error checking. It comes in two flavors: odd or even. To produce
the parity bit, all 5-9 bits of the data byte are added up, and the evenness of the sum decides whether the
bit is set or not.

For example, assuming parity is set to even and was being added to a data byte like 0b01011101, which has an
odd number of 1’s (5), the parity bit would be set to 1. Conversely, if the parity mode was set to odd, the
parity bit would be 0.


## An example, 9600 8N1:

 * 9600 8N1 - 9600 baud, 8 data bits, no parity, and 1 stop bit - is one of the more commonly used serial protocols.

 * A device transmitting the ASCII characters "O" and "K" would have to create two packets of data. The ASCII
value of O is 79, which breaks down into an 8-bit binary value of 01001111, while K’s binary value is 01001011.
All that’s left is appending sync bits.

 * It’s assumed that data is transferred least-significant bit first. Notice how each of the two bytes is sent as
it reads from right-to-left.

```shell=
        0111100101 0110100101
        |        | |        |
        V        | V        |
     start bit   | s        |
                 V          V
              end bit       e
```
  * For every byte of data transmitted, there are actually 10 bits being sent: a start bit, 8 data bits, and a
stop bit. So, at 9600 bps, we’re actually sending 9600 bits per second or 960 (9600/10) bytes per second.


## References:
 * [rules-of-serial](https://learn.sparkfun.com/tutorials/serial-communication/rules-of-serial)
