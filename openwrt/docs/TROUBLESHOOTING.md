# Troubleshooting

## Terminal in via serial port:

- Connect using a data USB-C cable to the console port on the front of the AP.
- Run `sudo dmesg | tail` to find the assigned tty interface.
  - Usually `/dev/ttyACM0` or `/dev/ttyUSB0`
- Connect to the console using `tio /dev/ttyACM0`
  - If permissions denied ensure account is in the dialout group:
  - `sudo usermod -a -G dialout $USER`, logout then reboot.


## Check ethernet interfaces

- List all interfaces `ifconfig | less`
- Management interface status `ifconfig mgmt-br`
- Toggle interface up and down `ip link set mgmt-br down | up`


## Show connected devices
`lldpcli show nei` for example will show:

```
System name: name of switch plugged
Port ID: what port is connected to the switch
Port description:
(cf-conference center)
(ex-expo floor)
(infraSLOW & infraFAST)
If it displays any other info - may connected to PCC network
```


## Confirming AP OS version

The following should match the commit hash the images were built from and the version of openwrt. Confirm that the build is
latest:

```
~$ cat /etc/scale-release
```


## Identifying successful flashes

WPS Led is setup for the following:

```
even scale conferences - LED ON 
odd scale conferences  - LED OFF
```


## Static IP interface

Dedicated static IP access on AP is possible via WAN(yellow) port and setting a static interface with the following configs:

```
~$ ip link add link enp5s0 name enp5s0.3517 type vlan id 3517
~$ ip addr add 192.168.255.1/24 dev enp5s0.3517
~$ ip link set enp5s0.3517 up
```

> Assumes interface is enp5s0

