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

## Quick Set-Up Guide

The following steps show, how a ROSE image can be built for the Raspberry Pi 4.

```
mkdir rose
cd rose
python3 -m venv .venv
. .venv/bin/acivate
pip install west
west init -m https://github.com/rose-project/rose-config
west update
```

## Build

```
cd rose
. config/build-env
. layers/openembedded-core/oe-init-build-env
bitbake sd-image
```

## Change Revision

To switch to a different revision of R**OS**E, change to a different revision in the subdirectory `<workdir>/config` and call `west update` again.


