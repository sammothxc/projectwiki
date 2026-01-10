---
title: Toughbooks
description: 
published: true
date: 2026-01-10T06:34:56.726Z
tags: 
editor: markdown
dateCreated: 2026-01-10T06:34:56.726Z
---

# FAQ for CF-29 and CF-18

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

