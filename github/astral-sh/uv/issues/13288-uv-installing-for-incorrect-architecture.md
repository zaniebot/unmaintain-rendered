---
number: 13288
title: uv installing for incorrect architecture (raspberry pi)
type: issue
state: open
author: jonathancyu
labels:
  - needs-mre
assignees: []
created_at: 2025-05-04T23:33:11Z
updated_at: 2025-06-10T20:50:36Z
url: https://github.com/astral-sh/uv/issues/13288
synced_at: 2026-01-07T13:12:18-06:00
---

# uv installing for incorrect architecture (raspberry pi)

---

_Issue opened by @jonathancyu on 2025-05-04 23:33_

### Summary

Running uv with any python version that is not 3.9 (system default) will result in a "Cannot open shared object file: No such file or directory" error.

Here are some logs that demonstrate the issue:

```
yucjo@raspberrypi ~/workspace/projects/ main
❯ uv -v run --python 3.9 python --version
DEBUG uv 0.7.2
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.9 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/yucjo/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.10-linux-armv7-gnueabi`
DEBUG Skipping incompatible managed installation `cpython-3.11.12-linux-armv7-gnueabi`
DEBUG Skipping incompatible managed installation `cpython-3.10.17-linux-armv7-gnueabi`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python3.9` (first executable in the search path)
DEBUG Using Python 3.9.2 interpreter at: /usr/bin/python3.9
DEBUG Running `python --version`
DEBUG Spawned child 20442 in process group 20439
Python 3.9.2
DEBUG Command exited with code: 0

yucjo@raspberrypi ~/workspace/projects/ main
❯ uv -v run --python 3.10 python --version
DEBUG uv 0.7.2
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.10 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/yucjo/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.10-linux-armv7-gnueabi`
DEBUG Skipping incompatible managed installation `cpython-3.11.12-linux-armv7-gnueabi`
DEBUG Found managed installation `cpython-3.10.17-linux-armv7-gnueabi`
DEBUG Failed to inspect Python interpreter from managed installations at `/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10`
DEBUG Skipping bad interpreter at /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10 from managed installations: Querying Python at `/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10` failed with exit status exit status: 127

[stderr]
/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10: error while loading shared libraries: /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0: cannot open shared object file: No such file or directory

DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.10`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.10`
DEBUG Skipping bad interpreter at /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10 from managed installations: Querying Python at `/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10` failed with exit status exit status: 127

[stderr]
/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10: error while loading shared libraries: /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0: cannot open shared object file: No such file or directory

DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/yucjo/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Released lock at `/home/yucjo/.local/share/uv/python/.lock`
error: Querying Python at `/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10` failed with exit status exit status: 127

[stderr]
/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10: error while loading shared libraries: /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0: cannot open shared object file: No such file or directory
```
The libraries are clearly present:
```
yucjo@raspberrypi ~/workspace/projects/ main
❯ l  /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0
-rwxr-xr-x 1 yucjo yucjo 21M May  5 00:29 /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0
```

One thing I'm noticing, is that this is an aarch64 machine, however uv is downloading the armv7 executables.
Running python 3.13 will download the armv7 version, yielding the same error:
```
yucjo@raspberrypi ~/workspace/projects/ main
❮ uv -v run --python 3.13 python --version
DEBUG uv 0.7.2
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.13 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `/home/yucjo/.local/share/uv/python`
DEBUG Skipping incompatible managed installation `cpython-3.12.10-linux-armv7-gnueabi`
DEBUG Skipping incompatible managed installation `cpython-3.11.12-linux-armv7-gnueabi`
DEBUG Skipping incompatible managed installation `cpython-3.10.17-linux-armv7-gnueabi`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.13`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.13`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `/home/yucjo/.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.13.3%2B20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.13.3-20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz to temporary location: /home/yucjo/.local/share/uv/python/.temp/.tmpZnA1Xt
Downloading cpython-3.13.3-linux-armv7-gnueabi (download) (17.0MiB)
 Downloading cpython-3.13.3-linux-armv7-gnueabi (download)
DEBUG Moving /home/yucjo/.local/share/uv/python/.temp/.tmpZnA1Xt/python to /home/yucjo/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi
DEBUG Released lock at `/home/yucjo/.local/share/uv/python/.lock`
error: Querying Python at `/home/yucjo/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi/bin/python3.13` failed with exit status exit status: 127

[stderr]
/home/yucjo/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi/bin/python3.13: error while loading shared libraries: /home/yucjo/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi/bin/../lib/libpython3.13.so.1.0: cannot open shared object file: No such file or directory
```

