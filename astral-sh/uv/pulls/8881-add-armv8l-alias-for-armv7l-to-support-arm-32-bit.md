```yaml
number: 8881
title: Add armv8l alias for armv7l to support arm 32-bit compatibility mode
type: pull_request
state: merged
author: adbjo
labels:
  - enhancement
assignees: []
merged: true
base: main
head: add_armv8l_alias_to_supported_architectures
created_at: 2024-11-07T08:21:58Z
updated_at: 2024-11-07T16:54:40Z
url: https://github.com/astral-sh/uv/pull/8881
synced_at: 2026-01-10T12:00:00Z
```

# Add armv8l alias for armv7l to support arm 32-bit compatibility mode

---

_Pull request opened by @adbjo on 2024-11-07 08:21_

## Summary

`uv` commands like `uv venv` and `uv tool` throw this error when running on arm64 in 32-bit [compatibility mode](https://elixir.bootlin.com/linux/v6.11.6/source/arch/arm64/include/asm/compat.h#L32):
```
unknown variant `armv8l`, expected one of `aarch64`, `arm64`, `armv6l`, `armv7l`, `powerpc64le`, `ppc64le`, `powerpc64`, `ppc64`, `i386`, `i686`, `x86`, `amd64`, `x86_64`, `s390x`, `riscv64`
--- stdout:
{"result": "success", "markers": {"implementation_name": "cpython", "implementation_version": "3.11.10", "os_name": "posix", "platform_machine": "armv8l", "platform_python_implementation": "CPython", "platform_release": "6.8.0-1015-azure", "platform_system": "Linux", "platform_version": "#17-Ubuntu SMP Mon Sep  2 15:50:42 UTC 2024", "python_full_version": "3.11.10", "python_version": "3.11", "sys_platform": "linux"}, "sys_base_prefix": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf", "sys_base_exec_prefix": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf", "sys_prefix": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf", "sys_base_executable": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/bin/python3.11", "sys_executable": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/bin/python3.11", "sys_path": ["/home/pi/.cache/uv/.tmpvXy34S", "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python311.zip", "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11", "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11/lib-dynload", "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11/site-packages"], "stdlib": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11", "scheme": {"platlib": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11/site-packages", "purelib": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/lib/python3.11/site-packages", "include": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/include/python3.11", "scripts": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf/bin", "data": "/home/pi/.local/share/uv/python/cpython-3.11.10-linux-armv7-gnueabihf"}, "virtualenv": {"purelib": "lib/python3.11/site-packages", "platlib": "lib/python3.11/site-packages", "include": "include/site/python3.11", "scripts": "bin", "data": ""}, "platform": {"os": {"name": "manylinux", "major": 2, "minor": 31}, "arch": "armv8l"}, "manylinux_compatible": true, "gil_disabled": false, "pointer_size": "32"}
--- stderr:

---
```

This is needed when building for 32-bit arm on a arm64 runner.

The `platform.machine()` function in python (which uses the `uname` syscall) outputs`armv8l` when running in 32-bit [compatibility mode](https://elixir.bootlin.com/linux/v6.11.6/source/arch/arm64/include/asm/compat.h#L32), which means that the system supports armv7l binaries.

## Test Plan

Not tested :) 


---

_Comment by @konstin on 2024-11-07 08:44_

Are armv8l and armv7l binary compatible?

---

_Comment by @adbjo on 2024-11-07 09:18_

> Are armv8l and armv7l binary compatible?

I'm no expert, and I don't really know what I'm doing, but any `armv8` chip can run `armv7` binaries, since arm 64-bit is backwards compatible, but a `armv7` chip cannot run `armv8` binaries.

arm 64-bit architecture is typically referred to using the `arm64` or `aarch64` aliases. The `armv8l` alias is only used when running in a 32-bit operating environment on a arm 64-bit chip. So `armv8l` output from `platform.machine()` means that the system runs binaries compiled for 32-bit arm (`armv7l`).

Read more here:
https://stackoverflow.com/questions/73688696/what-is-my-architecture-and-what-does-armv8l-exactly-means

---

_Comment by @adbjo on 2024-11-07 13:20_

The `uv` installer already has support for it:
```bash
case "$_cputype" in

    i386 | i486 | i686 | i786 | x86)
        _cputype=i686
        ;;

    xscale | arm)
        _cputype=arm
        if [ "$_ostype" = "linux-android" ]; then
            _ostype=linux-androideabi
        fi
        ;;

    armv6l)
        _cputype=arm
        if [ "$_ostype" = "linux-android" ]; then
            _ostype=linux-androideabi
        else
            _ostype="${_ostype}eabihf"
        fi
        ;;

    armv7l | armv8l)
        _cputype=armv7
        if [ "$_ostype" = "linux-android" ]; then
            _ostype=linux-androideabi
        else
            _ostype="${_ostype}eabihf"
        fi
        ;;

    aarch64 | arm64)
        _cputype=aarch64
        ;;
```

---

_Merged by @charliermarsh on 2024-11-07 16:42_

---

_Closed by @charliermarsh on 2024-11-07 16:42_

---

_Label `enhancement` added by @konstin on 2024-11-07 16:54_

---
