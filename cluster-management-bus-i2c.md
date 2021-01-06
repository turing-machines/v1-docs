# Cluster Management Bus \(i2c\)

Turing Pi cluster management bus configuration, security and internal devices

### Configure I2C and devices <a id="configure-i2c-and-devices"></a>

First аdd `dtoverlay` 

```text
sudo nano /boot/config.txt
```

and add the following lines

```text
dtoverlay=i2c1,pins_44_45
dtoverlay=i2c-rtc,mcp7940x
```

Enable I2C interface in `sudo raspi-config` -&gt; Interfacing Options -&gt; I2C -&gt; enable and reboot

Check everything is fine with I2C and RTC

```text
dmesg | grep i2c
[    4.414436] i2c /dev entries driver  # ok

ls /dev/*i2c*
/dev/i2c-1  # ok

dmesg | grep rtc
[    6.489206] rtc-ds1307 1-006f: registered as rtc0

ls /dev/*rtc*
/dev/rtc  /dev/rtc0
```

### I2C tools <a id="i2c-tools"></a>

Install i2c-tools

```text
sudo apt-get install i2c-tools
```

{% hint style="info" %}
If the command doesn't work update your pi



```text
sudo apt-get update
```
{% endhint %}

Now check our I2C devices on the CMB

```text
sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- 57 -- -- -- -- 5c -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- 6f
70: -- -- -- -- -- -- -- --
```

You can see three I2C devices Ethernet Switch \(0x5Ch\), I2C expander \(0x57h\) and RTCC \(0x6Fh\).

The `i2cget` command is used to read a byte from a specified register on the I2C device. The format for this command is as follows

```text
i2cget [-f] [-y] 1   [MODE]
```

\[-f\] \[-y\] Options:

* -f force access to the device even if the device is still busy. However, be careful. This can cause the i2cget command to return an invalid value. We recommend avoiding this option.
* -y disable interactive mode. Using this command will skip the prompt for confirmation from the i2cget command. By default, it will wait for the confirmation from the user before interacting with the I2C device.
* DEVICE\_ADDRESS is an I2C bus address \(e.g 0x04h\)
* ADDRESS is the address on the slave from which to read data \(eg. 0x00h\)

The optional MODE can be one of the following:

* b – read/write a byte of data, this is the default if left blank
* w – read/write a word of data \(two bytes\)
* c – write byte/read byte transaction

Example of reading from register 0xF2h where factory default value is 0xFFh

```text
sudo i2cget -y 1 0x57 0xf2
0xff
```

Use the `i2cset` command to write data to an I2C device. The format of this command as follows

```text
i2cset [-m mask] [-f] [-y] 1    [MODE]
```

Options:

* -f, -y, DEVICE\_ADDRESS, ADDRESS as for i2cget
* -m mask the bit mask parameter, if specified, describes which bits of value will be actually written to data-address. Bits set to 1 in the mask are taken from VALUE, while bits set to 0 will be read from ADDRESS and thus preserved by the operation

Example of writing value 0x77h to the first byte of user space SRAM \(starts from 0x00h\) and then reading

```text
sudo i2cset -y 1 0x57 0x00 0x77
sudo i2cget -y 1 0x57 0x00
0x77
```

### Security <a id="security"></a>

As a security precaution, system devices are not exposed by default inside Docker containers. You can expose specific devices to your container using the `--device` option to docker run, as in

```text
docker run --device /dev/i2c-1 app-image
```

or remove all restrictions with the `--privileged` flag

```text
docker run --privileged app-image
```

### User space EEPROM <a id="user-space-eeprom"></a>

I2C expander and RTCC each have 64 bytes of general purpose user memory, so combined you have 128 bytes of EEPROM. On device 0x57h user space address from 0x00h to 0x3Fh and on 0x6Fh from 0x20h to 0x5Fh.

### Ethernet Switch <a id="ethernet-switch"></a>

