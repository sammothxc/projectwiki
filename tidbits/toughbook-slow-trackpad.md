---
title: Toughbook: Slow Trackpad
description: 
published: true
date: 2026-01-19T05:31:41.629Z
tags: 
editor: markdown
dateCreated: 2026-01-19T05:31:41.629Z
---

# Panasonic Toughbook: Slow Trackpad

Tested on CF-18 and CF-29.

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