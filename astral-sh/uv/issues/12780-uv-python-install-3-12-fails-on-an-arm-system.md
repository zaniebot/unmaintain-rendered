```yaml
number: 12780
title: uv python install 3.12 fails on an arm system, despite its being a supported platform
type: issue
state: closed
author: johndunderhill
labels:
  - bug
  - releases
assignees: []
created_at: 2025-04-09T13:20:40Z
updated_at: 2025-04-17T23:32:58Z
url: https://github.com/astral-sh/uv/issues/12780
synced_at: 2026-01-12T16:01:12Z
```

# uv python install 3.12 fails on an arm system, despite its being a supported platform

---

_@johndunderhill_

### Summary

```
(venv) pi@GKOS49:~ $ curl -LsSf https://astral.sh/uv/install.sh | sh
downloading uv 0.6.13 arm-unknown-linux-musl-staticeabihf
no checksums to verify
installing to /home/pi/.local/bin
  uv
  uvx
everything's installed!

(venv) pi@GKOS49:~ $ uv --version
uv 0.6.13

(venv) pi@GKOS49:~ $ RUST_LOG=trace uv python install 3.12 --verbose
DEBUG uv 0.6.13
TRACE Found `ld` path: /lib/ld-linux-armhf.so.3
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]
You have invoked `ld.so', the helper program for shared library executables.
This program usually lives in the file `/lib/ld.so', and special directives
in executable files using ELF shared libraries tell the system's program
loader to load the helper program from this file.  This helper program loads
the shared libraries needed by the program executable, prepares the program
to run, and runs it.  You may invoke this helper program directly from the
command line to load and run an ELF executable file; this is like executing
that file itself, but always uses this helper program from the file you
specified, instead of the helper program file specified in the executable
file you run.  This is mostly of use for maintainers to test new versions
of this helper program; chances are you did not intend to run this program.

  --list                list all dependencies and how they are resolved
  --verify              verify that given object really is a dynamically linked
                        object we can handle
  --inhibit-cache       Do not use /etc/ld.so.cache
  --library-path PATH   use given PATH instead of content of the environment
                        variable LD_LIBRARY_PATH
  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names
                         in LIST
  --audit LIST          use objects named in LIST as auditors
  --preload LIST        preload objects named in LIST"

TRACE Tried to find musl version by running `"/lib/ld-linux-armhf.so.3"`, but failed:
Could not find musl version in output of: `/lib/ld-linux-armhf.so.3`
error: No download found for request: cpython-3.12-linux-arm-gnueabi

(venv) pi@GKOS49:~ $ uname -rmo
6.1.21-v8+ aarch64 GNU/Linux

(venv) pi@GKOS49:~ $ getconf LONG_BIT
32

(venv) pi@GKOS49:~ $ lscpu
Architecture:                    aarch64
Byte Order:                      Little Endian
CPU(s):                          4
On-line CPU(s) list:             0-3
Thread(s) per core:              1
Core(s) per socket:              4
Socket(s):                       1
Vendor ID:                       ARM
Model:                           3
Model name:                      Cortex-A72
Stepping:                        r0p3
CPU max MHz:                     1800.0000
CPU min MHz:                     600.0000
BogoMIPS:                        108.00
L1d cache:                       128 KiB
L1i cache:                       192 KiB
L2 cache:                        1 MiB
Vulnerability Itlb multihit:     Not affected
Vulnerability L1tf:              Not affected
Vulnerability Mds:               Not affected
Vulnerability Meltdown:          Not affected
Vulnerability Mmio stale data:   Not affected
Vulnerability Retbleed:          Not affected
Vulnerability Spec store bypass: Vulnerable
Vulnerability Spectre v1:        Mitigation; __user pointer sanitization
Vulnerability Spectre v2:        Vulnerable
Vulnerability Srbds:             Not affected
Vulnerability Tsx async abort:   Not affected
Flags:                           fp asimd evtstrm crc32 cpuid

(venv) pi@GKOS49:~ $ cat /etc/os-release
PRETTY_NAME="Raspbian GNU/Linux 11 (bullseye)"
NAME="Raspbian GNU/Linux"
VERSION_ID="11"
VERSION="11 (bullseye)"
VERSION_CODENAME=bullseye
ID=raspbian
ID_LIKE=debian
HOME_URL="http://www.raspbian.org/"
SUPPORT_URL="http://www.raspbian.org/RaspbianForums"
BUG_REPORT_URL="http://www.raspbian.org/RaspbianBugs"
```