The onboard RTL8370 integrate all the functions of a high-speed switch system; including SRAM for packet buffering, non-blocking switch fabric, and internal register management into a single CMOS device.

Device Address: 0x5Ch

### I2C expander <a id="i2c-expander"></a>

I2C expander offers users a digitally programmable alternative to hardware jumpers and mechanical switches that are being used to control power on compute nodes.

Device Address: 0x57h

| Register | Register Description | Default Value |
| :--- | :--- | :--- |
| 0x00h to 3Fh | 64 bytes of general-purpose user EEPROM | 0x00h |
| 0xF2h | Control Register | 0xFFh |
| 0xF8h | Status Register | – |
| 0xFAh to 0xFFh | 6 bytes of general-purpose user SRAM | – |

#### Power Management <a id="power-management"></a>

On/off compute module through register 0xF2h

```text
# Bits  : 76543210
# Slots : 5674321x

# Power off worker nodes 2,3,4,5,6,7
sudo i2cset -m 0xfc -y 1 0x57 0xf2 0x00

# Power on worker nodes
sudo i2cset -m 0xfc -y 1 0x57 0xf2 0xff
```

On/off compute module by node number

```text

# 0x02 : Node #1 (Master)
# 0x04 : Node #2 (Worker 1)
# 0x08 : Node #3 (Worker 2)
# 0x10 : Node #4 (Worker 3)
# 0x80 : Node #5 (Worker 4)
# 0x40 : Node #6 (Worker 5)
# 0x20 : Node #7 (Worker 6)

# Power off node #3
sudo i2cset -m 0x08 -y 1 0x57 0xf2 0x00

# Power on node #7
sudo i2cset -m 0x20 -y 1 0x57 0xf2 0xff
```

Read on/off status

```text
sudo i2cget -y 1 0x57 0xf2
0xff
```

#### Compute Module status <a id="compute-module-status"></a>

Read compute module status through register 0xF8h

* 1 = Installed in slot AND switched on
* 0 = Not installed in slot OR switched off

```text
# Bits  : 76543210
# Slots : 5674321x

sudo i2cget -y 1 0x57 0xf8
0x0e  # register 0xf2 = 0xff, all slots are power up but installed only three
```

### Real-Time Clock/Calendar <a id="real-time-clock-calendar"></a>

Real-Time Clock/Calendar \(RTCC\) tracks time using internal counters for hours, minutes, seconds, days, months, years, and day of week. Alarms can be configured on all counters up to and including months.

SRAM and timekeeping circuitry are powered from the back-up supply when main power is lost, allowing the device to maintain accurate time and the SRAM contents. The times when the device switches over to the back-up supply and when primary power returns are both logged by the power-fail time-stamp.

Device Address: 0x6Fh

| Register | Register Description | Default Value |
| :--- | :--- | :--- |
| 0x00h to 0x06h | Time & Date | – |
| 0x07h to 0x09h | Configuration and Trimming | – |
| 0x0Ah to 0x10h | Alarm 0 | – |
| 0x11h to 0x17h | Alarm 1 | – |
| 0x18h to 0x1Fh | Power-Fail/Power-Up Time-Stamps | – |
| 0x20h to 0x5Fh | 64 bytes of general-purpose user SRAM | – |

#### Time & Date <a id="time-date"></a>

Show time from hardware RTC

```text
sudo hwclock --show
2019-10-26 20:21:03.005326+01:00
```

### External I2C port <a id="external-i2c-port"></a>

Pinouts

```text
|  1  |  2  |  3  |  4  |
| GND | VCC | SCL | SDA |
```

## Reference Links <a id="reference-links"></a>

* [I2C expander datasheet](https://datasheets.maximintegrated.com/en/ds/DS4520.pdf)
* [RTC datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/MCP7940N-Battery-Backed-I2C-RTCC-with-SRAM-20005010G.pdf)

