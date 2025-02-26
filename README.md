Intel MIPI ov08x40 sensor driver for Arch Linux

Install:
```
sudo pacman -Sy git base-devel dkms linux-headers gst-plugin-libcamera pipewire-libcamera libcamera-tools libcamera-ipa v4l2loopback-dkms v4l2loopback-utils
git clone https://github.com/henkv1/ov08x40_arch.git
cd ov08x40_arch
sudo dkms add usbio-drivers/
sudo dkms add ov08x40/
sudo dkms autoinstall
cd v4l2-relayd
makepkg -si
sudo systemctl enable v4l2-relayd
echo -e "gpio-usbio\ni2c-usbio\nv4l2loopback" | sudo tee -a /etc/modules-load.d/ov08x40.conf

sudo reboot
```
