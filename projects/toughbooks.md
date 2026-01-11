---
title: Panasonic Toughbooks
description: 
published: true
date: 2026-01-11T03:52:24.430Z
tags: 
editor: markdown
dateCreated: 2026-01-10T06:34:56.726Z
---

# Toughbooks CF-29 and CF-18

Common issues with the CF-29 and CF-18, solved on BunsenLabs Linux Boron.

## Slow Trackpad?

Run:

```shell
sudo apt remove xserver-xorg-input-libinput
sudo apt install xserver-xorg-input-evdev xinput-calibrator
```

And reboot. Then,

```shell
xinput --list --short
```

Find your mouse, and note `id=<device_number>`. Then try these settings:

```shell
xinput --set-prop <device_number_here> "Device Accel Profile" 0.75
xinput --set-prop <device_number_here> "Coordinate Transformation Matrix" 0.8 0 0 0 0.8 0 0 0 0.2
```

These seem to work well on the CF-29.
Adjusting the very last number seems to make the biggest difference, and the other two non-zero numbers help dial it in.

> Many thanks to [@eightysixed](https://forums.bunsenlabs.org/viewtopic.php?id=7527) for his info!

## Touchscreen Calibration

Run:

```shell
sudo apt remove xserver-xorg-input-libinput
sudo apt install xserver-xorg-input-evdev xinput-calibrator
```

And reboot. Then,

```shell
sudo xinput_calibrator
```

It'll have you touch some points on the screen to calibrate, then it will print something to the terminal like:

```shell
Section "InputClass"
	Identifier		"calibration"
	MatchProduct	"LBPS/2 Fujitsu Lifebook TouchScreen"
	Option	"Calibration"	"335 3975 295 3758"
	Option	"SwapAxes"		"0"

EndSection
```

**Copy the snippet it tells you to copy** and then paste it into the file `/etc/X11/xorg.conf.d/99-calibration.conf` AND/OR `/usr/share/X11/xorg.conf.d/99-calibration.conf` (for whatever reason the CF-18 and CF-29 require that file be in both locations to calibrate properly on BunsenLabs Linux).

> All credit goes to [Chris Keller](https://askubuntu.com/questions/1188624/trouble-with-xinput-calibrator-on-a-toughbook-cf-19)!
