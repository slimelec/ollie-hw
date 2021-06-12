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

## Firmware
Firmware is based on [cantact-fw](https://github.com/slimelec/cantact-fw) and [candleLight_fw](https://github.com/slimelec/candleLight_fw) combined into a single binary file. The firmware can be selected using the dip switch (FW SEL), setting the switch ON the device will boot with candlelight_fw, if switched OFF it will boot with cantact-fw.

#### Flashing new firmware
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/jumper.png" width=400>|<img src="https://github.com/slimelec/ollie-hw/blob/master/images/Prog.png" width=400>

1. Download [firmware](https://github.com/slimelec/ollie-hw/blob/master/firmware/ollie_v120.bin)
2. Install [stm32cubeprog](https://www.st.com/en/development-tools/stm32cubeprog.html)
3. Set the jumper to enter bootloader mode.
4. Connect USB and flash firmware

## UART
Based on XR21V1414 4-ch USB to UART chip. Drivers are available [**Here**](https://www.maxlinear.com/support/design-tools/software-drivers)

#### Windows
Once you connect ollie to a Windows machine, you should be able to see 4 COM port channels in the device manager.

![dm](https://github.com/slimelec/ollie-hw/blob/master/images/dev_manager.png)

#### Linux
On Linux you should see four devices created, typically /dev/ttyXRUSB[0-3]. If you see /dev/ttyACM[0-3], there is a chance the UART might not behave correctly in this case you need to install drivers by following these steps:
1. \# git clone https://github.com/brendanhoran/xr_usb_serial_common
2. \# make *(on raspberry pi make sure you have the [kernel headers](https://www.raspberrypi.org/documentation/linux/kernel/headers.md) installed)*
3. \# cp -a ../xr_usb_serial_common-1a /usr/src/
4. \# dkms add -m xr_usb_serial_common -v 1a
5. \# dkms build -m xr_usb_serial_common -v 1a
6. \# dkms install -m xr_usb_serial_common -v 1a
7. \# echo blacklist cdc-acm > /etc/modprobe.d/blacklist-cdc-acm.conf
8. \# update-initramfs -u
9. Now plug in ollie.

#### Note: RS485 is connected to Ch C and RS232 is connected to Ch D. RS485 Bias(220R) and Termination resister(120R) can be enables using the dip switch. Refer to [pinout](#pinout)

<img src="https://github.com/slimelec/ollie-hw/blob/master/images/ollie_2x7_pinout.png" width=200>

## CAN
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/ollie-front-sel-sw.png">

Ollie runs dual CAN firmware, it can be used with candleLight or slcan firmware.

Setting the dip to postion 1 (as shown in the picture) will enable candleLight. Postion 2 enables slcan. Switch must be set before the Micro USB is plugged in.

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






