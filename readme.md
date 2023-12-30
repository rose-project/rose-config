# R**OS**E Build Configuration

Sets up a workspace for building the R**OS**E reference distribution using the 

## Dependencies

In case of ubuntu, the following packages have to be installed:

```
python3-venv
```

## Workspace Set-Up

```
python3 -m venv <workdir>/.venv
. <workdir>/.venv/bin/acivate
pip install west
west init -m https://github.com/rose-project/rose-config
cd <workdir>/
west update
```

## Build

```
cd <workdir>
. config/build-env
. layers/openembedded-core/oe-init-build-env
bitbake sd-image
```

## Change Revision

To switch to a different revision of R**OS**E, change to a different revision in the subdirectory `<workspace>`/config` and call `west update` again.

## License

[MIT](LICENSE)