### Platform

Raspbian GNU/Linux 11 (bullseye) aarch64

### Version

uv 0.7.2

### Python version

3.12.10

---

_Label `bug` added by @jonathancyu on 2025-05-04 23:33_

---

_Renamed from "Cannot open shared object file: No such file or directory" to "uv installing for incorrect architecture (raspberry pi)" by @jonathancyu on 2025-05-04 23:35_

---

_Label `bug` removed by @konstin on 2025-05-05 10:26_

---

_Label `needs-mre` added by @konstin on 2025-05-05 10:26_

---

_Comment by @konstin on 2025-05-05 10:27_

I can't reproduce this on my raspberry pi, running the following commands results in the aarch64 Python 3.10 being download and run:

```
uv python install cpython-3.10.17-linux-armv7-gnueabi
uv -v run --python 3.10 python
```

Can you try `uv python uninstall -a` and `uv cache clean` to see if that helps? Otherwise, can you share commands to reproduce that state?

---

_Comment by @jonathancyu on 2025-05-05 16:10_

Doesn't look like -a is a valid argument to uninstall, what version of uv are you on?
```
yucjo@raspberrypi ~
❯ uv python uninstall -a
error: unexpected argument '-a' found

  tip: to pass '-a' as a value, use '-- -a'

Usage: uv python uninstall [OPTIONS] <TARGETS>...

For more information, try '--help'.
```

---

_Comment by @charliermarsh on 2025-05-05 16:11_

I think he meant `--all`.

---

_Comment by @jonathancyu on 2025-05-05 16:16_

Good catch, still running into the same issue:
```
yucjo@raspberrypi ~
❯ uv python uninstall --all
Searching for Python installations
Uninstalled 4 versions in 668ms
 - cpython-3.10.17-linux-armv7-gnueabi
 - cpython-3.11.12-linux-armv7-gnueabi
 - cpython-3.12.10-linux-armv7-gnueabi
 - cpython-3.13.3-linux-armv7-gnueabi

yucjo@raspberrypi ~
❯ uv cache clean
Clearing cache at: .cache/uv
Removed 6 files (1.6KiB)

yucjo@raspberrypi ~
❯ uv -v run --python 3.10 python          
DEBUG uv 0.7.2
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python 3.10 in virtual environments, managed installations, or search path
DEBUG Searching for managed installations at `.local/share/uv/python`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python3` (first executable in the search path)
DEBUG Skipping interpreter at `/usr/bin/python3` from first executable in the search path: does not satisfy request `3.10`
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python` (search path)
DEBUG Skipping interpreter at `/usr/bin/python` from search path: does not satisfy request `3.10`
DEBUG Requested Python not found, checking for available download...
DEBUG Acquired lock for `.local/share/uv/python`
DEBUG Using request timeout of 30s
INFO Fetching requested Python...
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.10.17%2B20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.10.17-20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz to temporary location: /home/yucjo/.local/share/uv/python/.temp/.tmpWgba78
Downloading cpython-3.10.17-linux-armv7-gnueabi (download) (18.9MiB)
 Downloading cpython-3.10.17-linux-armv7-gnueabi (download)
DEBUG Moving /home/yucjo/.local/share/uv/python/.temp/.tmpWgba78/python to .local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi
DEBUG Released lock at `/home/yucjo/.local/share/uv/python/.lock`
error: Querying Python at `/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10` failed with exit status exit status: 127

[stderr]
/home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/python3.10: error while loading shared libraries: /home/yucjo/.local/share/uv/python/cpython-3.10.17-linux-armv7-gnueabi/bin/../lib/libpython3.10.so.1.0: cannot open shared object file: No such file or directory
```

