---
number: 15875
title: "`Warning: Failed to hardlink files;` In a local ubuntu system"
type: issue
state: closed
author: matrs
labels:
  - question
assignees: []
created_at: 2025-09-15T16:05:16Z
updated_at: 2025-09-16T16:42:35Z
url: https://github.com/astral-sh/uv/issues/15875
synced_at: 2026-01-07T13:12:19-06:00
---

# `Warning: Failed to hardlink files;` In a local ubuntu system

---

_Issue opened by @matrs on 2025-09-15 16:05_

### Summary

Everything is local, an ext4 filesystem in a standard `Kubuntu` installation. The packages installation worked, but with this warning at the end:
```sh
$ uv pip install --prerelease=allow --index-strategy unsafe-best-match --extra-index-url=https://pypi.nvidia.com "cuml-cu12==25.8.*"
warning: Failed to hardlink files; falling back to full copy. ...

$ which python
/mnt/WORK/fiftyone/fiftyone/bin/python
$ echo $VIRTUAL_ENV
/mnt/WORK/fiftyone/fiftyone
$ uv --version
uv 0.8.17
$ uname -a
Linux ... 6.14.0-29-generic #29~24.04.1-Ubuntu SMP PREEMPT_DYNAMIC Thu Aug 14 16:52:50 UTC 2 x86_64 x86_64 x86_64 GNU/Linux
```

### Platform

Linux 6.14.0-29-generic x86_64 GNU/Linux

### Version

0.8.17

### Python version

3.12.3

---

_Label `bug` added by @matrs on 2025-09-15 16:05_

---

_Label `bug` removed by @konstin on 2025-09-15 16:10_

---

_Label `question` added by @konstin on 2025-09-15 16:10_

---

_Comment by @zanieb on 2025-09-15 16:10_

Where's your cache? e.g., from `uv cache dir`?

---

_Comment by @zanieb on 2025-09-15 16:11_

If your cache is on a different file system, you'll see this warning.

---

_Comment by @konstin on 2025-09-15 16:11_

In this case, uv can't hardlink because the cache is in `~/.cache/uv`, while the project is on a different drive, so the warning is correct. You can turn it off by setting link mode to copy explicitly.

---

_Comment by @matrs on 2025-09-15 16:22_

I see, thanks for the answer.  Yes, the  venv is in a different **partition but in the same drive** that `$HOME`.

---

_Comment by @zanieb on 2025-09-15 16:41_

Yeah different partitions have different file systems. You'll want to change the `UV_CACHE_DIR`

---

_Closed by @matrs on 2025-09-16 16:42_

---
