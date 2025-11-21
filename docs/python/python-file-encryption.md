---
title: Python File Encryption
description:
  Encrypt and decrypt local files on your file system using the python
  cryptography library
category: Python
tags:
  - python
  - cryptography
  - security
---

## Purpose

Sure, you may use one of the many, many file encryption offerings out there.
[GPG Tools](https://gnupg.org/) for example, is a reliable and brilliant tool.
Most Linux users will be familiar with GPG to some degree. You should not
reinvent the wheel here.

However, it is useful to understand some basic principles around cryptographic
operations. Surface level education about how tools work helps to broker a
deeper sense of trust with tools and the behaviours they exhibit to users.

The following is a very rudimentary python script to encrypt and decrypt files
using Fernet.

It accepts three arguments `encrypt | decrypt`, `path to file` and `key`.

To ensure the script can run, one just needs to install `cryptography`.

Usage:

```shell
echo "I am plaintext" > /tmp/test.txt
python -m venv venv
source venv/bin/activate
pip install cryptography
python fsecure.py encrypt /tmp/test.txt "12345"
cat /tmp/test.txt
# > will no longer be plaintext !

```

Outputs:

`/tmp/test.txt.fsc`

## Script

```python
import sys
import os
import base64

from cryptography.fernet import Fernet
from cryptography.hazmat.primitives import hashes
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC

# extract the args from the command @
_, mode, file, key = sys.argv

salt = b"salt-bae"
# https://www.ietf.org/rfc/rfc2898.txt
kdf = PBKDF2HMAC(
    # using SHA256 hashing algo
    algorithm=hashes.SHA256(),
    length=32,
    salt=salt,
    iterations=390000,
)

# create a KDF derivative so we don't need to have a 32 char padded expression
crypto_key = base64.urlsafe_b64encode(kdf.derive(key.encode()))
file_bytes = open(file, "rb").read()
# create the bi-directional fernet instance, en|decrypt use the same key. Note:
# this is known as symmetric encryption
fernet = Fernet(crypto_key)

if mode == "decrypt":
    plaintext = fernet.decrypt(file_bytes)
    plaintext_file = ".".join(file.split(".")[:-1])
    open(plaintext_file, "wb+").write(plaintext)
    os.remove(file)
elif mode == "encrypt":
    open(f"{file}.fsc", "wb+").write(fernet.encrypt(file_bytes))
    os.remove(file)
else:
    raise ValueError(f"Unrecognized mode {mode}")
```

## Additional Installation

For this quick utility, I have located this script with a bash helper. This will
dependent how your PATH is setup. My .zshrc adds `.scripts` to path.

```shell
export PATH=$HOME/.scripts:$PATH
```

This is entirely down to your own configuration however.

> `~/.scripts/fsecure`

```shell
#!/bin/bash
source ~/.scripts/.venvs/fsecure/bin/activate
python ~/.scripts/fsecure.py "$@"
```

> `~/.scripts/fsecure.py`

Above script located here.

This then allows the utility to be used like any other command:

`fsecure encrypt /tmp/test.txt "12345"`
