meta-engicam-hailo
==================

Ver. 1.5

This README file contains information about the meta-engicam-hailo yocto layer.


SOMs supported
--------------

- hailo15-smarcore

See the [Test Board Documentation](docs/tests.md) for more information


## First buld


````
git clone https://github.com/hailo-ai/meta-hailo-soc.git -b 1.5.0
cd meta-hailo-soc
kas build kas/hailo15-sbc.yml
git clone **TODO engicam repo** ..
bitbake-layers add-layer ../meta-engicam-hailo

````

On conf/local.conf file you must set the right machine:

````
-MACHINE ??= "hailo15-sbc"
+MACHINE = "hailo15-smarcore"
````


Now you can build your image:


````
bitbake core-image-minimal
````

Once that the build folder is created you can lanch the command below for settings the environment.


````
source poky/oe-init-build-env
````


# Bootloader programmaing

The first part of bootaloder is store in and external SPI eeprom.

The following instruction illustrate how reprograming it.


### HW setup on multimedia 4k


serial connection on J29 and dip switch 2 a ON



## First step

Dowloand the sw package :


hailo_vision_processor_sw_package_2024-10.tar.gz

from:

https://hailo.ai/developer-zone/software-downloads/


Unpack it and launch into the folder the virtaul python enviroment setup:

````
hailo_vision_processor_sw_package_2024-10/tools
python3 -m venv .
source ./bin/activate
pip install hailo15_board_tools-1.5.0-py3-none-any.whl
````

This oparatin must be do only one time. You can easyly enter on python virtual enviroment with command:

````
source ./bin/activate
````

## SPI programing

In order to program the SPI memory you  need to follow the following steps:

1. Move the dip switch 2 a ON position.

2. Prepare the enviroment for flashing:

````
cd <hailo_vision_processor_sw_package folder>/tools
ln -sf <yocto hailo folder>/meta-hailo-soc/build/tmp/deploy/images/hailo15-smarcore .
````

3. Power on the board and type the following command to host terminal


````
uart_boot_fw_loader --firmware hailo15-smarcore/hailo15_uart_recovery_fw.bin  --serial-device-name /dev/ttyUSB0

hailo15_spi_flash_program --scu-bootloader hailo15-smarcore/hailo15_scu_bl.bin --scu-firmware hailo15-smarcore/hailo15_scu_fw.bin --uboot-device-tree hailo15-smarcore/u-boot.dtb.signed --bootloader hailo15-smarcore/u-boot-spl.bin  --scu-bootloader-config hailo15-smarcore/scu_bl_cfg_a.bin --bootloader-env hailo15-smarcore/u-boot-initial-env --customer-certificate hailo15-smarcore/customer_certificate.bin --uart-load  --serial-device-name /dev/ttyUSB0

````

4. Power off the board move the dip switch 2 a OFF position and power on the board

5. Check the boot



## eMMC programmig


From u-boot CLI type the command:

````
run boot_mmc1
````