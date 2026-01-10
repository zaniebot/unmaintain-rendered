```yaml
number: 11396
title: "Catch broken `mac_ver()`"
type: pull_request
state: merged
author: konstin
labels:
  - error messages
assignees: []
merged: true
base: main
head: konsti/broken-mac-ver
created_at: 2025-02-10T19:00:48Z
updated_at: 2025-02-10T21:49:18Z
url: https://github.com/astral-sh/uv/pull/11396
synced_at: 2026-01-10T11:10:37Z
```

# Catch broken `mac_ver()`

---

_Pull request opened by @konstin on 2025-02-10 19:00_

A user reported a homebrew Python that would raise an exception in the interpreter probing script because `platform.mac_ver()` returned `('', ('', '', ''), '')` on his installation due to https://github.com/Homebrew/homebrew-core/issues/206778

This is easy enough to catch and show a proper error message instead of the Python backtrace.

---

_Label `error messages` added by @konstin on 2025-02-10 19:00_

---

_@zanieb approved on 2025-02-10 20:00_

---

_Comment by @henryiii on 2025-02-10 20:05_

```
  × Failed to inspect Python interpreter from provided path at `/usr/local/Cellar/nox/2025.2.9/libexec/bin/python`
  ╰─▶ Querying Python at `/usr/local/Cellar/nox/2025.2.9/libexec/bin/python` failed with exit status exit status: 1

      [stdout]
      {"result": "error", "kind": "broken_mac_ver"}

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 1, in <module>
          import sys; sys.path = ["/Users/henryschreiner/.cache/uv/.tmpDksnYo"] + sys.path; from python.get_interpreter_info import main; main()
                                                                                                                                          ~~~~^^
        File "/Users/henryschreiner/.cache/uv/.tmpDksnYo/python/get_interpreter_info.py", line 543, in main
          os_and_arch = get_operating_system_and_architecture()
        File "/Users/henryschreiner/.cache/uv/.tmpDksnYo/python/get_interpreter_info.py", line 499, in get_operating_system_and_architecture
          "major": int(version[0]),
                   ~~~^^^^^^^^^^^^
      ValueError: invalid literal for int() with base 10: ''
```
Here is the new output.

---

_Comment by @konstin on 2025-02-10 20:38_

Sorry to ask again, do you mind trying with the new commit?

---

_Comment by @henryiii on 2025-02-10 21:40_

They uploaded a macOS Sequoia binary an hour ago, so I can't now, that fixed the problem. (https://github.com/Homebrew/homebrew-core/pull/207165)

---

_Comment by @henryiii on 2025-02-10 21:44_

If I fake it by modifying `/usr/local/Cellar/python@3.13/3.13.2/Frameworks/Python.framework/Versions/3.13/lib/python3.13/platform.py`, I get:

```
  × Failed to inspect Python interpreter from provided path at `/usr/local/Cellar/nox/2025.2.9/libexec/bin/python`
  ├─▶ Can't use Python at `/usr/local/Cellar/nox/2025.2.9/libexec/bin/python`
  ╰─▶ Broken Python installation, `platform.mac_ver()` returned an empty value, please reinstall Python
```

---

_Merged by @konstin on 2025-02-10 21:49_

---

_Closed by @konstin on 2025-02-10 21:49_

---

_Branch deleted on 2025-02-10 21:49_

---