It says it's finding cypthon armv7 at `/usr/bin/python3`, but the interpreter disagrees:
```
yucjo@raspberrypi ~ 42s
❯ which python3
/usr/bin/python3

yucjo@raspberrypi ~
❯ python3                                                  
Python 3.9.2 (default, Mar 20 2025, 22:21:41) 
[GCC 10.2.1 20210110] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import platform
>>> print(platform.machine())
aarch64
>>> print(platform.architecture())
('32bit', 'ELF')
```

Am I misunderstanding what the following line means?
```
DEBUG Found `cpython-3.9.2-linux-armv7-gnu` at `/usr/bin/python3` (first executable in the search path)
```

---

_Comment by @konstin on 2025-05-05 19:29_

That's an odd combination:
```
>>> print(platform.machine())
aarch64
>>> print(platform.architecture())
('32bit', 'ELF')
```

Can you share the output of the following two commands?

```
uname -a
file $(readlink -f $(which python3))
```

---

_Comment by @jonathancyu on 2025-05-05 19:30_

Here you go
```
yucjo@raspberrypi ~
❯ uname -a                      
Linux raspberrypi 6.1.21-v8+ #1642 SMP PREEMPT Mon Apr  3 17:24:16 BST 2023 aarch64 GNU/Linux

yucjo@raspberrypi ~
❯ file $(readlink -f $(which python3))
/usr/bin/python3.9: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, BuildID[sha1]=184698d7bc1f75bdc21aca0938ae29b6f724e227, for GNU/Linux 3.2.0, stripped
```

---

_Comment by @konstin on 2025-05-05 19:58_

Can you check if you installed uv as 32-bit or 64-bit?

```
file $(which uv)
```

Did you install any multiarch packages? When I manually force an armv7 Python installation, the interpreter doesn't not launch for me, which should be earlier than the error about the missing shared library:

```
~/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi/bin/python3.13
-bash: /home/konsti/.local/share/uv/python/cpython-3.13.3-linux-armv7-gnueabi/bin/python3.13: cannot execute: required file not found
```

---

_Comment by @jonathancyu on 2025-05-06 04:08_

It's installed as a 32 bit:
```
yucjo@raspberrypi ~
❯ file $(which uv)
/home/yucjo/.local/bin/uv: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.101, stripped
```

---

_Comment by @konstin on 2025-05-06 08:48_

When downloading the same file I can't run it on my machine, are there any modifications you made to run 32-bit binaries?

```
$ ./uv-armv7-unknown-linux-gnueabihf/uv
-bash: ./uv-armv7-unknown-linux-gnueabihf/uv: cannot execute: required file not found
$ file ./uv-armv7-unknown-linux-gnueabihf/uv
./uv-armv7-unknown-linux-gnueabihf/uv: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.101, stripped
```

---

_Comment by @dirkheinkecc on 2025-05-06 09:00_

We have the same problem with a Pi4 on Raspberry Pi OS (Legacy - bullseye). If you need more details let me know:

```

pi@raspberrypi: $ uname -a
Linux raspberrypi 6.12.16-v8+ #1859 SMP PREEMPT Mon Feb 24 13:14:16 GMT 2025 aarch64 GNU/Linux

pi@raspberrypi: $ file $(readlink -f $(which python3))
/usr/bin/python3.13: ELF 32-bit LSB executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, BuildID[sha1]=262f1bbb4f95658f180197977ded7f920810fad6, for GNU/Linux 3.2.0, with debug_info, not stripped

pi@raspberrypi: $ file $(which uv)
/home/pi/.local/bin/uv: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.101, stripped


$ uv python list
cpython-3.14.0a6-linux-armv7-gnueabi                 <download available>
cpython-3.14.0a6+freethreaded-linux-armv7-gnueabi    <download available>
cpython-3.13.3-linux-armv7-gnueabi                   <download available>
cpython-3.13.3+freethreaded-linux-armv7-gnueabi      <download available>
cpython-3.13.0-linux-armv7-gnu                       /usr/bin/python3.13
cpython-3.13.0-linux-armv7-gnu                       /usr/bin/python3 -> /etc/alternatives/python3
cpython-3.13.0-linux-armv7-gnu                       /usr/bin/python -> /etc/alternatives/python
cpython-3.12.10-linux-armv7-gnueabi                  <download available>
cpython-3.11.12-linux-armv7-gnueabi                  <download available>
cpython-3.10.17-linux-armv7-gnueabi                  <download available>
cpython-3.9.22-linux-armv7-gnueabi                   <download available>
cpython-3.9.2-linux-armv7-gnu                        /usr/bin/python3.9
```

