# Netgear-A6210
This driver supports Ralink / Mediatek mt766u, mt7632u and mt7612u chipsets.

In particular, the driver supports several USB dongles such as Netgear-A6210,
ASUS USB-AC55, ASUS USB-N53 and EDUP EP-AC1601.

Linux kernel version up to 5.7.19 has been tested.
Recommend to use systemd-networkd instead of NetworkManager, if you encounter some bugs, such as can not be used sometimes after reboot or re-plug.
You can refer to these three articles for configuration to systemd-networkd:
1. [this article @zhihu.com](https://zhuanlan.zhihu.com/p/19770401), it was wrote in Chinese.
2. [Systemd-network @archwiki](https://wiki.archlinux.org/index.php/Systemd-networkd#Interface_and_desktop_integration) 
3. [Systemd-resolved @archwiki](https://wiki.archlinux.org/index.php/Systemd-resolved#DNS). 

To build the driver, follow these steps:

    $ git clone https://github.com/Netgear-A6210-linux-driver/Netgear-A6210.git
    $ cd Netgear-A6210
    $ make
    $ sudo make install

The driver is mostly tested on 64 bit Ubuntu 15.10 and Debian 8.3 with NETGEAR AC1200
High Gain Wifi USB Adapter. Some other distro/dongle combinations work as well, for
example Linux Mint 17.3 "Rosa" - KDE (32-bit)/ASUS USB-N53 seems to work flawlessly
(as reported by Roland Bauer). I have tested it up kernel version 5.0.5 with Arch Linux
x86_64 and Arch Linux ARMv7 (Raspberry Pi 2 Model B).

The supported chipsets can be present in other devices. To include additional
devices, you need to add corresponding VendorID, DeviceID into the file
"rtusb_dev_id.c"

The original code was downloaded from:
http://cdn-cw.mediatek.com/Downloads/linux/MT7612U_DPO_LinuxSTA_3.0.0.1_20140718.tar.bz2

The driver provided at this link NO LONGER COMPILES, so do not attempt to use it. I
plan on maintaining this driver so that it continues to compile with the newest kernel
releases and I would like to deobfuscate it, which is a work in progress.

I am working on creating a tarball for usage with Arch Linux and adding it the the AUR,
I may end up adding the package to other distros if requested.

This is work in progress. The driver is functional, however, there are still several
issues that need to be addressed, such as the driver providing extraneous output
(for debugging purposes) to the kernel logs. Also, hot-unplugging may cause the
network manager to become unreliable. After plugging the dongle back in, you may need
to restart the manager:

	$ sudo service network-manager restart
			or
	$ sudo netctl restart <profile>

This seems to be Linux distro dependent, but has definitely been observed on Ubuntu,
I have not yet had any problems with the driver on Arch.

At present, there is no LED support.

EDUP EP-AC1601 works (or to be precise, should work), but at present there are
several problems such as frequent dropping of connection, failure to connect, wildly
oscillating signal strength etc. This "feature" also seems to depend on the Linux distro,
probably as a result of differing kernels, so please use the most up-to-date
installation provided.

## DKMS Install

On Debian-based distros, you can add the module to DKMS so it will automatically
build and install on each successive kernel upgrade. To do this, issue the following
commands from within the repo's folder:

	$ cd ..
	$ sudo mv Netgear-A6210/ /usr/src/netgear-a6210-2.5.0
	$ sudo dkms install netgear-a6210/2.5.0

To remove:

	$ sudo dkms remove netgear-a6210/2.5.0 --all

This process is automated by the install script as well.
