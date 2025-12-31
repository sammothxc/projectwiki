---
title: Compaq Portable I Mechanical Keyboard
description: 
published: true
date: 2025-12-31T18:43:05.748Z
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

## Resources

> - https://forum.vcfed.org/index.php?threads/compaq-portable-i-corroded-keyboard.57223/
> - https://github.com/tmk/tmk_core
> - https://www.youtube.com/watch?v=fGLiPh0byCo
