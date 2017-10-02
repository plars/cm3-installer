# cm3-installer

## Introduction

This is a snap used for some automated testing of Ubuntu Core images
on RPi CM3 devices. It runs on a RPi with an automation phat hat attached
to it for automating installation of a CM3 attached to it.

You can see more information about the automation phat at:
https://shop.pimoroni.com/products/automation-phat

## Setup

To make use of this, you'll need to install it with --devmode. You will also
need to install the 'pi3gpio' snap.

To connect your RPi 3 and automation phat, you will need to modify a usb
cable. Cut open a micro usb cable and locate the 5v power wire. Cut this
wire and strip both ends of it. You will need to connect those two ends
to the relay on the automation phat.  One of the wires should be connected
to 'COM' and the other to 'NO' so that it is in the "normally open" side.
The USB-A connector on this cable should be connected to one of the open
usb ports on the RPi 3. The micro-usb end should be connected to the
usb slave port on the CM3.

Next, make sure that the 'usb slave boot enable' jumper on the CM3 is
in the 'en' (enabled) position.  This will allow writing to the CM3 as a
usb storage device when the power is enable on the usb slave cable, but
when the power is disabled, it will boot the image normally.

Next, hook up power and usb network dongles, and any other equipement needed
to the cm3 as usual.

## Usage
The following commands will need to be executed on the RPi 3 (control host)
attached to the CM3, not the CM3 itself.

Set the relay to high::

```
   $ sudo pi3gpio set high 16
```

Plug power into the cm3 and run::

```
   $ sudo cm3-installer <URL>
```

...where <URL> is the url of the image (xz) you want to download and install.
Once this command completes, run::

```
   $ sync
   $ sudo udisksctl power-off -b /dev/sda
   $ sudo pi3gpio set low 16
```

You may now power off the CM3 and power it back on, and it should boot with
the image you specified.
