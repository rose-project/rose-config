# R**OS**E Build Configuration

Sets up a workspace for building the R**OS**E reference distribution using the [OpenEmbedded](https://www.openembedded.org) build system. 

## License

These scripts and configuration files are distributed unter the [MIT License](license).

## Building an Image

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


## Building the SDK

You can build an SDK package that does includes a toolchain, headers and libraries for cross compiling. To build the SDK, first source the environment and then run the Bitbake task populate_sdk:

```
bitbake -c populate_sdk rose-image
```

This will generate a self extracting installer script in the build directory, e.g. `tmp-glibc/deploy/sdk/oecore-rose-image-x86_64-cortexa72-raspberrypi4-toolchain-0.1.sh`

The SDK works independent of Bitbake and can be installed to any location on the system.

```
./oecore-rose-image-x86_64-cortexa72-raspberrypi4-toolchain-0.1.sh
ROSE SDK installer version 0.1
==============================
Enter target directory for SDK (default: /usr/local/oecore-x86_64): ~/Projects/ROSE/sdk
You are about to install the SDK to "/home/<USER>/Projects/ROSE/sdk". Proceed [Y/n]? Y
Extracting SDK...................................................................................................done
Setting it up...done
SDK has been successfully set up and is ready to be used.
Each time you wish to use the SDK in a new shell session, you need to source the environment setup script e.g.
 $ . /home/<USER>/Projects/ROSE/sdk/environment-setup-cortexa72-oe-linux
```

After extracting the SDK, an environment script can be sourced to set up the build environment:

```
. /home/<USER>/Projects/ROSE/sdk/environment-setup-cortexa72-oe-linux
echo $CC
aarch64-oe-linux-gcc -mcpu=cortex-a72+crc -mbranch-protection=standard --sysroot=/home/<USER>/Projects/ROSE/sdk/sysroots/cortexa72-oe-linux
```
