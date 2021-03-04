# Packages
This repository contains developer packages which are used within turris project.

## Using in gitlab-CI
To integrate it into your project you need to define two stages `build` and `publish`.
Then you can simply include the remote yaml file provided within this repository.

```yaml
stages:
  ...
  - build
  - publish

...

include:
  - remote: "https://gitlab.nic.cz/turris/foris-controller/packages/-/raw/master/templates/python.yml"
```

Building and publishing trigger only when a version tag occurs (`^v[0-9].*`) and
it requires that all previous stages passes.

## Overriding template configuration
Sometimes it is necessary to override some configuration within the provided template. Lets say that we want to build only for python 3.7 and 3.8.

```yaml
...

include:
  - remote: "https://gitlab.nic.cz/turris/foris-controller/packages/-/raw/master/templates/python.yml"

build::python:
  parallel:
    matrix:
      - IMAGE: ['python:3.7-slim', 'python:3.8-slim']

```

## How to bend your pip to use packages from the registry

You can either use a special cmd line attr for pip
```bash
pip install <package_name> --extra-index-url https://gitlab.nic.cz/api/v4/projects/1066/packages/pypi/simple
```
or you can place it into your pip configuration (`~/.config/pip/pip.conf`)
```ini
[global]
extra-index-url =
	https://gitlab.nic.cz/api/v4/projects/1066/packages/pypi/simple
```
