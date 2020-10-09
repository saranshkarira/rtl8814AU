# rtl8814AU

Realtek 8814AU USB WiFi driver.

Note from Saransh :  This is a fork of [Thomas Pircher](https://github.com/tpircher/rtl8814AU) repository. From here on, I'll be developing and maintaining the driver for the coming linux kernel versions. Thanks Thomas for your work and giving me cliffnotes for the future.

Updated with support for kernels >= 4.14 and < 5.3.

# Manual build

When building the code, you need to point `make` to the kernel source and
specify the kernel version.
```shell
make install KSRC=/path/to/linux-source KVER=x.y.x
```

This script automates this:
```shell
#!/bin/sh
set -e

src=/usr/src/linux
suffix=$(sed -ne 's/^CONFIG_LOCALVERSION="\(.*\)"/\1/p' $src/.config)

ver=$(cd $src; make kernelversion)$suffix
make -j clean all KSRC=$src KVER=$ver
sudo make install KSRC=$src KVER=$ver
```

# DKMS support

From your src dir

````shell
sudo cp -R . /usr/src/rtl8814au-4.3.21
sudo dkms build -m rtl8814au -v 4.3.21
sudo dkms install -m rtl8814au -v 4.3.21
````

This should keep your 8814AU adapter working post kernel updates.
