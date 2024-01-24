# UK EO Data Hub Platform: template repository

This serves as a template repository for new UKEODHP Python-base software components.

When using this template remember to:
* Edit `pyproject.toml` and set everything marked `CHANGEME`
* Update the dependencies as described below.
* Create a Docker repo in AWS (https://eu-west-2.console.aws.amazon.com/ecr/create-repository?region=eu-west-2 for
  the Telespazio UKEODHP repos) and update `Makefile` with the name of the Docker image to build.
* Consider adding `dockerrun` and/or `run` targets to the `Makefile`.
* Add a description here of how to do local development.

# Development of this component

## Getting started

You will need Python 3.11. On Debian you may need:
* `sudo add-apt-repository -y 'deb http://ppa.launchpad.net/deadsnakes/ppa/ubuntu focal main'` (or `jammy` in place of `focal` for later Debian)
* `sudo apt update`
* `sudo apt install python3.11 python3.11-venv`

and on Ubuntu you may need
* `sudo add-apt-repository -y 'ppa:deadsnakes/ppa'`
* `sudo apt update`
* `sudo apt install python3.11 python3.11-venv`

To prepare running it:

* `virtualenv venv -p python3.11`
* `. venv/bin/activate`
* `rehash`
* `python -m ensurepip -U`
* `pip3 install -r requirements.txt`
* `pip3 install -r requirements-dev.txt`

You should also configure your IDE to use black so that code is automatically reformatted on save.

## Building and testing

This component uses `pytest` tests and the `ruff` and `black` linters. `black` will reformat your code in an
opinionated way.

A number of `make` targets are defined:
* `make test`: run tests continuously
* `make testonce`: run tests once
* `make lint`: lint and reformat
* `make dockerbuild`: build a `latest` Docker image (use `make dockerbuild `VERSION=1.2.3` for a release image)
* `make dockerpush`: push a `latest` Docker image (again, you can add `VERSION=1.2.3`) - normally this should be done
  only via the build system and its GitHub actions.

## Managing requirements

Requirements are specified in `pyproject.toml`, with development requirements listed separately. Specify version
constraints as necessary but not specific versions. After changing them:

* Run `pip-compile` (or `pip-compile -U` to upgrade requirements within constraints) to regenerate `requirements.txt`
* Run `pip-compile --extra dev -o requirements-dev.txt` (again, add `-U` to upgrade) to regenerate
  `requirements-dev.txt`.
* Run the `pip3 install -r requirements.txt` and `pip3 install -r requirements-dev.txt` commands again and test.
* Commit these files.

If you see the error

```commandline
Backend subprocess exited when trying to invoke get_requires_for_build_wheel
Failed to parse /.../template-python/pyproject.toml
```

then install and run `validate-pyproject pyproject.toml` and/or `pip3 install .` to check its syntax.

To check for vulnerable dependencies, run `pip-audit`.
