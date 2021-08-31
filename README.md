# ArchLinux-ARMv7-cho-Nexus-7-2012-wifi-rev-e1565-kernel-514-rc3-support-USB-OTG
Linux on Nexus 7 2012

Kết nối với usb keyboard qua cổng micro-usb vì đây là Archlinux minimal distro nên tự chọn DE cài, lxdm/lxqt(lxde), xfce 4.16, xfce4 mix i3wm, gnome 40, phosh, sxmo



Link download trên Google drive:



https://drive.google.com/folderview?id=1szK5trBse8-j1hSIgzZDppL3R6ySNSCP



Cài android-tools và fastboot trên Linux/Windows



Unlock bootloader cho Nexus 7, tham khảo trên mạng

https://wiki.postmarketos.org/wiki/Google_Nexus_7_2012_(asus-grouper)



Variants

grouper rev. PM269 - without GSM (oldest)
grouper rev. E1565 - without GSM (modern revision)
tilapia rev. E1565 - with GSM



Do I have grouper or tilapia?



TWRP (adb shell) $ grep androidboot.baseband=unknown /proc/cmdline && echo grouper || echo tilapia



Which hardware revision of grouper do I have?



TWRP (adb shell) $ find /sys/devices/ | grep -c max776 && echo You have E1565



TWRP (adb shell) $ find /sys/devices/ | grep -c tps6591 && echo You have PM269



Đưa máy về bootloader. Kết nối Nexus 7 2012 wifi vào PC/laptop. Chuẩn usb 1.0, 1.1, 2.0 dùng tốt. Chuẩn usb 3.0 dễ bị over cache push vào Nexus 7 2012, unsupport



$ sudo adb reboot bootloader



Flash boot image vào boot partition (đổi tên boot.img-asus-grouper thành boot.img)



$ fastboot flash boot boot.img



Cài TWRP 3.3.1-0 trở lên, vào Advance → Terminal



$ df



$ umount /dev/block/mmcblk0p__   <- fill partition number   #(2 lần)



Dùng lệnh adb để bung rootfs vào mmcblk0p__ trên PC/laptop



$ sudo adb start-server



Chuyển đến thư mục chứa rootfs image



$ sudo adb push ArchLinuxARM-armv7-latest+asus-grouper.img /dev/block/mmcblk0p__  <- fill partition number



grouper has likely data on /dev/block/mmcblk0p9 but make sure!
tilapia has likely data on /dev/block/mmcblk0p10 but make sure!



default user alarm with the password alarm



Các utils để trong /opt gồm các scripts:



- sysctl.conf tối ưu VMs và thông số kernel



- cpufreq.start tối ưu Ondemand governor



- temp_throttle để kìm hãm con ngựa Tegra thành Troy không quá nhiệt (hơn 60 độ kernel sẽ tự khởi động lại, để 59 độ là max, ở 53 và 55 độ ổn định)



- clear_RAM để remove thêm RAM nếu cần



Grate-driver được phát triển ở đây, accelerate 2D và 3D qua Gallium(libllvm11):



https://github.com/grate-driver/



Opentegra driver GPU 2D accelerate, ArchLinux AUR



https://aur.archlinux.org/packages/xf86-video-opentegra-git/



***Fix sound ALC5642 cho tegra-rt5640***



https://help.ubuntu.com/community/SoundTroubleshooting



https://forum.ubuntuusers.de/topic/medion-akoya-e2228t/2/



$ sudo lsmod | grep "^snd" | cut -d " " -f 1

snd_soc_tegra30_i2s

snd_soc_tegra_pcm

snd_soc_tegra_rt5640

snd_soc_tegra_utils

snd_soc_rt5640

snd_soc_rl6231

snd_soc_core

snd_soc_tegra30_ahub

snd_pcm_dmaengine

snd_pcm

snd_timer

snd



$ sudo nano /etc/modules

snd_soc_tegra30_i2s

snd_soc_tegra_pcm

snd_soc_tegra_rt5640

snd_soc_tegra_utils

snd_soc_rt5640

snd_soc_tegra30_ahub



$ reboot



Checking soc soundcard loaded:



$ sudo cat /proc/asound/card*/id



ALC5642



$ sudo alsa force-reload



$ alsamixer



Enable các thông số thiết lập (phím M hoặc phím mũi tên lên/xuống): "Speaker R" "Speaker L" "DAC MIXR INF1" "DAC MIXL INF1" "SPOL MIX DAC R1" "SPOL MIX DAC L1" "Stereo DAC MIXR DAC R1" "Stereo DAC MIXL DAC L1"



Wifi dùng wifi-menu/wpa_supplicant/iwd và network-manager



Bluetooth dùng được bluez5 và blueman



NFC dùng được với neard



Image source build từ đây:



https://forum.xda-developers.com/t/linux-on-the-acer-iconia-tab-a500-2020-edition.4136023/post-85373009
