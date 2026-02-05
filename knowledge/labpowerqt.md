---
title: Installing LabPowerQT
description: 
published: true
date: 2026-02-05T04:55:27.951Z
tags: kb, linux
editor: markdown
dateCreated: 2026-02-05T04:55:02.416Z
---

# Installing LabPowerQT

Run this on Debian/Ubuntu:

```bash
sudo apt-get install qt5-qmake qtbase5-dev qttools5-dev libqt5serialport5-dev libqt5quick5 qtdeclarative5-dev
cd ~/labpowerqt
mkdir build && cd build
cmake ..
make
sudo cp src/labpowerqt /usr/local/bin/labpowerqt
sudo chmod +x /usr/local/bin/labpowerqt
sudo cp ~/labpowerqt/icons/application32.png /usr/share/pixmaps/labpowerqt.png
cat > /usr/share/applications/labpowerqt.desktop << 'EOF'
[Desktop Entry]
Version=1.0
Type=Application
Name=LabPowerQT
Icon=labpowerqt
Comment=Lab power supply control application
Exec=labpowerqt
Icon=utilities-terminal
Categories=Utility;
EOF
sudo chmod 644 /usr/share/applications/labpowerqt.desktop