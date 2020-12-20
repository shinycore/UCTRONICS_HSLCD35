# UCTRONICS_HSLCD35

Driver for U6111 SPI display. This is a fork of an original repository, with fixes to make it work with my Raspberry Pi
Zero (Raspberry Pi OS 2020-12-02, 5.4 kernel, stable communication with no flickering or missing/white/partial frames).
I haven't tested other distros but installation procedure should be basically the same. DTB files are
architecture-agnostic, and these should work with any 5.4 kernel.

**Important:** I'm by no means experienced kernel developer/hacker, I just had to fix it, shrug. Do not expect As for
your Qs.

## Touch

Manufacturer's DTS for 5.4 is fine. Something has changed upstream so they needed to invert X/Y axis. At the end of the
day, it works.

## Display

Someone has made decision to change GPIO reset pin from `0x19` (GPIO25, 22nd pin) to `0x10` (GPIO16, 36th pin). The
problem is this display has 26-pin connector, so GPIO16 is floating (
see: https://www.raspberrypi.org/documentation/usage/gpio). I had to revert this change.

There are similar SPI displays out there, for example Waveshare ones. Updated overlays use the same GPIO pins for both
LCD reset and TS pen down event, but logic state values are inverted (`0` -> `1`,
see: https://github.com/swkim01/waveshare-dtoverlays/commit/f0bdf7200c5822d73d2e013712c98903bfd96ca8). With this
changed, the display finally works!

## Installation instructions

```bash
git clone https://github.com/shinycore/uctronics_hslcd35
cd uctronics_hslcd35
sudo apt install cmake --yes
sudo ./install_driver.sh
```
