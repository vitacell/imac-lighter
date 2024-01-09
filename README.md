# iMac-lighter
iMac screen backlight adjust on the ambient light, without baklit keyboard support. For Macbook version go to:
https://github.com/harttle/macbook-lighter/

!! TESTED on iMac late 2015 4K i5, ArchLinux, Xfce4 on Xorg/X11.

Internally, imac-lighter reads the following files:

* /sys/devices/platform/applesmc.768/light 		      # ambient light sensor
* /sys/class/backlight/acpi_video0/brightness		    # video brightness
* /sys/class/backlight/acpi_video0/max_brightness	  # maximum brightness


---------------------------------------------------------------------------------------------
So you're expected to install corresponding Nvidia/Intel drivers first.

Sometimes, some GNU/Linux distros use newer Linux versions, those have too much regressions after Linux 5.15. If you have screen's no backlight controls, missing sliders, and your power manager fails to recognize the backlight... you might need to add a specific "acpi_backlight" kernel parameter because of regressions, please read:

https://wiki.archlinux.org/title/Kernel_parameters

For dirty fast, just add some of these lines (test one by one, util one works for you):

acpi_backlight=native

acpi_backlight=video

acpi_backlight=vendor

acpi_backlight=none

into /etc/default/grub file, line "GRUB_CMDLINE_LINUX_DEFAULT".
Your line must to look something like this: 
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet acpi_backlight=video split_lock_detect=off"

Then update your bootloader. For grub usually is, then reboot the iMac:
grub-mkconfig -o /boot/grub/grub.cfg

For manual backlight adjusting, make sure you have a power manager installed (in Xfce is "xfce4-power-manager").
---------------------------------------------------------------------------------------------


---------------------------------------------------------------------------------------------
!! For other iMac model, you maybe need to replace brightness directory in /sys/class/brightness, in the imac-lighter program files.

Find your backlight directory/directories with this command:
ls /sys/class/backlight/*

some of them, is your actual backlight controlling files, you might want to manual test with some command like this:

(putting the backling at 10%):
sudo sh -c 'echo 10 > /sys/class/backlight/PUT_HERE_YOUR_BACKLIGHT_FOLDER/brightness'
---------------------------------------------------------------------------------------------


---------------------------------------------------------------------------------------------
## Setup

All commands including imac-lighter-screen
will be available with sudo previledge once imac-lighter finished install.

To use in non-root environment such as [xbindkeys](https://wiki.archlinux.org/index.php/Xbindkeys),
it's recommended to setup an "udev" rule to allow users in the
"video" group to set the backlights.
Place a file /etc/udev/rules.d/90-backlight.rules containing:

```
SUBSYSTEM=="backlight", ACTION=="add", \
  RUN+="/bin/chgrp video /sys/class/backlight/%k/brightness", \
  RUN+="/bin/chmod g+w /sys/class/backlight/%k/brightness"
```
---------------------------------------------------------------------------------------------


---------------------------------------------------------------------------------------------
## Usage

```bash
# Increase screen backlight by 50
imac-lighter-screen --inc 50
# Set screen backlight to max
imac-lighter-screen --max
# start auto adjust daemon
systemctl start imac-lighter
# start auto adjust interactively, root previlege needed
imac-lighter-ambient
```
---------------------------------------------------------------------------------------------


## Tested iMacs Versions:

* iMac late 2015 4K i5, ArchLinux, Xfce4 on Xorg/X11, LY as display manager.
