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

```
touch /etc/udev/rules.d/99-trackpoint.rules
```

To know exactly which rule files are applied and in which order, use the `test` udev command:

```
udevadm test -a add  /sys/devices/platform/i8042/serio1
```

And to know which attributes are available, use the udevadm info command:
```
udevadm info -ap /sys/devices/platform/i8042/serio1
```

Now here is the `99-trackpoint.rules`:

```
ACTION=="add" \
, SUBSYSTEM=="serio" \
, DEVPATH=="/devices/platform/i8042/serio1" \
, ATTR{sensitivity}="70" \
, ATTR{resolution}="50"
```

You can test with the above test command, load and trigger the add event:

* sudo udevadm control --reload
* sudo udevadm trigger --action add

Yeah!

