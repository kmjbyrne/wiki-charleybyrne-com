---
title: Generating Random Passwords With Bash
category: Bash
tags:
  - security
  - passwords
  - bash
---

## Using Date SHA256

```bash
date +%s | sha256sum | base64 | head -c 32 ; echo
```

## Using Dev Random

Where `POPULATION` is the characters permitted in the generated password and
`SIZE` can be a natural number.

```bash
POPULATION="_A-Z-a-z-0-9?!="
SIZE=16
< /dev/urandom tr -dc "${POPULATION}" | head -c ${1:-$SIZE}; echo;
```
