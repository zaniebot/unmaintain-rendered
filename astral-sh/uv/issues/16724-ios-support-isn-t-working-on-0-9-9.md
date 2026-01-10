---
number: 16724
title: "iOS support isn't working on 0.9.9"
type: issue
state: open
author: henryiii
labels:
  - bug
assignees: []
created_at: 2025-11-13T15:13:36Z
updated_at: 2025-11-14T05:49:54Z
url: https://github.com/astral-sh/uv/issues/16724
synced_at: 2026-01-10T01:26:09Z
---

# iOS support isn't working on 0.9.9

---

_Issue opened by @henryiii on 2025-11-13 15:13_

### Summary

I'm trying to use uv's iOS support directly in cibuildwheel, and I'm getting:

(AS)
```
Using CPython 3.13.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtual environment at: /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-wu8a60og/cp313-ios_arm64_iphoneos/build/venv
Activate with: source /private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-wu8a60og/cp313-ios_arm64_iphoneos/build/venv/bin/activate
error: Failed to inspect Python interpreter from active virtual environment at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-wu8a60og/cp313-ios_arm64_iphoneos/build/venv/bin/python3`
  Caused by: Querying Python at `/private/var/folders/6c/pzd640_546q6_yfn24r65c_40000gn/T/cibw-run-wu8a60og/cp313-ios_arm64_iphoneos/build/venv/bin/python3` returned an invalid response: unknown variant `iphoneos`, expected one of `aarch64`, `arm64`, `armv5tel`, `armv6l`, `armv7l`, `armv8l`, `powerpc64le`, `ppc64le`, `powerpc64`, `ppc64`, `powerpc`, `ppc`, `i386`, `i686`, `x86`, `amd64`, `x86_64`, `s390x`, `loongarch64`, `riscv64`, `wasm32`
```

(Intel)
```
Using CPython 3.13.9 interpreter at: /Library/Frameworks/Python.framework/Versions/3.13/bin/python3.13
Creating virtual environment at: /private/var/folders/vx/zmvzwd094y53mqcpn3jbm7580000gn/T/cibw-run-bcaei7sg/cp313-ios_x86_64_iphonesimulator/build/venv
Activate with: source /private/var/folders/vx/zmvzwd094y53mqcpn3jbm7580000gn/T/cibw-run-bcaei7sg/cp313-ios_x86_64_iphonesimulator/build/venv/bin/activate
error: Failed to inspect Python interpreter from active virtual environment at `/private/var/folders/vx/zmvzwd094y53mqcpn3jbm7580000gn/T/cibw-run-bcaei7sg/cp313-ios_x86_64_iphonesimulator/build/venv/bin/python3`
  Caused by: Querying Python at `/private/var/folders/vx/zmvzwd094y53mqcpn3jbm7580000gn/T/cibw-run-bcaei7sg/cp313-ios_x86_64_iphonesimulator/build/venv/bin/python3` returned an invalid response: unknown variant `iphonesimulator`, expected one of `aarch64`, `arm64`, `armv5tel`, `armv6l`, `armv7l`, `armv8l`, `powerpc64le`, `ppc64le`, `powerpc64`, `ppc64`, `powerpc`, `ppc`, `i386`, `i686`, `x86`, `amd64`, `x86_64`, `s390x`, `loongarch64`, `riscv64`, `wasm32`

```

`iphoneos` and `iphonesimulator` seem to be confusing it. From Discord. CC @freakboy3742. https://github.com/pypa/cibuildwheel/pull/2586

### Platform

macOS

### Version

0.9.9

### Python version

3.13

---

_Label `bug` added by @henryiii on 2025-11-13 15:13_

---

_Comment by @konstin on 2025-11-13 15:24_

The PEP talks about the (private?) `sys.implementation._multiarch` and `platform.ios_ver()`, @freakboy3742 do you know how those relate to `sysconfig.get_platform()`, the main value that uv currently reads?

---

_Comment by @freakboy3742 on 2025-11-13 22:19_

@konstin I have no knowledge at all about how *uv* is handling this. 

From the Python side, `sysconfig.get_platform()` is [internally constructed](https://github.com/python/cpython/blob/main/Lib/sysconfig/__init__.py#L731) as `{sys.platform}-{min_ios_version}-{sys._implementation.multiarch}`.

In practice, this means:

ARM64 device:
* `sys._implementation.multiarch` returns `arm64-iphoneos`
* `platform.ios_ver()` returns a namedtuple  `(system="iOS", release="18.6", model="iPhone16,4", is_simulator=False)`. The release and model is the actual iOS version and internal device name of the running device. System will return `iPad` on an iPad.
* `sysconfig.get_platform()` returns `ios-13.0-arm64-iphoneos`

ARM64 simulator:
* `sys._implementation.multiarch` returns `arm64-iphonesimulator`
* `platform.ios_ver()` returns a namedtuple  `(system="iOS", release="13.0", model="iphoneos", is_simulator=True)`
* `sysconfig.get_platform()` returns `ios-13.0-arm64-iphonesimulator`

x86-64 simulator:
* `sys._implementation.multiarch ` returns `x86_64-iphonesimulator`
* `platform.ios_ver()` returns a namedtuple  `(system="iOS", release="13.0", model="iphoneos", is_simulator=True)`
* `sysconfig.get_platform()` returns `ios-13.0-x86_64-iphonesimulator` .

The references to 13.0 describe the minimum supported iOS version, which is a function 

If the interpreter is being started in cibuildwheel, then it's running on macOS; depending on exactly how the interpreter was started, it may be in a "fake" virtual environment - a venv that has a `.pth` file that monkey patches key values in `sys`, `platform`, `sysconfig` and `os` to return iOS-like values. 

You can get an isolated version of that environment handling in [`xbuild`](http://github.com/beeware/xbuild). `xbuild` contains `xvenv`, which converts a "real" macOS venv in to a "fake" iOS, Android or Emscripten venv; `xbuild` is an extension of `build` that creates cross-platform environments and starts a cross-platform build. It's enough of a fake to convince `pip` to install iOS/Android wheels; and provided the rest of the environment variables are set up appropriately, to manage compiling binary wheels.

Happy to provide any additional details that might be helpful.

---

_Comment by @samypr100 on 2025-11-14 05:40_

Interesting that [Android](https://github.com/pypa/cibuildwheel/pull/2587) is working in cibuildwheel but not iOS.

---

_Comment by @henryiii on 2025-11-14 05:49_

Android doesn't have "simulator" vs "device" separate binaries, iOS does. I think that's why it's more complicated.

---
