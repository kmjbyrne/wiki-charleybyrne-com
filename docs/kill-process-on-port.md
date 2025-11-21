---
title: Killing Port Process
category: Linux
tags:
  - linux
  - bash
---

How to kill a process running a particular port in Linux.

## How To

Sometimes when stopping some applications the process will not kill itself
correctly. This can lead to rogue processes consuming a port resource which will
prevent your application stopping/starting/re-running. This can sometimes occur
with some server applications like Tomcat, Python Flask, Python FastAPI or any
scripts that do not exit gracefully.

Example with port 5000:

```shell
# will show the list of processes using port 8080
sudo lsof -t -i:5000

# see the PID and pass the PID to the following command
sudo kill -9 PID
```

Explained:

`lsof` stands for `LiSt Open Files` which is a Linux command to find out which
`files` are open in which processes.

`-t` shows only the process IDs.

`-i` limit the processes to internet specific processes.

`:5000` the port number.

`kill` kills a specific process.

`-9` nuke it!

`PID` is the process ID that you want to kill.
