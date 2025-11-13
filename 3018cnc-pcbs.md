---
title: PCBs with 3018 CNC
description: 
published: true
date: 2025-11-13T22:11:02.657Z
tags: 
editor: markdown
dateCreated: 2025-11-08T21:35:02.466Z
---

> 🚧 Still under construction: more coming soon!
{.is-warning}

# Milling Single-sided PCBs with cheap desktop 3018 CNC Machine

In KiCad 9.99:

Track width: 0.5mm
Clearance: 0.5mm

Usual components:
Resistor_THT:R_Axial_DIN0204_L3.6mm_D1.6mm_P7.62mm_Horizontal
Capacitor_THT:CP_Radial_D7.5mm_P2.50mm
Capacitor_THT:C_Rect_L4.6mm_W2.0mm_P2.50mm_MKS02_FKP02
Connector_BarrelJack:BarrelJack_Kycon_KLDX-0202-xC_Horizontal

To export: `File` -> `Plot`
![plot.png](/assets/plot.png)
Then `Plot`
Then `Generate Drill Files...`
![drill-files.png](/assets/drill-files.png)

In Flatcam 8.993 BETA:

Initial settings: `Edit` -> `Preferences` (or <kbd>Shift</kbd> + <kbd>P</kbd>)
Units: set to the same units you used to design the board (probably `mm`)

