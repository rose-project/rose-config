# R**OS**E Config

Sets up a workspace for building the R**OS**E reference distribution.

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



