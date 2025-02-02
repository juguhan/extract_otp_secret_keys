# Extract TOTP/HOTP secret keys from Google Authenticator

[![CI Status](https://github.com/scito/extract_otp_secret_keys/actions/workflows/ci.yml/badge.svg)](https://github.com/scito/extract_otp_secret_keys/actions/workflows/ci.yml)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/protobuf)
[![GitHub Pipenv locked Python version](https://img.shields.io/github/pipenv/locked/python-version/scito/extract_otp_secret_keys)](https://github.com/scito/extract_otp_secret_keys/blob/master/Pipfile.lock)
![protobuf version](https://img.shields.io/badge/protobuf-4.21.5-informational)
[![License](https://img.shields.io/github/license/scito/extract_otp_secret_keys)](https://github.com/scito/extract_otp_secret_keys/blob/master/LICENSE)
[![GitHub tag (latest SemVer)](https://img.shields.io/github/v/tag/scito/extract_otp_secret_keys?sort=semver&label=version)](https://github.com/scito/extract_otp_secret_keys/tags)
[![Stand With Ukraine](https://raw.githubusercontent.com/vshymanskyy/StandWithUkraine/main/badges/StandWithUkraine.svg)](https://stand-with-ukraine.pp.ua)

---

Extract two-factor authentication (2FA, TFA) secret keys from export QR codes of "Google Authenticator" app.
The secret and otp values can be printed and exported to json or csv. The QR codes can be printed or saved as PNG images.

## Usage

1. Export the QR codes from "Google Authenticator" app
2. Read QR codes with QR code reader
3. Save the captured QR codes in a text file. Save each QR code on a new line. (The captured QR codes look like `otpauth-migration://offline?data=...`)
4. Call this script with the file as input:

        python extract_otp_secret_keys.py example_export.txt

## Program help: arguments and options

<pre>usage: extract_otp_secret_keys.py [-h] [--json FILE] [--csv FILE] [--printqr] [--saveqr DIR] [--verbose] [--quiet] infile

positional arguments:
  infile                file or - for stdin (default: -) with "otpauth-migration://..." URLs separated by newlines, lines starting with # are ignored

options:
  -h, --help            show this help message and exit
  --json FILE, -j FILE  export to json file
  --csv FILE, -c FILE   export to csv file
  --printqr, -p         print QR code(s) as text to the terminal (requires qrcode module)
  --saveqr DIR, -s DIR  save QR code(s) as images to the given folder (requires qrcode module)
  --verbose, -v         verbose output
  --quiet, -q           no stdout output</pre>

## Dependencies

    pip install -r requirements.txt

Known to work with

* Python 3.10.6, protobuf 4.21.5, qrcode 7.3.1, and pillow 9.2

For protobuf versions 3.14.0 or similar or Python 3.6, use the extract_otp_secret_keys version 1.4.0.

### Optional

For printing QR codes, the qrcode module is required, otherwise it can be omitted.

    pip install qrcode[pil]

## Technical background

The export QR code of "Google Authenticator" contains the URL `otpauth-migration://offline?data=...`.
The data parameter is a base64 encoded proto3 message (Google Protocol Buffers).

Command for regeneration of Python code from proto3 message definition file (only necessary in case of changes of the proto3 message definition or new protobuf versions):

    protoc --python_out=protobuf_generated_python google_auth.proto

The generated protobuf Python code was generated by protoc 21.5 (https://github.com/protocolbuffers/protobuf/releases/tag/v21.5).

## References

* Proto3 documentation: https://developers.google.com/protocol-buffers/docs/pythontutorial
* Template code: https://github.com/beemdevelopment/Aegis/pull/406

## Alternative installation methods

### pipenv

You can you use [Pipenv](https://github.com/pypa/pipenv) for running extract_otp_secret_keys.

```
pipenv install
pipenv shell
python extract_otp_secret_keys.py example_export.txt
```

### Visual Studio Code Remote - Containers / VSCode devcontainer

You can you use [VSCode devcontainer](https://code.visualstudio.com/docs/remote/containers-tutorial) for running extract_otp_secret_keys.

Requirement: Docker

1. Start VSCode
2. Open extract_otp_secret_keys.code-workspace
3. Open VSCode command palette (Ctrl-Shift-P)
4. Type command "Remote-Containers: Reopen in Container"
5. Open integrated bash terminal in VSCode
6. Execute: python extract_otp_secret_keys.py example_export.txt

### venv

Alternatively, you can use a python virtual env for the dependencies:

    python -m venv venv
    . venv/bin/activate
    pip install -r requirements-dev.txt
    pip install -r requirements.txt

The requirements\*.txt files contain all the dependencies (also the optional ones).
To leave the python virtual env just call `deactivate`.

### devbox

Install [devbox](https://github.com/jetpack-io/devbox), which is a wrapper for nix. Then enter the environment with Python and the packages installed with:

```
devbox shell
```

## Tests

### PyTest

There are basic [pytest](https://pytest.org)s, see `test_extract_otp_secret_keys_pytest.py`.

Run tests:

```
pytest
```
or
```
python -m pytest
```

### unittest

There are basic [unittest](https://docs.python.org/3.10/library/unittest.html)s, see `test_extract_otp_secret_keys_unittest.py`.

Run tests:

```
python -m unittest
```

### VSCode Setup

Setup for running the tests in VSCode.

1. Open VSCode command palette (Ctrl-Shift-P)
2. Type command "Python: Configure Tests"
3. Choose unittest or pytest. (pytest is recommended, both are supported)
4. Set ". Root" directory

***

# #StandWithUkraine 🇺🇦

I have Ukrainian relatives and friends.

#RussiaInvadedUkraine on 24 of February 2022, at 05:00 the armed forces of the Russian Federation attacked Ukraine. Please, stand with Ukraine, stay tuned for updates on Ukraine's official sources and channels in English and support Ukraine in its fight for freedom and democracy in Europe.
