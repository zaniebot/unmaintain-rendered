```yaml
number: 11863
title: "Available PyPy3.10 is listed as 3.10.19 but it's actually 3.10.16"
type: issue
state: closed
author: DavidCEllis
labels:
  - bug
  - external
assignees: []
created_at: 2025-02-28T18:31:49Z
updated_at: 2025-03-05T00:36:22Z
url: https://github.com/astral-sh/uv/issues/11863
synced_at: 2026-01-12T16:00:48Z
```

# Available PyPy3.10 is listed as 3.10.19 but it's actually 3.10.16

---

_@DavidCEllis_

### Summary

I see the issue about the lack of PyPy implementation version info but there's also a bug with the current 3.10 download file having the incorrect stdlib support version number.

The download list from `uv python list --only-downloads` currently shows `pypy-3.10.19-windows-x86_64-none` as available. However this is actually PyPy supporting the stdlib 3.10.16 as you see when you install it.

```
> uv python install pypy-3.10
Installed Python 3.10.19 in 2.50s
 + pypy-3.10.19-windows-x86_64-none
> .\uv\python\pypy-3.10.19-windows-x86_64-none\pypy.exe -V
Python 3.10.16 (e90335a7776c, Feb 05 2025, 13:30:31)
[PyPy 7.3.18 with MSC v.1941 64 bit (AMD64)]
```

This is confusing as after install `uv python list` shows the key as `3.10.16` but the folder is `3.10.19`. Uninstall requires using `3.10.19` even though it is not listed as installed.

`uv python list` output:
```
pypy-3.10.19-windows-x86_64-none                     <download available>
pypy-3.10.16-windows-x86_64-none                     C:\Users\ducks\AppData\Roaming\uv\python\pypy-3.10.19-windows-x86_64-none\pypy3.10.exe
```

### Platform

Windows 10 x86_64 and Ubuntu 24.04

### Version

0.6.3

### Python version

_No response_

---

_Label `bug` added by @DavidCEllis on 2025-02-28 18:31_

---

_Comment by @zanieb on 2025-02-28 18:49_

Thanks for the report!

---

_Assigned to @zanieb by @zanieb on 2025-02-28 18:49_

---

_Comment by @zanieb on 2025-03-03 21:06_

This looks wrong upstream https://github.com/pypy/pypy/issues/5236

---

_Label `external` added by @zanieb on 2025-03-03 21:06_

---

_Comment by @DavidCEllis on 2025-03-03 21:41_

Looks like the number is wrong upstream.

But I still expected that the `key` given by `uv python list --output-format json` would match the key needed to install/uninstall regardless? I was surprised that the path and key didn't match after install.

---

_Comment by @zanieb on 2025-03-03 21:51_

The `uv python list` output derives the key from the metadata reported by the interpreter, which is required because we list keys for unmanaged Python installations. In contrast, uninstallation assumes the key reported in the directory is correct. This is an invariant we assume to hold in the download metadata, I'm hesitant to add any special handling around it.

---

_Comment by @zanieb on 2025-03-05 00:25_

Will be fixed by https://github.com/astral-sh/uv/pull/11965

---

_Closed by @zanieb on 2025-03-05 00:36_

---

_Closed by @zanieb on 2025-03-05 00:36_

---
