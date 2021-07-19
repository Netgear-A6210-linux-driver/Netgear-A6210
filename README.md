# Netgear-A6210

This driver supports Ralink / Mediatek mt766u, mt7632u and mt7612u chipsets.

In particular, the driver supports several USB dongles such as Netgear-A6210,
ASUS USB-AC55, ASUS USB-N53 and EDUP EP-AC1601.

Linux kernel version up to 5.12.15 has been tested.

## Building

To build the driver, follow these steps:

    $ git clone https://github.com/Netgear-A6210-linux-driver/Netgear-A6210.git
    $ cd Netgear-A6210
    $ make
    $ sudo make install

The supported chipsets can be present in other devices. To include additional
devices, you need to add corresponding VendorID, DeviceID into the file
`rtusb_dev_id.c`

## Source

The original code was downloaded from:
`http://cdn-cw.mediatek.com/Downloads/linux/MT7612U_DPO_LinuxSTA_3.0.0.1_20140718.tar.bz2`
But the code provided there does no longer compile.

## Issues

This is work in progress. The driver is functional, however, there are still several
issues that need to be addressed, such as the driver providing extraneous output
(for debugging purposes) to the kernel logs. Also, hot-unplugging may cause the
network manager to become unreliable. After plugging the dongle back in, you may need
to restart the manager:

	$ sudo service network-manager restart
			or
	$ sudo netctl restart <profile>

Alternatively, you may try using systemd-networkd instead of NetworkManager.
You can refer to these articles for configuration of systemd-networkd:
* [this article @zhihu.com](https://zhuanlan.zhihu.com/p/19770401) (article written in Chinese).
* [Systemd-network @archwiki](https://wiki.archlinux.org/index.php/Systemd-networkd#Interface_and_desktop_integration)
* [Systemd-resolved @archwiki](https://wiki.archlinux.org/index.php/Systemd-resolved#DNS).

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