HOWEVER:

```
(venv) pi@GKOS49:~ $ uv python install cpython-3.12.8-linux-armv7-gnueabihf
Installed Python 3.12.8 in 3m 39s
 + cpython-3.12.8-linux-armv7-gnueabihf
(venv) pi@GKOS49:~ $ ls ~/.local/share/uv/python/cpython-3.12.8-linux-armv7-gnueabihf/bin/python3
/home/pi/.local/share/uv/python/cpython-3.12.8-linux-armv7-gnueabihf/bin/python3
(venv) pi@GKOS49:~ $ ~/.local/share/uv/python/cpython-3.12.8-linux-armv7-gnueabihf/bin/python3
Python 3.12.8 (main, Jan 14 2025, 22:38:28) [GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 1+1
2
>>>
```


### Platform

Linux 6.1.21-v8+ aarch64 GNU/Linux (in 32-bit mode)

### Version

uv 0.6.13

### Python version

3.12.8

---

_Label `bug` added by @johndunderhill on 2025-04-09 13:20_

---

_Comment by @johndunderhill on 2025-04-09 13:24_

@zanieb per discussion under https://github.com/astral-sh/uv/issues/6873

---

_Comment by @zanieb on 2025-04-09 13:26_

Thank you!

cc @Gankra the installer is downloading `arm-unknown-linux-musl-staticeabihf` but I don't quite know why. It's an ARMv8 system but that binary corresponds to ARMv6 HF.

@johndunderhill Could you try downloading

- https://github.com/astral-sh/uv/releases/download/0.6.13/uv-aarch64-unknown-linux-gnu.tar.gz
- https://github.com/astral-sh/uv/releases/download/0.6.13/uv-aarch64-unknown-linux-musl.tar.gz

and confirm that they work on your system?

---

_Label `releases` added by @zanieb on 2025-04-09 13:26_

---

_Comment by @johndunderhill on 2025-04-09 14:24_

First one _does not_ work, second one does.


```
pi@GKOS49:~/temp/uv1/uv-aarch64-unknown-linux-gnu $ ls -l
total 34132
-rwxr-xr-x 1 pi pi 34605312 Apr  7 14:53 uv
-rwxr-xr-x 1 pi pi   340512 Apr  7 14:53 uvx

pi@GKOS49:~/temp/uv1/uv-aarch64-unknown-linux-gnu $ ./uv
-bash: ./uv: No such file or directory

pi@GKOS49:~/temp/uv1/uv-aarch64-unknown-linux-gnu $ file uv
uv: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 4.10.17, stripped
```

```
pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ ls -l
total 32840
-rwxr-xr-x 1 pi pi 33211992 Apr  7 14:53 uv
-rwxr-xr-x 1 pi pi   413568 Apr  7 14:53 uvx

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ ./uv --version
uv 0.6.13

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ file uv
uv: ELF 64-bit LSB executable, ARM aarch64, version 1 (SYSV), statically linked, stripped
```

The second one may be able to install Python, but I'm not sure if the selected Python will work, because the download is timing out.  It's attempting to download `cpython-3.12.9-linux-aarch64-gnu` in response to `./uv python install 3.12`.  This unit is one of a fleet of machines deployed throughout the country; some have spotty Internet access.  I'll try again later, or see if I can find a similar unit with better access.

John

---

_Comment by @johndunderhill on 2025-04-09 14:38_

Nah, didn't pick the right one.

```
pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ ./uv python install 3.12
Installed Python 3.12.9 in 2m 33s
 + cpython-3.12.9-linux-aarch64-gnu

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ ./uv python list
error: Failed to inspect Python interpreter from managed installations at `/home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12`
  Caused by: Failed to query Python interpreter at `/home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12`
  Caused by: No such file or directory (os error 2)

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ ls /home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12
/home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ /home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12
-bash: /home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12: No such file or directory

pi@GKOS49:~/temp/uv2/uv-aarch64-unknown-linux-musl $ file /home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12
/home/pi/.local/share/uv/python/cpython-3.12.9-linux-aarch64-gnu/bin/python3.12: ELF 64-bit LSB pie executable, ARM aarch64, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-aarch64.so.1, for GNU/Linux 3.7.0, BuildID[sha1]=54c37326ccd9e6e2e10a1c0857b5977aecfdc103, not stripped
```

