---
title: Basics for Bash CLI Programs
description: Starter snippets for Bash command line interfaces.
category: Linux
tags:
  - cli
  - linux
  - bash
  - snippets
---

## CLI Arguments

Passing arguments or flags is a common way to modify the behaviours of a CLI or
extend some particular functionality.

In this code snippet, upon execution, the script will iterate through everything
right of the shell argument ($0).

We have facilitated a `name` and `age` argument.

```shell
./my-cli --name "Keanu Reeves" --age 142
```

`vim my-cli`

```shell
#!/bin/bash

POS_ARGS=()

# iterate all of the space separated arguments
while [[ $# -gt 0 ]]; do
    case $1 in
        # single dash and double dash is a shorthand and longhand names for a key: value argument
        -n|--name)
            name="$2"
            shift # continue pass the argument key
            shift # continue pass the argument value
            ;;
        -a|--age)
            age="$2"
            shift # continue pass the argument key
            shift # continue pass the argument value
            ;;
        -*|--*)
            echo "This key is not an accepted argument $1"
            exit 1
            ;;
        *)
            POS_ARGS+=("$1") # save positional arg
            shift # continue pass the argument value
            ;;
        # esac is to case what fi is to if ;) https://en.wikipedia.org/wiki/ALGOL_68
    esac
done

echo ${name}
echo ${age}
```

## Basic Animation

To add a little spice to your CLIs during synchronous operations.

The following creates a little alternating animation between sleep statements.

```shell
#!/usr/bin/env bash

while true; do
    printf "\r | waiting ..."
    sleep 0.25
    printf "\r - waiting ..."
    sleep 0.25
done
```
