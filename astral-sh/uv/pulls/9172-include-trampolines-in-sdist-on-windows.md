```yaml
number: 9172
title: Include trampolines in sdist on Windows
type: pull_request
state: merged
author: zanieb
labels:
  - windows
  - releases
assignees: []
merged: true
base: main
head: zb/sd-tr
created_at: 2024-11-17T15:58:31Z
updated_at: 2024-11-18T01:47:26Z
url: https://github.com/astral-sh/uv/pull/9172
synced_at: 2026-01-10T12:00:00Z
```

# Include trampolines in sdist on Windows

---

_Pull request opened by @zanieb on 2024-11-17 15:58_

Naive fix for

```
         Compiling uv-trampoline-builder v0.0.1 (C:\Users\...\AppData\Local\uv\cache\sdists-v6\pypi\uv\0.5.2\9xswF03fJ5dr3vH_iowkm\src\crates\uv-trampoline-builder)
      error: couldn't read `crates\uv-trampoline-builder\src\../../uv-trampoline/trampolines/uv-trampoline-x86_64-gui.exe`: The system cannot find the path specified. (os error 3)
        --> crates\uv-trampoline-builder\src\lib.rs:21:5
         |
      21 |     include_bytes!("../../uv-trampoline/trampolines/uv-trampoline-x86_64-gui.exe");
         |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
```

Closes: https://github.com/astral-sh/uv/issues/9138.


---

_Label `releases` added by @zanieb on 2024-11-17 15:58_

---

_Review requested from @konstin by @zanieb on 2024-11-17 16:24_

---

_Label `windows` added by @charliermarsh on 2024-11-17 16:40_

---

_Comment by @charliermarsh on 2024-11-17 16:40_

See: https://github.com/astral-sh/uv/issues/9138

---

_Comment by @dlech on 2024-11-17 17:13_

Success!

```console
> pipx install ~\Downloads\uv-0.5.2.tar.gz
  installed package uv 0.5.2, installed using Python 3.13.0
  These apps are now globally available
    - uv.exe
    - uvx.exe
done! ✨ 🌟 ✨
> uv --version
uv 0.5.2
```

---

_Comment by @charliermarsh on 2024-11-17 17:14_

Amazing, thanks @dlech.

---

_@charliermarsh approved on 2024-11-17 17:14_

---

_Marked ready for review by @zanieb on 2024-11-17 17:48_

---

_Merged by @charliermarsh on 2024-11-18 01:47_

---

_Closed by @charliermarsh on 2024-11-18 01:47_

---

_Branch deleted on 2024-11-18 01:47_

---
