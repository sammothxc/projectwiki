---
title: Toughbook: Touchscreen Calibration
description: 
published: true
date: 2026-01-19T05:34:04.079Z
tags: 
editor: markdown
dateCreated: 2026-01-19T05:34:04.079Z
---

# Panasonic Toughbook: Touchscreen Calibration

Tested on CF-18 and CF-29.

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
