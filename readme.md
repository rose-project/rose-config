# R**OS**E Build Configuration

Sets up a workspace for building the R**OS**E reference distribution using the [OpenEmbedded](https://www.openembedded.org) build system. 

## License

These scripts and configuration files are distributed unter the [MIT License](license).

## System Requirements

For general system requirements, please refer to the [Yocto Project documentation](https://docs.yoctoproject.org/ref-manual/system-requirements.html). 

The following packages are needed in addition:
```
python3-venv
```

## Users Guide

The following steps show, how a ROSE image can be built for the Raspberry Pi 4.

### Fetching the source

```
mkdir rose
cd rose
python3 -m venv .venv
. .venv/bin/activate
pip install west
west init -m https://github.com/rose-project/rose-config
west update
```

### Build

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

### Flashing an SD-Card

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

### Change ROSE Revision

The west tool will manage the revision of the sources to fetch using its manifest file in the subdirectory `config`. This subderectory contains the `rose-config` git repository. To change to a different revision of ROSE, you change configuration by fetching a different revision of `rose-config` and then calling west `update again`. 

