---
number: 13713
title: uv fails to detect OS from interpreter on NixOS
type: issue
state: closed
author: definfo
labels:
  - bug
assignees: []
created_at: 2025-05-28T22:57:52Z
updated_at: 2025-05-30T17:17:21Z
url: https://github.com/astral-sh/uv/issues/13713
synced_at: 2026-01-07T13:12:18-06:00
---

# uv fails to detect OS from interpreter on NixOS

---

_Issue opened by @definfo on 2025-05-28 22:57_

### Summary

Running any uv command that requires a Python interpreter will raise error:
```
➜ uv --verbose sync
DEBUG uv 0.7.7
DEBUG Found project root: `/home/definfo/dir`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/home/definfo/dir`
DEBUG Using Python request `/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3` from explicit request
DEBUG Checking for Python environment at `/home/definfo/dir/.venv`
DEBUG Checking for Python interpreter in directory `/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3`
DEBUG Skipping bad interpreter at /nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3/bin/python3 from provided path: Unknown operating system: ``
DEBUG Released lock at `/tmp/uv-3e90f49eaef6eaa5.lock`
error: Failed to inspect Python interpreter from provided path at `/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3`
  Caused by: Can't use Python at `/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3/bin/python3`
  Caused by: Unknown operating system: ``
```

This can be reproduced with a bare uv in nixpkgs as well as uv2nix flakes.

### Platform

Linux 6.12.30 x86_64 GNU/Linux

### Version

0.7.7

### Python version

Should be 3.13.3 yet uv failed to detect it :(

---

_Label `bug` added by @definfo on 2025-05-28 22:57_

---

_Comment by @zanieb on 2025-05-29 12:27_

This comes from https://github.com/astral-sh/uv/blob/40efe6119bd645a061e975955139f6bde8a2f56a/crates/uv-python/python/get_interpreter_info.py#L408-L536

Can you provide the output of `sysconfig.get_platform()`?

---

_Comment by @definfo on 2025-05-29 14:38_

> This comes from
> 
> [uv/crates/uv-python/python/get_interpreter_info.py](https://github.com/astral-sh/uv/blob/40efe6119bd645a061e975955139f6bde8a2f56a/crates/uv-python/python/get_interpreter_info.py#L408-L536)
> 
> Lines 408 to 536 in [40efe61](/astral-sh/uv/commit/40efe6119bd645a061e975955139f6bde8a2f56a)
>  def get_operating_system_and_architecture(): 
>      """Determine the Python interpreter architecture and operating system. 
>   
>      This can differ from uv's architecture and operating system. For example, Apple 
>      Silicon Macs can run both x86_64 and aarch64 binaries transparently. 
>      """ 
>      # https://github.com/pypa/packaging/blob/cc938f984bbbe43c5734b9656c9837ab3a28191f/src/packaging/_musllinux.py#L84 
>      # Note that this is not `os.name`. 
>      # https://docs.python.org/3/library/sysconfig.html#sysconfig.get_platform 
>      # windows x86 will return win32 
>      platform_info = sysconfig.get_platform().split("-", 1) 
>      if len(platform_info) == 1: 
>          if platform_info[0] == "win32": 
>              operating_system, version_arch = "win", "i386" 
>          else: 
>              # unknown_operating_system will flow to the final error print 
>              operating_system, version_arch = platform_info[0], "" 
>      else: 
>          [operating_system, version_arch] = platform_info 
>      if "-" in version_arch: 
>          # Ex: macosx-11.2-arm64 
>          version, architecture = version_arch.rsplit("-", 1) 
>      else: 
>          # Ex: linux-x86_64 
>          version = None 
>          architecture = version_arch 
>   
>      if sys.version_info < (3, 7): 
>          print( 
>              json.dumps( 
>                  { 
>                      "result": "error", 
>                      "kind": "unsupported_python_version", 
>                      "python_version": format_full_version(sys.version_info), 
>                  } 
>              ) 
>          ) 
>          sys.exit(0) 
>   
>      if operating_system == "linux": 
>          # noinspection PyProtectedMember 
>          from .packaging._manylinux import _get_glibc_version 
>   
>          # noinspection PyProtectedMember 
>          from .packaging._musllinux import _get_musl_version 
>   
>          # https://github.com/pypa/packaging/blob/4dc334c86d43f83371b194ca91618ed99e0e49ca/src/packaging/tags.py#L539-L543 
>          # https://github.com/astral-sh/uv/issues/9842 
>          if struct.calcsize("P") == 4: 
>              if architecture == "x86_64": 
>                  architecture = "i686" 
>              elif architecture == "aarch64": 
>                  architecture = "armv8l" 
>   
>          musl_version = _get_musl_version(sys.executable) 
>          glibc_version = _get_glibc_version() 
>   
>          if musl_version: 
>              operating_system = { 
>                  "name": "musllinux", 
>                  "major": musl_version[0], 
>                  "minor": musl_version[1], 
>              } 
>          elif glibc_version != (-1, -1): 
>              operating_system = { 
>                  "name": "manylinux", 
>                  "major": glibc_version[0], 
>                  "minor": glibc_version[1], 
>              } 
>          elif hasattr(sys, "getandroidapilevel"): 
>              operating_system = { 
>                  "name": "android", 
>                  "api_level": sys.getandroidapilevel(), 
>              } 
>          else: 
>              print(json.dumps({"result": "error", "kind": "libc_not_found"})) 
>              sys.exit(0) 
>      elif operating_system == "win": 
>          operating_system = { 
>              "name": "windows", 
>          } 
>      elif operating_system == "macosx": 
>          # Apparently, Mac OS is reporting i386 sometimes in sysconfig.get_platform even 
>          # though that's not a thing anymore. 
>          # https://github.com/astral-sh/uv/issues/2450 
>          version, _, architecture = platform.mac_ver() 
>   
>          if not version or not architecture: 
>              print(json.dumps({"result": "error", "kind": "broken_mac_ver"})) 
>              sys.exit(0) 
>   
>          # https://github.com/pypa/packaging/blob/cc938f984bbbe43c5734b9656c9837ab3a28191f/src/packaging/tags.py#L356-L363 
>          is_32bit = struct.calcsize("P") == 4 
>          if is_32bit: 
>              if architecture.startswith("ppc"): 
>                  architecture = "ppc" 
>              else: 
>                  architecture = "i386" 
>   
>          version = version.split(".") 
>          operating_system = { 
>              "name": "macos", 
>              "major": int(version[0]), 
>              "minor": int(version[1]), 
>          } 
>      elif operating_system in [ 
>          "freebsd", 
>          "netbsd", 
>          "openbsd", 
>          "dragonfly", 
>          "illumos", 
>          "haiku", 
>      ]: 
>          operating_system = { 
>              "name": operating_system, 
>              "release": version, 
>          } 
>      else: 
>          print( 
>              json.dumps( 
>                  { 
>                      "result": "error", 
>                      "kind": "unknown_operating_system", 
>                      "operating_system": operating_system, 
>                  } 
>              ) 
>          ) 
>          sys.exit(0) 
>      return {"os": operating_system, "arch": architecture} 
> 
> Can you provide the output of `sysconfig.get_platform()`?

$ , uv --verbose run python -c "import sysconfig; print(sysconfig.get_platform())"
DEBUG uv 0.7.8
DEBUG Found project root: `/home/<dir>`
DEBUG No workspace root found, using project root
DEBUG Discovered project `<project>` at: /home/<dir>
DEBUG Acquired lock for `/home/<dir>`
DEBUG Using Python request `/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3/bin/python3.13` from explicit request
DEBUG Checking for Python environment at `.venv`
DEBUG Released lock at `/tmp/uv-d5a15f50e0d37c2d.lock`
error: Failed to query Python interpreter
  Caused by: failed to canonicalize path `/home/<dir>/.venv/bin/python3`: Permission denied (os error 13)

$ python -c "import sysconfig; print(sysconfig.get_platform())"
linux-x86_64

$ which python
/nix/store/2mab9iiwhcqwk75qwvp3zv0bvbiaq6cs-python3-3.13.3/bin/python

---

_Comment by @zanieb on 2025-05-29 14:43_

That's confusing, I don't see how we'd get to that error case with that output 🤔 

---

_Comment by @definfo on 2025-05-29 14:50_

The latter error message is simply a directory permission issue which seems unrelated to the previous one. :(
I'm trying to reproduce that situation.

---

_Comment by @definfo on 2025-05-30 17:17_

🤔 I can no longer reproduce this error after reboot.
I'll mark this issue as closed here (and reopen in case of any further context).

---

_Closed by @definfo on 2025-05-30 17:17_

---
