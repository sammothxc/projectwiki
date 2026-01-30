---
title: Vivado 'libtinfo5' Missing
description: 
published: true
date: 2026-01-30T05:29:40.309Z
tags: 
editor: markdown
dateCreated: 2026-01-30T05:29:40.309Z
---

# Vivado 2024.1: 'libtinfo5' Missing

This error can be fixed by downloading and installing [libtinfo5](http://ftp.us.debian.org/debian/pool/main/n/ncurses/libtinfo5_6.4-4_amd64.deb) and [libncurses5](http://ftp.us.debian.org/debian/pool/main/n/ncurses/libncurses5_6.4-4_amd64.deb).

Use this startup script for Vivado:

```bash
#!/bin/bash

VIVADO_DIR=/path/to/AMD/2025.1/Vivado

if [ -z "$LD_LIBRARY_PATH" ]
then
    LD_LIBRARY_PATH=$VIVADO_DIR/lib/lnx64.o
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIVADO_DIR/lib/lnx64.o/Ubuntu
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIVADO_DIR/lib/lnx64.o/Ubuntu/24
    export LD_LIBRARY_PATH
else
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIVADO_DIR/lib/lnx64.o
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIVADO_DIR/lib/lnx64.o/Ubuntu
    LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VIVADO_DIR/lib/lnx64.o/Ubuntu/24
    export LD_LIBRARY_PATH
fi

# Source AMD Vivado settings
source $VIVADO_DIR/settings64.sh

# Start AMD Vivado
$VIVADO_DIR/bin/vivado
```

> Credit to [Krotti83](https://www.reddit.com/r/debian/comments/1lsmflv/debian_13_trixie_libtinfo5_and_libncurses5_for/)