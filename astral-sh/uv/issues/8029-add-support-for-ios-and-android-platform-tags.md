```yaml
number: 8029
title: Add support for iOS and Android platform tags
type: issue
state: closed
author: freakboy3742
labels:
  - enhancement
  - help wanted
  - compatibility
assignees: []
created_at: 2024-10-09T00:51:38Z
updated_at: 2025-09-03T22:24:53Z
url: https://github.com/astral-sh/uv/issues/8029
synced_at: 2026-01-12T15:59:18Z
```

# Add support for iOS and Android platform tags

---

_@freakboy3742_

PEP 730 added support for iOS as a tier 3 supported platform, using the platform tags:
* `ios_X_Y_arm64_iphoneos` (iOS ARM64 device)
* `ios_X_Y_arm64_iphonesimulator` (iOS simulator running on ARM64 macOS)
* `ios_X_Y_x86_64_iphonesimulator` (iOS simulator running on x86_64 macOS)

where `X_Y` is the minimum iOS API version. `13.0` is the default version supported by a CPython build; 12.0 is known to work except for some warnings when compiling the sqlite3 module. As with macOS, a 13.0 wheel will work on iOS 18, but a 18.0 wheel won't run on a device running iOS 13.

PEP 738 added support for Android as a tier 3 supported platform, using the tags:
* `android_X_arm64_v8a` (Android ARM64 device, or emulator on ARM64 hardware)
* `android_X_x86_64` (Android emulator on x86_64 hardware)

where `X` is the Android SDK level; `21` by default. As with iOS, the value of `X` is the oldest device SDK that is supported.

`uv pip install` should add support for these tags to the list allowed by `--python-platform`. 

As with macOS, it may also be desirable to include shortcuts so that a full tag isn't required; however, this can't be a simple "ios"/"android" shortcut. For iOS, at least 2 shortcuts will be required, as `iphoneos` and `iphonesimulator` are different ABIs. There is also the need to accomodate architecture targeting, most Android developers on Linux/Windows will have x86_64 devices, but need to be able to target ARM64 Android devices as part of build/release processes.


---

_Label `compatibility` added by @charliermarsh on 2024-12-27 14:36_

---

_Label `enhancement` added by @charliermarsh on 2024-12-27 14:36_

---

_Comment by @charliermarsh on 2024-12-27 14:37_

Makes sense, thanks!

---

_Label `help wanted` added by @charliermarsh on 2024-12-27 14:37_

---

_Comment by @charliermarsh on 2024-12-27 16:56_

Is there any reference code for detecting whether the current Python is Android / iOS, and at what versions?

---

_Comment by @freakboy3742 on 2024-12-28 02:16_

@charliermarsh `sys.platform` returns `ios` on iOS (on both device and simulator), and `android` on Android. The current OS version can be determined using `platform.ios_ver()` and `platform.android_ver()` respectively. `ios_ver()` also has a flag that tells you if you're on a simulator or device; `android_ver()` can tell you the API level in addition to the OS version.

Two caveats on this.

Firstly, on Python 3.12 and earlier, iOS is officially unsupported. However, Android *can* be compiled with 3.12 and earlier; but it returns `sys.platform == "linux"`. This was one of the big changes that came from PEP 738 and Tier 3 support in Python 3.13.

Secondly, the most important use case for iOS/Android installs is *not* running natively on the platform. For most practical purposes, you will *not* be performing an install (pip, uv or otherwise) *on* iOS or Android. iOS and Android installs will almost always be cross-platform installs, running on a macOS/Linux/Windows build system targeting an iOS/Android host. 

This is essentially a limitation of the platforms themselves - when you ship an app using the respective app stores for each platform, they need to be fully self-contained, with all code that will be executed at runtime. While it is technically possible to download code at runtime, you won't be able to use the normal `uv` interface, as there is no ability to invoke subprocesses on either platform. You'd need to wrap uv as a Python module and invoke it programmatically in a thread of the app itself.


---

_Comment by @freakboy3742 on 2024-12-28 02:35_

For reference, this is the patch adding support for iOS to pip: pypa/pip#12962 (released in pip 24.3).

---

_Comment by @charliermarsh on 2024-12-28 02:42_

Perfect, thanks!

---

_Closed by @charliermarsh on 2025-09-03 22:24_

---
