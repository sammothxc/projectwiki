---
title: Compaq Portable I Mechanical Keyboard
description: 
published: true
date: 2026-01-12T07:53:27.206Z
tags: 
editor: markdown
dateCreated: 2025-12-31T18:32:37.457Z
---

# Compaq Portable Plus Mechanical Keyboard

## Connector

The Compaq Portable uses a proprietary connector that seems to just be a fancy 2x3 right angle pin header.
```
Female connector that plugs into the keyboard, fat side up:

 [123]
 |456|

 1: GND
 2: VCC (+12V)
 3: GND
 4: RST?
 5: DATA
 6: CLK
 ```
 THe pinout also uses +12V for VCC, which is then dropped down by a voltage regulator on the keyboard.
 
 Pinout of standard XT keyboard connector for reference:
![xt-pinout.png](/xt-pinout.png)

## Protocol

There are two versions of the PC/XT protocol: one used by the IBM (and good clones), and a second used by the majority of all other clones.
**The Compaq Portable Plus uses the original IBM version.**

The main difference is that the IBM version has two start bits and the clone version only has one start bit.

### IBM Version

The TMK Keyboard wiki has an excellent writeup on this topic:

> IBM keyboard places 0 on the first falling edge and 1 on the second. The first start bit should be ignored or has no effect in XT host interface. Some of clones like Lynk also emulate this signaling.
>
> Let's reffer to the first one as start(0) and the second as start(1) here. start(0) bit is not actual start bit.
>
> We should read data line 10 times at falling egdes:
>
> `start(0), start(1), bit0, bit1, bit2, bit3, bit4, bit5, bit6, bit7`
> #### Note for start(0)
> The pseudo start(0) bit is comprised of RTS and CTS sequence according to Technichal Refernce.(see below) The sequece is taken place to make sure both lines are active(high).
>
> The start(0) bit has very tight time margin(around 5us) to read data line as Low because IBM XT Keyboard releases data line to Hi immediately after clock line falls without any wait. See waveform of IBM XT keyboard below.
>
> The start(0) happens with just two machine instructions like below and the instructions take only two machine cycles of Intel 8048(2.5us/cycle) each.
> ```nasm
> 0245: 98 BF   ANL BUS,#$BF     ; ok, CLK high: pull it low
> 0247: 88 20   ORL BUS,#$20     ; and release DATA..
> ```
> That 5us may not be enough margin even for modern microcontroller and it can misses the bit without special care. What we can do is to read as fast as possible after the first clock. With AVR with 16MHz ISR elaborately defined with C takes a few micro seconds(aroudn 3us) and ISR_NAEKED with simple assembly code require 700ns.
>
> - Credit to [tmk](https://github.com/tmk/tmk_keyboard/wiki/IBM-PC-XT-Keyboard-Protocol)!

### Clone Version

> They have just one start bit on first falling edge and their clock is fast in comparison with original.
>
> Reading at falling edges:
> `start(1), bit0, bit1, bit2, bit3, bit4, bit5, bit6, bit7`
>
> - Credit to [tmk](https://github.com/tmk/tmk_keyboard/wiki/IBM-PC-XT-Keyboard-Protocol)!

### How to tell the difference

Measure Data and Clock on startup: IBM version should show the Clock high while Data is held low. Clones will just have both high.

### Clock and Data Signals

> The keyboard and system communicate over the 'clock' and 'data' lines. The source of each of these lines is an open-collector device on the keyboard that allows either the keyboard or the system to force a line to an inactive (low) level. When no communication is occurring, the 'clock' line is at an active (high) level. The state of the' data' line is held inactive (low) by the keyboard. 
>
> An inactive signal will have a value of at least 0, but not greater than +0.7 volts. A signal at the inactive level is a logical O. An active signal will have a value of at least +2.4, but not greater than +5.5 volts. A signal at the active level is a logical 1. Voltages are measured between a signal source and the dc network ground. 
>
> The keyboard 'clock' line provides the clocking signals used to clock serial data from the keyboard. If the host system forces the 'clock' line to an inactive level, keyboard transmission is inhibited. 
>
> When the keyboard sends data to the system, it generates the 'clock' signal to time the data. The system can prevent the keyboard from sending data by forcing the 'clock' line to an inactive level, or by holding the 'data' line at an inactive level.
> During the BAT, the keyboard allows the 'clock' and 'data' lines to go to an active level.
>
> - [IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf](http://www.minuszerodegrees.net/manuals/IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf), page 4-32

> The XT interface is very simple.  Bits are shifted into an 74LS322 shift register.  When the start bit reaches the high-order bit of the shift register, it sets an interrupt, disables shifting and pulls data low one shift time later.  In other words, the start bit is shifted out. The bit order is LSB first for 8 data bits.  After that, the interface is essentially blind until the PC has picked up the character.
>
> Many keyboards start out by strobing a low "pseudo-start"  bit, then follow with a high "real start".  Apparently, this results in more stable performace.  We'll do the same thing.
>
> At some point, the XT host will drop the keyboard clock line for 20 mS or more.	 When this happens, we need to simulate a keyboard reset.
>
> Clock is held low by the PC as an inhibit--the keyboard should (and cannot) send data until both data and clock lines have been allowed to go high.
>
> - comment in a file for the [AT2XT keyboard converter](https://gist.github.com/tmk/5604a84f40cefeb5b359a114634db221#file-xtatkey-asm-L466-L479)

### Data Stream

> Data transmissions from the keyboard consist of a 9-bit data stream sent serially over the' data' line. A logical 1 is sent at an active (high) level. The following table shows the functions of the bits.
> Bit 	 Function
> 1 Start bit (always 1)
> 2 Data bit 0 (least-significant)
> 3 Data bit 1
> 4 Data bit 2
> 5 Data bit 3
> 6 Data bit 4
> 7 Data bit 5
> 8 Data bit 6
> 9 Data bit 7 (most-significant)
>
> - [IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf](http://www.minuszerodegrees.net/manuals/IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf), pages 4-33

### Keyboard Data Output

> When the keyboard is ready to send data, it first checks the status of the keyboard 'clock' line. If the line is active (high), the keyboard issues a request-to-send (RTS) by making the 'clock' line inactive (low). The system must respond with a clear-to-send (CTS), generated by allowing the 'data' line to become active, within 250 microseconds after RTS, or data will be stored in the keyboard buffer. After receiving CTS, the keyboard begins sending the 9 serial bits. The leading edge of the first clock pulse will follow CTS by 60 to 120 microseconds. During each clock cycle, the keyboard clock is active for 25 to 50 microseconds. Each data bit is valid from 2.5 microseconds before the leading edge until 2.5 microseconds after the trailing edge of each keyboard clock cycle.
> - [IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf](http://www.minuszerodegrees.net/manuals/IBM_5155_5160_Technical_Reference_6280089_MAR86.pdf), pages 4-33

### Waveform

Typical waveform for the Compaq Portable Plus:
> 
>```
>    ____       _ 1 _ 2 _ 3 _ 4 _ 5 _ 6 _ 7 _ 8 _ 9 _______
>CLK     \_____/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/ \_/   
>             _____ ___ ___ ___ ___ ___ ___ ___ ___
>DAT ________/     \___X___X___X___X___X___X___X___\_______
>        ^   ^  S    0   1   2   3   4   5   6   7
>        RTS CTS
>```
>
> - Credit to [tmk](https://github.com/tmk/tmk_keyboard/wiki/IBM-PC-XT-Keyboard-Protocol)!

![cpp-waveform.png](/cpp-waveform.png)
> https://geekhack.org/index.php?topic=17458.msg350637#msg350637

## Resources

> - https://forum.vcfed.org/index.php?threads/compaq-portable-i-corroded-keyboard.57223/
> - https://github.com/tmk/tmk_core
> - https://www.youtube.com/watch?v=fGLiPh0byCo
> - https://github.com/tmk/tmk_keyboard/wiki/IBM-PC-XT-Keyboard-Protocol#keyboard-hard-reset
> - https://github.com/AndersBNielsen/pcxtkbd/blob/master/XT_KEYBOARD.ino
> - https://web.archive.org/web/20210307022734/http://halicery.com/Hardware/Intel%208042%20and%208048/8048_XT_INTERN.TEXT