Installing 3.13.3 works, but then trying to create a venv with it, fails.

---

_Comment by @ErichBSchulz on 2025-05-17 02:57_

just a bump on this for fwiw.

I'm on a new pi5 with a fresh install, I'm trying to get faster-whisper running and it's been hours and even grok and gemini can't get me going!


Installed uv using pipx: `pipx install uv`


```
erich@possum001:~/possum/exp/fwhisper $ rm -rf ~/.local/share/uv/python/cpython-3.12.10-linux-armv7-gnueabi
erich@possum001:~/possum/exp/fwhisper $ rm -rf ~/.cache/uv
erich@possum001:~/possum/exp/fwhisper $ uname -m
aarch64
erich@possum001:~/possum/exp/fwhisper $ uv python install 3.12
Installed Python 3.12.10 in 6.97s
 
erich@possum001:~/possum/exp/fwhisper $ uv python list
cpython-3.14.0a6-linux-armv7-gnueabi                 <download available>
cpython-3.14.0a6+freethreaded-linux-armv7-gnueabi    <download available>
cpython-3.13.3-linux-armv7-gnueabi                   <download available>
cpython-3.13.3+freethreaded-linux-armv7-gnueabi      <download available>
cpython-3.12.10-linux-armv7-gnueabi                  <download available>
cpython-3.11.12-linux-armv7-gnueabi                  <download available>
cpython-3.11.2-linux-armv7-gnu                       /usr/bin/python3.11
cpython-3.11.2-linux-armv7-gnu                       /usr/bin/python3 -> python3.11
cpython-3.11.2-linux-armv7-gnu                       /usr/bin/python -> python3
cpython-3.10.17-linux-armv7-gnueabi                  <download available>
cpython-3.9.22-linux-armv7-gnueabi                   <download available>
```


>>> import platform
>>> print(platform.architecture()) 
('32bit', 'ELF')
>>> exit




---

_Comment by @zanieb on 2025-05-17 03:36_

Can you share `uv python install 3.12 -v` and `uv python list 3.12 -v`?

---

_Comment by @ErichBSchulz on 2025-05-17 03:44_

sure

```
erich@possum001:~/possum/exp/fwhisper $ uv python install 3.12 -v
DEBUG uv 0.7.4
DEBUG Acquired lock for `/home/erich/.local/share/uv/python`
DEBUG No installation found for request `Python 3.12`
DEBUG Found download `cpython-3.12.10-linux-armv7-gnueabi` for request `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Downloading https://github.com/astral-sh/python-build-standalone/releases/download/20250409/cpython-3.12.10%2B20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz
DEBUG Extracting cpython-3.12.10-20250409-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz to temporary location: /home/erich/.local/share/uv/python/.temp/.tmp1qCMnj
Downloading cpython-3.12.10-linux-armv7-gnueabi (download) (17.2MiB)
 Downloading cpython-3.12.10-linux-armv7-gnueabi (download)
DEBUG Moving /home/erich/.local/share/uv/python/.temp/.tmp1qCMnj/python to /home/erich/.local/share/uv/python/cpython-3.12.10-linux-armv7-gnueabi
DEBUG Skipping installation of Python executables, use `--preview` to enable.
Installed Python 3.12.10 in 4.55s
 + cpython-3.12.10-linux-armv7-gnueabi
