```yaml
number: 6873
title: "`uv python install 3.12` is not working on armv7 libc is identify as `gnu` instead of `gnueabihf`"
type: issue
state: closed
author: zarch
labels:
  - bug
  - uv python
assignees: []
created_at: 2024-08-30T14:31:41Z
updated_at: 2025-04-09T12:18:29Z
url: https://github.com/astral-sh/uv/issues/6873
synced_at: 2026-01-12T15:59:08Z
```

# `uv python install 3.12` is not working on armv7 libc is identify as `gnu` instead of `gnueabihf`

---

_@zarch_


I tried uv `0.3.5` to install a specific version of python on my raspberry-pi:

```bash
❯ uname -rmo
6.1.21-v7l+ armv7l GNU/Linux
```

When I try to install python I got:

```bash
❯ uv python install 3.12
Searching for Python versions matching: Python 3.12
error: No download found for request: cpython-3.12-linux-armv7-gnu
```

Looking at the code it seems that all the version available are taken from [uv-python/download-metadata.json](https://github.com/astral-sh/uv/blob/main/crates/uv-python/download-metadata.json), where at the moment we have:

```json
{
  "cpython-3.12.5-linux-armv7-gnueabi": {
    "name": "cpython",
    "arch": "armv7",
    "os": "linux",
    "libc": "gnueabi",
    "major": 3,
    "minor": 12,
    "patch": 5,
    "url": "https://github.com/indygreg/python-build-standalone/releases/download/20240814/cpython-3.12.5%2B20240814-armv7-unknown-linux-gnueabi-install_only_stripped.tar.gz",
    "sha256": "7a584de9c2824f43d7a7b1c26eb61a18af770ebd603a74b45d57601ba62ba508"
  },
  "cpython-3.12.5-linux-armv7-gnueabihf": {
    "name": "cpython",
    "arch": "armv7",
    "os": "linux",
    "libc": "gnueabihf",
    "major": 3,
    "minor": 12,
    "patch": 5,
    "url": "https://github.com/indygreg/python-build-standalone/releases/download/20240814/cpython-3.12.5%2B20240814-armv7-unknown-linux-gnueabihf-install_only_stripped.tar.gz",
    "sha256": "a9992b30d7b3ecb558cd12fde919e3e2836f161f8f777afea31140d5fff6362e"
  }
}
```

Using other tool such as `pystand` they work as expected:

```
❯ uvx pystand show
3.9.19 @ 20240814 distribution="armv7-unknown-linux-gnueabihf"
3.10.14 @ 20240814 distribution="armv7-unknown-linux-gnueabihf"
3.11.9 @ 20240814 distribution="armv7-unknown-linux-gnueabihf"
3.12.5 @ 20240814 distribution="armv7-unknown-linux-gnueabihf"
```

Looking at the code from my understanding `uv` is trying to infer this information from `ldd` while `pystand` look at the [platform](https://github.com/bulletmark/pystand/blob/1594bcc44d3f3195025e9bbc2fed32d925dc5c80/pystand.py#L52 ) in a simpler way:

```python
# Default distributions for various platforms
DISTRIBUTIONS = {
    ('Linux', 'x86_64'): 'x86_64-unknown-linux-gnu',
    ('Linux', 'aarch64'): 'aarch64-unknown-linux-gnu',
    ('Linux', 'armv7l'): 'armv7-unknown-linux-gnueabihf',
    ('Linux', 'armv8l'): 'armv7-unknown-linux-gnueabihf',
    ('Darwin', 'x86_64'): 'x86_64-apple-darwin',
    ('Darwin', 'aarch64'): 'aarch64-apple-darwin',
    ('Windows', 'x86_64'): 'x86_64-pc-windows-msvc',
    ('Windows', 'i686'): 'i686-pc-windows-msvc',
}
```

Perhaps `uv` should use/integrate a similar approach?


---

_Comment by @zarch on 2024-08-30 14:35_

Or at least it would be nice to be able to define / force the right value by command line interface?

---

_Comment by @charliermarsh on 2024-08-30 14:36_

@konstin -- Do you know if this is expected to work yet?

---

_Comment by @konstin on 2024-08-30 17:17_

iirc we have to detect whether we're on a hardfloat abi or not and use `guneabi` or `gnueabihf` depending on that

---

_Comment by @zarch on 2024-09-02 09:19_

If you like I can prepare a PR, but I'm not sure which is the best approach to detect if we are are using hardfloat abi or not.

Executing the code enabling the `RUST_LOG=trace`
```bash
❯ RUST_LOG=trace uv python install 3.12 --verbose
DEBUG uv 0.4.2
TRACE Checking lock for `/root/.local/share/uv/python` at `/root/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/root/.local/share/uv/python`
Searching for Python versions matching: Python 3.12
TRACE ld path: /lib/ld-linux-armhf.so.3
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]\nYou have invoked `ld.so', the helper program for shared library executables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared libraries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a dynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n  --preload LIST        preload objects named in LIST\n"
TRACE Tried to find musl version by running `"/lib/ld-linux-armhf.so.3"`, but failed: Could not find musl version in output of: `/lib/ld-linux-armhf.so.3`
DEBUG Released lock at `/root/.local/share/uv/python/.lock`
error: No download found for request: cpython-3.12-linux-armv7-gnu
```

I see that, in debian, in the `ld` path it is already indicated the is hardfloat: `TRACE ld path: /lib/ld-linux-armhf.so.3`, so we could infer from the path, but I'm not sure it is general enough to cover other case and distributions. I tried few other distributions on my laptop:

- fedora (rawhide): `TRACE ld path: /lib64/ld-linux-x86-64.so.2`
- archlinux (stable): `TRACE ld path: /lib64/ld-linux-x86-64.so.2`
- alpine(edge): `TRACE ld path: /lib/ld-musl-x86-64.so.1`

So it seems to me that this pattern is shared among linux distributions.
Perhaps, I can add a if condition within the `detect_linux_libc()` function that check if the path contains `armhf` string then it return `gnueabihf`, if contains just `arm` then return: `gnueabi` else continue with the other check.

Otherwise, we can simply implement the same logic used by `pystand` and see the platform architecture calling `uname -m`. Other approaches that you would like to follow?

---

_Label `bug` added by @zanieb on 2024-09-03 14:34_

---

_Label `uv python` added by @zanieb on 2024-09-03 14:34_

---

_Comment by @orgua on 2024-09-25 15:11_

I have the same trouble on a armv7 beaglebone black. Latest `uv 0.4.16` won't install any python version, not just 3.12. Would love to be able to use that feature! Armv7 is slow enough and could use every improvement newer py-versions bring :)

---

_Comment by @kakkoyun on 2024-09-26 19:43_

I’ve thought through a few different options for detecting whether to use `gnueabi` or `gnueabihf`. One possibility is analyzing an ELF binary using `readelf` (or Goblin) to check for `Tag_ABI_VFP_args` (see [ARM documentation](https://developer.arm.com/documentation/dui0474/m/linker-command-line-options/--output-float-abi-option)), but I decided to go with a simpler approach: checking `/proc/cpuinfo` (#7725). The idea is to look for the `vfp` flag, which indicates hardware floating-point support. There’s some more info on that [here](https://wiki.debian.org/ArmHardFloatPort#VFP).

I have some Raspberry Pis lying around, but they need to be provisioned first. Could I ask for your help testing #7725, @orgua @zarch? I’d be happy to provide a pre-built version of `uv` from the PR if that’s easier for you!

---

_Comment by @konstin on 2024-10-01 15:35_

(i tried testing this but my raspi is too new and `docker run -it --rm -v $(pwd):/io --platform linux/arm/v7 ubuntu` shows my native cpu)

---

_Comment by @kakkoyun on 2024-10-02 15:46_

> (i tried testing this but my raspi is too new and `docker run -it --rm -v $(pwd):/io --platform linux/arm/v7 ubuntu` shows my native cpu)

Thanks. I'm in the process of reviving some of my old pi's. I'll update the PR when I've something.

---

_Comment by @zarch on 2024-10-03 10:21_

@kakkoyun Sorry for the delay but I have to free up some space to be able to compile `uv` on my rpi... :-D

The current branch is not working for me. I tried:

```bash
uv on  detect_hardfloat is 📦 v0.4.16 via 🐍 via 🦀 v1.81.0 took 3m19s 
❯ target/debug/uv python install 3.12
Searching for Python versions matching: Python 3.12                                                                  
error: Failed to parse Python installation key `cpython-3.12.6-linux-armv7-gnueabi`: invalid libc: Unknown libc envir
onment: gnueabi
```
I'm on:
```bash
uv on  detect_hardfloat is 📦 v0.4.16 via 🐍 via 🦀 v1.81.0 took 10s 
❯ git show
commit fde37914b7c01df2d97c747c9a6a28f1b3bd6dc1 (HEAD -> detect_hardfloat, origin/detect_hardfloat)
Author: Kemal Akkoyun <kakkoyun@gmail.com>
Date:   Fri Sep 27 15:02:32 2024 +0200
commit fde37914b7c01df2d97c747c9a6a28f1b3bd6dc1 (HEAD -> detect_hardfloat, origin/detect_hardfloat)               
Author: Kemal Akkoyun <kakkoyun@gmail.com>
Date:   Fri Sep 27 15:02:32 2024 +0200
[...]
```

Here my `cpuinfo`:

```bash
uv on  detect_hardfloat is 📦 v0.4.16 via 🐍 via 🦀 v1.81.0 took 6s 
❯ cat /proc/cpuinfo
processor       : 0
model name      : ARMv7 Processor rev 3 (v7l)
BogoMIPS        : 108.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 1
model name      : ARMv7 Processor rev 3 (v7l)
BogoMIPS        : 108.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 2
model name      : ARMv7 Processor rev 3 (v7l)
BogoMIPS        : 108.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 3
model name      : ARMv7 Processor rev 3 (v7l)
BogoMIPS        : 108.00
Features        : half thumb fastmult vfp edsp neon vfpv3 tls vfpv4 idiva idivt vfpd32 lpae evtstrm crc32 
CPU implementer : 0x41
CPU architecture: 7
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

Hardware        : BCM2711
Revision        : d03115
Serial          : 100000008cd9439f
Model           : Raspberry Pi 4 Model B Rev 1.5
```

And here the command execution with the full log:
```bash
uv on  detect_hardfloat is 📦 v0.4.16 via 🐍 via 🦀    v1.81.0 
❯ RUST_LOG=trace cargo run python install 3.12 --verbose
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.92s
     Running `target/debug/uv python install 3.12 --verbose`
DEBUG uv 0.4.16
TRACE Checking lock for `/home/zarch/.local/share/uv/python` at `/home/zarch/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/zarch/.local/share/uv/python`
WARN Ignoring malformed managed Python entry:
    Failed to parse Python installation key `cpython-3.12.6-linux-armv7-gnueabi`: invalid libc: Unknown libc environment: gnueabi
Searching for Python versions matching: Python 3.12
TRACE ld path: /lib/ld-linux-armhf.so.3
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]\nYou have invoked `ld.so', the helper program for shared library executables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared libraries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a dynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n  --preload LIST        preload objects named in LIST\n"
TRACE Tried to find musl version by running `"/lib/ld-linux-armhf.so.3"`, but failed: Could not find musl version in output of: `/lib/ld-linux-armhf.so.3`
DEBUG Using request timeout of 30s
DEBUG Released lock at `/home/zarch/.local/share/uv/python/.lock`
error: Failed to parse Python installation key `cpython-3.12.6-linux-armv7-gnueabi`: invalid libc: Unknown libc environment: gnueabi
```

---

_Comment by @kakkoyun on 2024-10-04 12:30_

@zarch Thanks a lot for testing it. These logs are helpful. I will give it another shot.

---

_Comment by @golgor on 2024-10-07 15:13_

This is not recommended, but if it is blocking you a it is possible to install a Python version manually.

It is a bit cumbersome but this worked at least for the RPi 4 I'm using:
1. Download and compile the source (This can be done with Pyenv; `pyenv install 3.12.7`)
2. Find the path to the installed python and copy it to ~/.local/share/uv/python
3. Make sure that the file name is similar to the following: `cpython-3.12.7-linux-armv7l-gnu`

`tree ~/.local/share/uv/python -L 2`
```bash
.
└ cpython-3.12.7-linux-armv7l-gnu
  ├── bin
  ├── include
  ├── lib
  └── share
```

---

_Comment by @zanieb on 2024-10-07 15:31_

You can also request a full download key, e.g., `uv python install cpython-3.12.6-linux-armv7-gnueabihf`, then rename the directory as described above

edit: Actually this fails to parse correctly because the error reported above. i'll fix that. https://github.com/astral-sh/uv/pull/7975

---

_Comment by @zarch on 2024-10-29 23:28_

Dear @kakkoyun and @zanieb 

I've just tested the #8498 applied on #7725 and rebase it to the current main branch (commit: [cd408cbc5](https://github.com/astral-sh/uv/commit/cd408cbc511bb048fb2574c59a3eb90119e987a2)), and it works on my pc.

How would you like to proceed?

```bash
uv on  detect_hardfloat [!⇕] is 📦 v0.4.28 via 🐍 via 🦀 v1.81.0 took 7s 
❯ cargo run python install 3.13
   Compiling uv-python v0.0.1 (/home/zarch/source/uv/crates/uv-python)
   Compiling uv-types v0.0.1 (/home/zarch/source/uv/crates/uv-types)
   Compiling uv-virtualenv v0.0.4 (/home/zarch/source/uv/crates/uv-virtualenv)
   Compiling uv-distribution v0.0.1 (/home/zarch/source/uv/crates/uv-distribution)
   Compiling uv-resolver v0.0.1 (/home/zarch/source/uv/crates/uv-resolver)
   Compiling uv-installer v0.0.1 (/home/zarch/source/uv/crates/uv-installer)
   Compiling uv-build-frontend v0.0.1 (/home/zarch/source/uv/crates/uv-build-frontend)
   Compiling uv-settings v0.0.1 (/home/zarch/source/uv/crates/uv-settings)
   Compiling uv-requirements v0.1.0 (/home/zarch/source/uv/crates/uv-requirements)
   Compiling uv-dispatch v0.0.1 (/home/zarch/source/uv/crates/uv-dispatch)
   Compiling uv-scripts v0.0.1 (/home/zarch/source/uv/crates/uv-scripts)
   Compiling uv-cli v0.0.1 (/home/zarch/source/uv/crates/uv-cli)
   Compiling uv-tool v0.0.1 (/home/zarch/source/uv/crates/uv-tool)
   Compiling uv v0.4.28 (/home/zarch/source/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3m 01s
     Running `target/debug/uv python install 3.13`
Searching for Python versions matching: Python 3.13
Installed Python 3.13.0 in 9.96s
 + cpython-3.13.0-linux-armv7-gnueabihf

uv on  detect_hardfloat [!⇕] is 📦 v0.4.28 via 🐍 via 🦀 v1.81.0 took 3m11s 
❯ /home/zarch/.local/share/uv/python/cpython-3.13.0-linux-armv7-gnueabihf/bin/python3
Python 3.13.0 (main, Oct 16 2024, 02:57:06) [GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 1+1
2
```

---

_Comment by @zanieb on 2024-10-29 23:38_

Sorry I got a bit confused that your branch was on top of the other pull request. I'll clean it up and merge, thanks!

---

_Closed by @zanieb on 2024-10-29 23:55_

---

_Comment by @johndunderhill on 2025-01-03 10:12_

@zanieb This does not seem to work at all.
```bash
pi@GKOS66:~/sensors $ RUST_LOG=trace ${SENSORS_UV_PATH}/uv python install 3.12
DEBUG uv 0.5.14
TRACE ld path: /lib/ld-linux-armhf.so.3
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "Usage: ld.so [OPTION]... EXECUTABLE-FILE [ARGS-FOR-PROGRAM...]\nYou have invoked `ld.so', the helper program for shared library executables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared libraries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a dynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n  --preload LIST        preload objects named in LIST\n"
TRACE Tried to find musl version by running `"/lib/ld-linux-armhf.so.3"`, but failed: Could not find musl version in output of: `/lib/ld-linux-armhf.so.3`
error: No download found for request: cpython-3.12-linux-arm-gnueabi
pi@GKOS66:~/sensors $ ${SENSORS_UV_PATH}/uv --version
uv 0.5.14
pi@GKOS66:~/sensors $ cat /etc/os-release
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
pi@GKOS66:~/sensors $ cat /proc/cpuinfo
processor       : 0
BogoMIPS        : 108.00
Features        : fp asimd evtstrm crc32 cpuid
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 1
BogoMIPS        : 108.00
Features        : fp asimd evtstrm crc32 cpuid
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 2
BogoMIPS        : 108.00
Features        : fp asimd evtstrm crc32 cpuid
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

processor       : 3
BogoMIPS        : 108.00
Features        : fp asimd evtstrm crc32 cpuid
CPU implementer : 0x41
CPU architecture: 8
CPU variant     : 0x0
CPU part        : 0xd08
CPU revision    : 3

Hardware        : BCM2835
Revision        : b03115
Serial          : 100000004403b895
Model           : Raspberry Pi 4 Model B Rev 1.5
```

---

_Comment by @zanieb on 2025-01-03 16:20_

It looks like the issue there is that per `cpython-3.12-linux-arm-gnueabi`, we're detecting `arm` instead of `armv7` and we don't have downloads available for `arm`. 

---

_Comment by @johndunderhill on 2025-04-08 07:26_

@zanieb Ok. We do not really understand this discussion--it's just too technical as it relates to the detection, build and distribution process for Python.  We understand hardware, but we don't understand what you mean. We really like **uv**--it's amazingly fast and easy to use--but we don't know how to help at this point.

We've had to abandon **uv** because of this problem.  We are back to using **pyenv** (and **pip**), which is a huge PITA, but it does work on a Raspberry Pi Model 4 B, which we have hundreds of in the field.  We're very sad about this.  :(

---

_Comment by @orgua on 2025-04-08 08:33_

Don't know about Raspberry Pis, but at least for BeagleBone armv7 SBC this issue seems to be fixed with uv v0.4.29 (or earlier) for us. tried it yesterday with v0.6.12 again successfully! downloading python is no problem for this platform anymore. uv python 3.13 is 70% slower than the builtin py 3.10, but that's another issue.

---

_Comment by @zanieb on 2025-04-08 17:46_

@johndunderhill happy to look into it if you can share a reproduction of what's happening (in a new issue please, feel free to tag me)

@orgua That's good to know! If you open an issue with a sample benchmark, reproduction, and logs we'd be very interested in looking into it. That's a big perf regression.

---

_Comment by @zanieb on 2025-04-08 17:47_

I see there's a reproduction I commented on before... let me see if I can explain what's going on there again...

---

_Comment by @zanieb on 2025-04-08 17:56_

We use the following code to determine your host architecture:

https://github.com/astral-sh/uv/blob/553bcccb6ae8139ecf1545854c0712961b978879/crates/uv-python/src/platform.rs#L99-L104

which returns the architecture uv for. This gives us `arm` on your uv installation (per the logs).

We build both `arm` and `armv7` variants of uv at

https://github.com/astral-sh/uv/blob/be615cb213c40df573da19a69d0855caf99455c0/.github/workflows/build-binaries.yml#L398-L401

The downloads are listed at, e.g., https://github.com/astral-sh/uv/releases/tag/0.6.13

However, we don't build managed Python versions for `arm`, just `armv7`. So you cannot install Python with uv if you are using the `arm` uv. How are you installing uv? Can you install the armv7 variant instead? 



---

_Comment by @zanieb on 2025-04-08 18:05_

I think Raspberry Pi Model 4 B is armv8 which, in my limited understanding, is 64-bit and you can just use our `aarch64` binaries?

---

_Comment by @johndunderhill on 2025-04-09 12:18_

Thanks, @zanieb.  We install **uv** using your script.  In production, our installer would be installing **uv** onto an arm system, and then trying to install Python 3.12 onto that same arm system.

You may be on to something here.  Running `uname -m` on some of the older systems returns `arm7l` which is an arm7 CPU running in little endian mode.  This is a 32-bit environment, something I had not taken into account.  Nonetheless, **uv** seems to work fine in this environment:

```
pi@raspberrypi4:~ $ uv self update
info: Checking for updates...
success: Upgraded uv from v0.5.9 to v0.6.13! https://github.com/astral-sh/uv/releases/tag/0.6.13
pi@raspberrypi4:~ $ uv python install 3.12.8 --reinstall
Installed Python 3.12.8 in 4.25s
 ~ cpython-3.12.8-linux-armv7-gnueabihf
pi@raspberrypi4:~ $ uv python list
cpython-3.14.0a6-linux-armv7-gnueabihf                 <download available>
cpython-3.14.0a6+freethreaded-linux-armv7-gnueabihf    <download available>
cpython-3.13.2-linux-armv7-gnueabihf                   <download available>
cpython-3.13.2+freethreaded-linux-armv7-gnueabihf      <download available>
cpython-3.12.9-linux-armv7-gnueabihf                   <download available>
cpython-3.12.8-linux-armv7-gnu                         .local/share/uv/python/cpython-3.12.8-linux-armv7-gnueabihf/bin/python3.12
cpython-3.11.11-linux-armv7-gnueabihf                  <download available>
cpython-3.10.16-linux-armv7-gnueabihf                  <download available>
cpython-3.9.21-linux-armv7-gnueabihf                   <download available>
cpython-3.7.3-linux-armv7-gnu                          /usr/bin/python3.7
cpython-3.7.3-linux-armv7-gnu                          /usr/bin/python3 -> python3.7
```

But this environment is "Raspbian GNU/Linux 10 (buster)."  The problem machine in my original report was "Raspbian GNU/Linux 11 (bullseye)".  I'll try to run down one of those and test it with the latest version.  If it works, I'll post here, otherwise I'll open a new issue.  Thanks for your help.

John

---
