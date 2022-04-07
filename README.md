<img src="https://github.com/slimelec/ollie-hw/blob/master/images/mpi_logo.png" width=300>

[www.meatpi.com](https://www.meatpi.com)
---

# Ollie

1. [Hardware](#hardware)
2. [Pinout](#pinout)
3. [Firmware](#firmware)
4. [UART](#uart)
5. [CAN](#can)

### Hardware:
- [Schematic_Ollie_v159r1.pdf](https://github.com/slimelec/ollie-hw/blob/master/hardware/Schematic_Ollie_v159r1.pdf)
- [Schematic_Ollie_DB9_v141r1.pdf](https://github.com/slimelec/ollie-hw/blob/master/hardware/Schematic_Ollie_DB9_v141r1.pdf)


### Pinout:
![Pinout](https://github.com/slimelec/ollie-hw/blob/master/images/Ollie_Pinout2.png)

- **UARTA** and **UARTB** voltage level can be set using slide switch (1V8, 3V3 and 5V).
- **RS232 RX** and **RS232 TX** pins ±12V. **RS232 RX** Absolute Max **±25V**.
## Firmware
Firmware is based on [cantact-fw](https://github.com/slimelec/cantact-fw) and [candleLight_fw](https://github.com/slimelec/candleLight_fw) combined into a single binary file. The firmware can be selected using the dip switch (FW SEL), setting the switch ON the device will boot with candlelight_fw, if switched OFF it will boot with cantact-fw.

#### Flashing new firmware
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/jumper.png" width=400>|<img src="https://github.com/slimelec/ollie-hw/blob/master/images/Prog.png" width=400>

1. Download:
    - [**Default firmware**](https://github.com/slimelec/ollie-hw/blob/master/firmware/ollie_v120.bin)
    - or [**PCAN emulator firmware**](https://github.com/slimelec/ollie-hw/releases/download/v0.3/pcan-ollie.zip)
3. Install [stm32cubeprog](https://www.st.com/en/development-tools/stm32cubeprog.html)
4. Set the jumper to enter bootloader mode.
5. Connect USB and flash firmware

## UART
Based on XR21V1414 4-ch USB to UART chip. Drivers are available [**Here**](https://www.maxlinear.com/support/design-tools/software-drivers)

#### Windows
Once you connect ollie to a Windows machine, you should be able to see 4 COM port channels in the device manager.

![dm](https://github.com/slimelec/ollie-hw/blob/master/images/dev_manager.png)

#### Linux
On Linux you should see four devices created, typically /dev/ttyXRUSB[0-3]. If you see /dev/ttyACM[0-3], there is a chance the UART might not behave correctly in this case you need to install drivers by following these steps:

_Optional_ Raspberry PI kernel headers setup:

Check for correct kernel headers:
```
ls /lib/modules/$(uname -r)/build
```

If not, install the kernel
```
sudo apt install raspberrypi-kernel-headers
```

You may also need to bump to the latest kernel version
```
sudo apt upgrade
sudo reboot
```

Main installation:

```
sudo apt install dkms
git clone https://github.com/brendanhoran/xr_usb_serial_common xr_usb_serial_common-1a 
cd xr_usb_serial_common-1a
make

sudo -s
cp udev_rules/ollie.rules /etc/udev/rules.d/
cp udev_rules/ollie-plug.sh /usr/local/bin/
cp -a ../xr_usb_serial_common-1a /usr/src/
dkms add -m xr_usb_serial_common -v 1a
dkms build -m xr_usb_serial_common -v 1a
dkms install -m xr_usb_serial_common -v 1a
update-initramfs -u
exit

sudo reboot
```

#### Note: RS485 is connected to Ch C and RS232 is connected to Ch D. RS485 Bias(220R) and Termination resister(120R) can be enables using the dip switch. Refer to [pinout](#pinout)

<img src="https://github.com/slimelec/ollie-hw/blob/master/images/ollie_2x7_pinout.png" width=200>

## CAN
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/ollie-front-sel-sw.png">

Ollie runs dual CAN firmware, it can be used with candleLight or slcan firmware.

Setting the dip to postion 1 (as shown in the picture) will enable candleLight. Postion 2 enables slcan. Switch must be set before the Micro USB is plugged in.

Ollie can also run (Peak CAN) PCAN firmware, follow the [**instruction**](#firmware)  to update the firmware.
#### Note: CAN Bus termination resistor(120R) can be enabled by setting the dip switch. Refer to [pinout](#pinout)

### 1- candleLight
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/MIcrobus.PNG">

candleLight firmware is compatible with MicroBus app (by [codenocold](https://github.com/codenocold/microbus)), which is available for [Windows](https://github.com/slimelec/ollie-hw/releases/tag/v0.2) and [Ubuntu](https://github.com/slimelec/ollie-hw/releases/tag/v0.2).

On windows you can also use with Cangaroo app. Download link available [**Here**](https://github.com/slimelec/ollie-hw/releases/download/v0.1/cangaroo-win32-0363ce7.zip). For linux you can compile it yourself, source available [**Here**](https://github.com/normaldotcom/cangaroo/)
#### candleLight Python 
Python drivers are available [**Here**](https://pypi.org/project/candle-driver/)

#### candleLight on Linux
candleLight is natively supported under linux using gs_usb driver. Just plug it in and using this command:

```ip link set can0 up type can bitrate 500000```

### 2- slcan
On windows you can use CANtact app. Download link available [**Here**](https://github.com/linklayer/cantact-app/releases/download/v0.3.0-alpha/cantact-v0.3.0-alpha.zip)

#### slcan Python 
python-can library documentation is available [**Here**](https://python-can.readthedocs.io/en/master/)

### 3- PCAN
When flashed with [**PCAN-USB firmware**](https://github.com/slimelec/ollie-hw#firmware) ollie can be used with PCAN-View or other softwares such as BUSMASTER.

---

© 2021 meatPi Electronics | www.meatpi.com | PO Box 5005 Clayton, VIC 3168, Australia






