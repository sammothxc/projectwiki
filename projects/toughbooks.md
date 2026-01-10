---
title: Panasonic Toughbooks
description: 
published: true
date: 2026-01-10T08:02:55.156Z
tags: 
editor: markdown
dateCreated: 2026-01-10T06:34:56.726Z
---

# FAQ for CF-29 and CF-18

Common issues with the CF-29 and CF-18, solved on BunsenLabs Linux Boron.

## Slow Trackpad?

Run:

```shell
xinput --list --short
```

Find your mouse, and note `id=<device_number>`. Then try these settings:

```shell
xinput --set-prop <device_number_here> "libinput Accel Speed" 0.75
xinput --set-prop <device_number_here> "Coordinate Transformation Matrix" 0.8 0 0 0 0.8 0 0 0 0.2
```

These seem to work well on the CF-29
Adjusting the very last number seems to make the biggest difference, and the other two non-zero numbers help dial it in.

> Many thanks to [@eightysixed](https://forums.bunsenlabs.org/viewtopic.php?id=7527) for the help!

## Touchscreen Calibration

Run:

```shell
sudo apt install xinput-calibrator
```

Then:

```shell
xinput_calibrator
```

It'll have you touch some points on the screen to calibrate, then it will print something to the terminal like:

```shell
Section "InputClass"
	Identifier	"calibration"
	MatchProduct	"LBPS/2 Fujitsu Lifebook TouchScreen"
	Option	"MinX"	"4395"
	Option	"MaxX"	"60116"
	Option	"MinY"	"7175"
	Option	"MaxY"	"58659"
	Option	"SwapXY"	"0"
	Option	"InvertX"	"0"
	Option	"InvertY"	"0"
EndSection
```

Copy and then past this into the file `/etc/X11/xorg.conf.d/99-calibration.conf`.

