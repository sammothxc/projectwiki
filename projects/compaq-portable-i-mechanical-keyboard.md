---
title: Compaq Portable I Mechanical Keyboard
description: 
published: true
date: 2026-01-12T07:22:52.339Z
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

### 

## Resources

> - https://forum.vcfed.org/index.php?threads/compaq-portable-i-corroded-keyboard.57223/
> - https://github.com/tmk/tmk_core
> - https://www.youtube.com/watch?v=fGLiPh0byCo
> - https://github.com/tmk/tmk_keyboard/wiki/IBM-PC-XT-Keyboard-Protocol#keyboard-hard-reset
> - https://github.com/AndersBNielsen/pcxtkbd/blob/master/XT_KEYBOARD.ino
