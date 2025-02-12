# Meta-Engicam-Hailo

Ver. 1.6

This README file contains information about the **meta-engicam-hailo** Yocto layer.


## SOMs Supported

- hailo15-smarcore

See the [Test Board Documentation](docs/tests.md) for more information


## First Yocto Build

Fetch the BSP sources:

```bash
git clone https://github.com/hailo-ai/meta-hailo-soc.git -b 1.6.0
cd meta-hailo-soc
git clone https://git.engicam.com/internal/meta-engicam-hailo.git
cp meta-engicam-hailo/kas/hailo15-smarcore.yaml kas/
kas build kas/hailo15-smarcore.yaml
```

Once you finished the first build you can use the Yocto framework as usual:

Es.

```bash
source poky/oe-init-build-env

devtool modify linux-yocto-hailo
devtool u-boot
bitbake core-image-minimal
```

## Bootloader on SPI Programming

See the [Bootloader Programming](docs/bootloader.md) for more information

## eMMC Programmig

Follow the following steps:

1. Copy the **wic** image file on usb stick.
2. Boot from the sdcard.
3. From Linux terminal type the commands:

```bash
mount /dev/sda1 /mnt
dd if=/mnt/<wic image file> of=/dev/mmcblk1 bs=128M conv=fsync
```

4. Reboot the system.
5. From u-boot CLI type the command:

```bash
run boot_mmc1
```
