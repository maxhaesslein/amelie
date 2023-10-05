# Amélie v.3

**Amélie** is a selfie printer that prints photos on cheap thermal paper, perfect for parties or weddings. It is named after [the 2001 movie by Jean-Pierre Jeunet](https://letterboxd.com/film/amelie/).

See [here](https://www.maxhaesslein.de/visual/objects/amelie/) for more pictures and details.

This is a personal project and may be a bit finicky to set up.

## Parts

- [Raspberry Pi 4 B (1GB)](https://www.raspberrypi.com/products/raspberry-pi-4-model-b/)
- [Waveshare 4inch HDMI Capacitive Touch IPS LCD Display](https://www.waveshare.com/product/raspberry-pi/displays/lcd-oled/4inch-hdmi-lcd-c.htm)
- [Adafruit Mini Thermal Receipt Printe](https://www.adafruit.com/product/597)
- [Adafruit Rugged Metal Pushbutton (22mm)](https://www.adafruit.com/product/3423)
- [Raspberry Pi Camera Module 3](https://www.raspberrypi.com/products/camera-module-3/)
- [Aioneus 40W 4-Port Fast Charging Block](https://www.aioneus.com/collections/usb-charger-plug/products/aioneus-40w-4-port-fast-charging-block-pd-qc-wall-plug)
- [DAOKAI USB Type-C Power Converter](https://www.amazon.de/dp/B0B2NSJ14M)
- Plexi Plate (Thanks to Cosi for helping me with the laser cutter)
- Amazon Basic Case
- small heat sinks for the Raspberry Pi
- a fast SD card
- a cheap, small USB thumb drive
- some additional cables

## Installation

Use a fresh Raspberry Pi OS Lite 32-bit installation. Install required dependencies with:

```bash
sudo apt install python3-opencv python3-picamera2 xserver-xorg xinit
```

Then download or clone this repo, extract it into `~/amelie` (this path is hardcoded at some places, if you want to use another path, change the code accordingly), then start it by calling the `start` file.

## Starting over SSH

To allow starting over SSH, you may want to add this to your */etc/X11/Xwrapper.config*:

```
allowed_users=anybody # was: console
```

then you can call the *start* file over SSH

## Autostart

To autostart, copy the `amelie.service` file into `~/.config/systemd/user/amelie.service` and then activate it with:

```
sudo systemctl daemon-reload
systemctl --user enable amelie.service
systemctl --user start amelie.service
journalctl --user -u amelie.service
```

## Settings

This script uses the picamera2 library, so if you use anything other then the official Raspberry Pi camera you may need to change some things accordingly.

## USB thumb drive, config options

You can mount a USB thumb drive at */mnt/usb0/* (or change this path in the `amelie` file). The script will automatically check, if this drive is mounted and if an `amelie.txt` file exists at this path. If it exists, it reads this file and overwrites the default options.
See `config-template/amelie.txt` for an example config file.
If you configure a `backupDirectory`, all images will be saved in this directory.
`pinR`, `pinG` and `pinB` are the pins for the RGB led. `pinButton` is the pin to listen for the button press. `pinShutdown` is another button that you can use to power down the Raspberry Pi. You need to hold this button for a few seconds before the shutdown triggers.
