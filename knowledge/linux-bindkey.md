---
title: Linux: Bindkey
description: 
published: true
date: 2026-01-22T22:42:08.628Z
tags: kb, linux
editor: markdown
dateCreated: 2026-01-22T22:42:08.628Z
---

# Linux: Bindkey

To make the numpad on some keyboards work in the linux terminal, use `bindkey -s "<KEYCODE>" "<KEY>"`

To determine the keycode, you can press  <kbd>Ctrl</kbd> + <kbd>v</kbd> followed by the key to see it in the terminal

Example excerpt from my `.zshrc`:

```bash
# Keypad
# 0 . Enter
bindkey -s "^[Op" "0"
bindkey -s "^[On" "."
bindkey -s "^[OM" "^M"
# 1 2 3
bindkey -s "^[Oq" "1"
bindkey -s "^[Or" "2"
bindkey -s "^[Os" "3"
# 4 5 6
bindkey -s "^[Ot" "4"
bindkey -s "^[Ou" "5"
bindkey -s "^[Ov" "6"
# 7 8 9
bindkey -s "^[Ow" "7"
bindkey -s "^[Ox" "8"
bindkey -s "^[Oy" "9"
# + -  * /
bindkey -s "^[Ol" "+"
bindkey -s "^[OS" "-"
bindkey -s "^[OR" "*"
bindkey -s "^[OQ" "/"
```

> Thanks [Adaephon](https://superuser.com/a/742193)!