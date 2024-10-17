
# Bootloader Programming

The first part of bootloader is stored in an external SPI EEPROM.

The following instruction illustrate how to reprogram it.


## Hardware Setup on Multimedia 4k

Serial connection on **J29** and dip switch 2 ON

## First Step

Download the sw package from this [link](https://hailo.ai/developer-zone/software-downloads/):

**hailo_vision_processor_sw_package_2024-10.tar.gz**

Unpack it, launch the virtual python enviroment setup in the folder and install it with **pip**:

```bash
cd hailo_vision_processor_sw_package_2024-10/tools
python3 -m venv .
source ./bin/activate
pip install hailo15_board_tools-1.5.0-py3-none-any.whl
```

>**Note**: This step must be done only one time. You can easyly enter in python virtual enviroment with:

```bash
source ./bin/activate
```

## SPI Programing

In order to program the **SPI memory** you  need to follow the following steps:

1. Move the dip switch 2 on the ON position.

2. Prepare the enviroment for flashing:

```bash
cd <hailo_vision_processor_sw_package folder>/tools
ln -sf <yocto hailo folder>/meta-hailo-soc/build/tmp/deploy/images/hailo15-smarcore .
```

3. Power on the board and type the following command in the **host terminal**:

````bash
uart_boot_fw_loader --firmware hailo15-smarcore/hailo15_uart_recovery_fw.bin  \
--serial-device-name /dev/ttyUSB0

hailo15_spi_flash_program --scu-bootloader hailo15-smarcore/hailo15_scu_bl.bin \
--scu-firmware hailo15-smarcore/hailo15_scu_fw.bin \
--uboot-device-tree hailo15-smarcore/u-boot.dtb.signed \
--bootloader hailo15-smarcore/u-boot-spl.bin  \
--scu-bootloader-config hailo15-smarcore/scu_bl_cfg_a.bin \
--bootloader-env hailo15-smarcore/u-boot-initial-env \
--customer-certificate hailo15-smarcore/customer_certificate.bin \
--uart-load  --serial-device-name /dev/ttyUSB0
````

4. Power off the board move the dip switch 2 on the OFF position and power on the board

5. Check if the module boots correctly
