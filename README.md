# Ollie Hardware

Hardware files coming soon.

# Hardware Guide
### Pinout:
![Pinout](https://github.com/slimelec/ollie-hw/blob/master/images/ollie-pinout.jpg)

### Firmware
Firmware is based on cantact-fw and candleLight_fw combined into a single binary file. The firmware can be selected using the dip switch (FW SEL), setting the switch ON the device will boot with candlelight_fw, if switched OFF it will boot with cantact-fw.

### Flashing new firmware
<img src="https://github.com/slimelec/ollie-hw/blob/master/images/jumper.png" width=400>|<img src="https://github.com/slimelec/ollie-hw/blob/master/images/Prog.png" width=400>

1. Download [firmware](https://github.com/slimelec/ollie-hw/blob/master/firmware/ollie_v120.bin)
2. Install [stm32cubeprog](https://www.st.com/en/development-tools/stm32cubeprog.html)
3. Set the jumper to enter bootloader mode.
4. Connect USB and flash firmware
