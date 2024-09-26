# R**OS**E Build Configuration

Sets up a workspace for building the R**OS**E reference distribution using the [OpenEmbedded](https://www.openembedded.org) build system. 

## License

These scripts and configuration files are distributed unter the [MIT License](license).

## User Guide

The following steps show, how a ROSE image can be built for the Raspberry Pi 4.

---
**Dependencies**

For information on system reququirements and packages to be installed, please refer to the [Yocto Project documentation](https://docs.yoctoproject.org/ref-manual/system-requirements.html).

The following packages are needed in addition:
```
python3-venv
```

---
**Fetching the source**

```
mkdir rose
cd rose
python3 -m venv .venv
. .venv/bin/activate
pip install west
west init -m https://github.com/rose-project/rose-config
west update
```

---
**Build**

```
cd rose
. config/build-env
```

This sets up the build environment and switches to the build dir. If the build dir does not yet exist, if also initializes build configuration files in build/conf.


To run a build simply call:
```
bitbake rose-image
```

After successful built, there should now be an image file `tmp-glibc/deploy/images/raspberrypi4/rose-image-raspberrypi4.rootfs.wic.bz2`.

**NOTE**

In case you are running Ubuntu 24 or later, you might see an permission error. As a workaround you can run this command with root rights: "echo 0 > /proc/sys/kernel/apparmor_restrict_unprivileged_userns". For details see: https://lists.yoctoproject.org/g/yocto/topic/workaround_for_uid_map_error/106192359


---
**Flashing an SD-Card**

The image can be copied to an SD-Card e.g. using bzcat: 

WARNING: Make sure to use the right block device e.g. with `lsblk`. You will overwrite data somewhere else on your computer otherwise. 

```
sudo -s
bzcat tmp-glibc/deploy/images/raspberrypi4/rose-image-raspberrypi4.rootfs.wic.bz2 > /dev/sdX
sync
exit
```

Now plug the SD card to your Raspberry Pi and power it up. If you connect a monitor via HDMI you should get a console where you can run e.g. the `kmscube` demo application.

Login user is root. 

A serial console is available through the Rapsberry Pi IO header. 

---
**Change ROSE Revision**

The west tool will manage the revision of the sources to fetch using its manifest file in the subdirectory `config`. This subderectory contains the `rose-config` git repository. To change to a different revision of ROSE, you change configuration by fetching a different revision of `rose-config` and then calling west `update again`. 

