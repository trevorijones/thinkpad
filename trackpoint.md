# Configure track point

Note: X as display manager

## Manually

Find the device: can be serio1 serio2
```sh
find /sys/devices/platform/i8042/ -name sensitivity
```

Sensitivity tweak:
echo 220 >  /sys/bus/serio/devices/serio1/sensitivity

## Persist with udev

Check the path to sys device file of track point 

Create the udev rule:

touch /etc/udev/rules.d/trackpoint.rules

SUBSYSTEM=="serio", DRIVERS=="psmouse", DEVPATH="/sys/devices/platform/i8042/serio1", ATTR{sensitivity}="220", ATTR{speed}="110"

On some systems use WAIT_FOR="/sys/devices/platform/i8042/serio1/sensitivity" instead of DEVPATH

Save the file and either reboot or run the commands above:
* sudo udevadm control --reload-rules
* sudo udevadm trigger 

Yeah!

