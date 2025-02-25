Intel MIPI ov08x40 sensor driver for Arch Linux

Install:
```
pacman -Sy dkms linux-headers gst-plugin-libcamera pipewire-libcamera libcamera-tools libcamera-ipa v4l2loopback-dkms v4l2loopback-utils
cd usbio-drivers
dkms add .
cd ../ov08x40
dkms add.
dkms autoinstall
cd ../v4l2-relayd
makepkg -si
modprobe i2c-usbio gpio-usbio
modprobe v4l2loopback
systemctl enable v4l2-relayd
systemctl start v4l2-relayd
```
