# Smarcore 4K with hailo15-smarcore


|  Device            | Status | NOTE                    |
|:------------------:|:------:|:-----------------------:|
|RTC                 | OK     |                         |
|HUB USB             | OK     |                         |
|Camera input        | OK     |                         |
|Console Debug       | OK     | On J29                  |
|Ethernet            | OK     |                         |
|SDCard              | OK     |                         |
|eMMC                | OK     |                         |
|Audio               | TBT    |                         |
|Wifi                | TBT    |                         |
|PCIe                | TBT    |                         |


## RTC

````
root@hailo15:~# dmesg  | grep -i rtc
[    1.401355] rtc-pcf8523 1-0068: registered as rtc0
[    1.407471] rtc-pcf8523 1-0068: setting system clock to 2018-03-09T12:35:46 UTC (1520598946)

root@hailo15:~# hwclock
2018-03-09 12:37:40.821722+00:00
root@hailo15:~# hwclock --systohc
````


## HUB USB

Hub USB reset GPIO:

````
gpioset gpiochip1 7=0
````

## Camera input

Tested with Hailo detect.sh demo application:

### hailo15 side command

````
cd /home/root/apps/detection
./detection.sh
````

### Host PC command

````
gst-launch-1.0 -v udpsrc port=5000 address=0.0.0.0 ! application/x-rtp,encoding-name=H264 ! queue ! rtph264depay ! queue ! h264parse ! avdec_h264 ! queue ! videoconvert ! fpsdisplaysink video-sink=autovideosink text-overlay=false sync=false
````

Note:

you should be need to modify detectio.sh updating the line:

````
    readonly DEFAULT_UDP_HOST_IP="10.0.0.2"
````

with your PC ip address.


## Ethernet

on u-boot

````
hailo15# gpio clean 19
hailo15# setenv ipaddr 10.24.0.52
hailo15# ping 10.24.0.1
````

# GPIO assigment


elow the gpio available:

|GPIO     | FUNCTION           |
|:-------:|:------------------:|
|GPIO_0   | GPIO_0             |
|GPIO_1   | GPIO_1             |
|GPIO_2   | LCD_BKLT_PWM       |
|GPIO_3   | PWM3               |
|GPIO_4   | GPIO_4             |
|GPIO_5   | GPIO_5             |
|GPIO_6   | i2c2_sda           |
|GPIO_7   | i2c2_scl           |
|GPIO_8   | PWRON_HOLD         |
|GPIO_9   | WLAN_WAKE_N        |
|GPIO_10  | SDIO0_CD_N         |
|GPIO_11  | USB_OVERCURRENT_N  |
|GPIO_12  | usb_drive_vbus_out |
|GPIO_13  | N/A                |
|GPIO_14  | GPIO_14            |
|GPIO_15  | PCIE_RST_N         |
|GPIO_16  | reset_cam_0        |
|GPIO_17  | WiFi_BT_disable_n  |
|GPIO_18  | GPIO_18            |
|GPIO_19  | ETH_RST            |
|GPIO_20  | WL_DISABLE_N       |
|GPIO_21  | PWRON_nPB          |
|GPIO_22  | SEL_CARD/WLAN_N    |
|GPIO_23  | HUB_USB_RST/EN_N   |
|GPIO_24  | LVDS_EN            |
|GPIO_25  | RTC_WAKE_N         |
|GPIO_26  | GPIO_26            |
|GPIO_27  | GPIO_27            |
|GPIO_28  | GPIO_28            |
|GPIO_29  | GPIO_29            |
|GPIO_30  | GPIO_30            |
|GPIO_31  | LVDS_IRQ           |


## GPIO example

Frou muser space you must use the GPIOLib tooù

example:

````
# gpioinfo
8<-- snip -->8

# gpiofind WiFi_BT_disable_n
gpiochip1 1
# gpioset gpiochip1 1=0
# gpioset gpiochip0 1=1gpio
````

