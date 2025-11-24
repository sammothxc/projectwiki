---
title: ESP32-S3 Supermini
description: 
published: true
date: 2025-11-24T05:49:24.721Z
tags: 
editor: markdown
dateCreated: 2025-11-24T05:49:24.721Z
---

## Pinout

![esp32s3supermini-pinout.png](/esp32s3supermini-pinout.png)

## PlatformIO

Create the folder `%HOMEPATH%/.platformio/boards` if it doesn't exist already. Make the file `esp32-s3-fh4r2.json` and put [these contents](https://github.com/sivar2311/platformio_boards/blob/main/esp32-s3-fh4r2.json) in it.

Put this in your `platformio.ini`

```
[env:esp32-s3-fh4r2]
platform = espressif32
framework = arduino
board = esp32-s3-fh4r2
```