---

_Comment by @zanieb on 2025-04-09 15:03_

We don't have `aarch64-musl` Python versions yet, it's non-trivial but can be tracked in https://github.com/astral-sh/python-build-standalone/pull/569

---

_Comment by @zanieb on 2025-04-09 15:05_

I'm surprised that the GNU Libc version isn't working, don't pis have GNU Libc installed?

---

_Comment by @Gankra on 2025-04-09 15:13_

I expect the issue is connected to the fact that your aarch64 is "in 32-bit mode".

---

_Comment by @Gankra on 2025-04-09 15:27_

Alright yeah. Specifically at the end of [platform selection (get_architecture)](https://astral.sh/uv/install.sh) we do this (deleted a bunch of irrelevant parts):

```sh
    # Detect 64-bit linux with 32-bit userland
    if [ "${_ostype}" = unknown-linux-gnu ] && [ "${_bitness}" -eq 32 ]; then
        case $_cputype in
            aarch64)
                _cputype=armv7
                _ostype="${_ostype}eabihf"
                ;;
        esac
    fi

    # treat armv7 systems without neon as plain arm
    if [ "$_ostype" = "unknown-linux-gnueabihf" ] && [ "$_cputype" = armv7 ]; then
        if ensure grep '^Features' /proc/cpuinfo | grep -q -v neon; then
            # At least one processor does not have NEON.
            _cputype=arm
        fi
    fi
```

* So at first we correctly identify this is `aarch64-unknown-linux-gnu`. 
* Then "hey wait this is a 32-bit userland... we should use `armv7-unknown-linux-gnueabihf`"
* Then "hey wait we couldn't find universal NEON support... then `arm-unknown-linux-gnueabihf`"
* Then "hey the only thing I have that MAYBE supports that is `uv-arm-unknown-linux-musleabihf`"

So you hit like, 3 levels of "I'm trying my best to make this work" fallback, and, evidently it doesn't really work. Between this and the python fetching I'm not sure we really support your platform, on several levels.

---

_Comment by @Gankra on 2025-04-09 15:38_

These days rustup has the neon check enhanced:

```
if ! (ensure grep '^Features' /proc/cpuinfo | grep -E -q 'neon|simd') ; then
    # Either `/proc/cpuinfo` is malformed or unavailable, or
    # at least one processor does not have NEON (which is asimd on armv8+).
    _cputype=arm
fi
```

Possible that would fix it?

---

_Comment by @Gankra on 2025-04-09 15:39_

Does https://github.com/astral-sh/uv/releases/download/0.6.13/uv-armv7-unknown-linux-gnueabihf.tar.gz work?

---

_Comment by @Gankra on 2025-04-09 15:44_

Filed https://github.com/astral-sh/cargo-dist/issues/15 for potential installer improvements.

---

_Comment by @johndunderhill on 2025-04-10 06:08_

> Does https://github.com/astral-sh/uv/releases/download/0.6.13/uv-armv7-unknown-linux-gnueabihf.tar.gz work?

Yes, it does.

```
(venv) pi@GKOS49:~/temp/uv3/uv-armv7-unknown-linux-gnueabihf $ ./uv --version
uv 0.6.13

(venv) pi@GKOS49:~/temp/uv3/uv-armv7-unknown-linux-gnueabihf $ file uv
uv: ELF 32-bit LSB pie executable, ARM, EABI5 version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux-armhf.so.3, for GNU/Linux 3.2.101, stripped
```


---

_Comment by @johndunderhill on 2025-04-10 06:23_

> We don't have `aarch64-musl` Python versions yet, it's non-trivial but can be tracked in [astral-sh/python-build-standalone#569](https://github.com/astral-sh/python-build-standalone/pull/569)

Ok, but bear in mind, per the initial post, `cpython-3.12.8-linux-armv7-gnueabihf` appears to work on this platform.  That was a guess on my part; I'm not entirely clear about the details, nor if this is even the best choice here.

---

_Closed by @Gankra on 2025-04-17 23:32_

---

_Closed by @Gankra on 2025-04-17 23:32_

---