DEBUG Released lock at `/home/erich/.local/share/uv/python/.lock`
erich@possum001:~/possum/exp/fwhisper $ uv python list 3.12 -v
DEBUG uv 0.7.4
DEBUG Searching for Python 3.12 in managed installations or search path
DEBUG Searching for managed installations at `/home/erich/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.10-linux-armv7-gnueabi`
DEBUG Failed to inspect Python interpreter from managed installations at `/home/erich/.local/share/uv/python/cpython-3.12.10-linux-armv7-gnueabi/bin/python3.12` 
error: Failed to inspect Python interpreter from managed installations at `/home/erich/.local/share/uv/python/cpython-3.12.10-linux-armv7-gnueabi/bin/python3.12`
  Caused by: Failed to query Python interpreter at `/home/erich/.local/share/uv/python/cpython-3.12.10-linux-armv7-gnueabi/bin/python3.12`
  Caused by: No such file or directory (os error 2)
```



---

_Comment by @ErichBSchulz on 2025-05-17 03:45_

that was very quick - I'm happy to work with you to fix and debug this now if you're available - otherwise I was planning on using venv to try to get there.

---

_Comment by @ErichBSchulz on 2025-05-17 04:31_

Doh! I think I've discovered my problem!! I downloaded the OS package at the top of this page:

https://www.raspberrypi.com/software/operating-systems/

2025-05-13-raspios-bookworm-armhf-full.img to be specific, which is 32 bit (I see now).

What I wanted (64 bit) was further down the page.

2025-05-13-raspios-bookworm-arm64

I hope this saves others the grief I've had today!







---

_Label `needs-mre` removed by @konstin on 2025-05-19 08:00_

---

_Label `needs-mre` added by @konstin on 2025-05-19 08:01_

---

_Comment by @jonathancyu on 2025-05-27 02:49_

This was indeed resolved when I followed this and did a fresh install of 64-bit raspbian!
I wish there were a better fix than going nuclear on the OS and re-installing though.

> Doh! I think I've discovered my problem!! I downloaded the OS package at the top of this page:
> 
> https://www.raspberrypi.com/software/operating-systems/
> 
> 2025-05-13-raspios-bookworm-armhf-full.img to be specific, which is 32 bit (I see now).
> 
> What I wanted (64 bit) was further down the page.
> 
> 2025-05-13-raspios-bookworm-arm64
> 
> I hope this saves others the grief I've had today!

---

_Closed by @jonathancyu on 2025-05-27 02:49_

---

_Reopened by @geofft on 2025-06-09 21:49_

---

_Comment by @geofft on 2025-06-09 21:50_

There's a [question on Discord](https://discord.com/channels/1039017663004942429/1381692679439519846) from someone who is _intentionally_ running a 32-bit userspace, and ideally it would be good to have this work.

---

_Referenced in [astral-sh/uv#13935](../../astral-sh/uv/issues/13935.md) on 2025-06-09 21:53_

---

_Comment by @kageurufu on 2025-06-09 23:43_

To clarify, Raspbian officially ships a 64-bit kernel with a 32-bit userspace.

I can verify later, but I believe the official 32 bit images still use an aarch64 kernel image unless `use_64bit=0` is added to /boot/config.txt

EDIT: https://github.com/raspberrypi/firmware/issues/1795

> The switch to running a 64-bit kernel by default on Pi 4 is intentional. We believe it gives a better experience with few drawbacks. As you've found, you can revert to the 32-bit kernel by adding `arm_64bit=0` to config.txt.
> 
> See https://forums.raspberrypi.com/viewtopic.php?p=2088935#p2088935 for more details.



---

_Comment by @kageurufu on 2025-06-10 20:50_

More investigation:

- `/proc/sys/kernel/arch` is `aarch64`
- `/proc/cpuinfo` has `Features: fp asimd evtstrm crc32 cpuid`

`std::env::const::ARCH` will be `"arm"`, as it's set at compile time and roughly follows the target tuple.

So something like `if features.contains("vfp") || features.contains("fp")` in https://github.com/astral-sh/uv/blob/90a7208a73a2050fe06ff37db11bbc60a7919ef9/crates/uv-python/src/cpuinfo.rs#L19 should work.

I can test this later today when that machine is free


---
