---
number: 11532
title: "`uv add --index` fails to install PyTorch CUDA packages on Windows"
type: issue
state: closed
author: AbyssSkb
labels:
  - question
assignees: []
created_at: 2025-02-15T05:04:25Z
updated_at: 2025-09-09T07:05:51Z
url: https://github.com/astral-sh/uv/issues/11532
synced_at: 2026-01-10T01:25:07Z
---

# `uv add --index` fails to install PyTorch CUDA packages on Windows

---

_Issue opened by @AbyssSkb on 2025-02-15 05:04_

### Question

Is there a way to directly install the CUDA version of PyTorch via the command line on Windows and support project dependency management with uv? 

I tried installing PyTorch using `uv add torch --index` on my computer, but it ultimately failed. The documentation doesn't seem to mention this. Why does `uv add torch --index` fail to install PyTorch? 

Although I can install PyTorch by modifying the `pyproject.toml` file as mentioned in the documentation, I want to know if it is possible to install it directly through the command line (which is simpler and more familiar).

```bash
> uv add torch --index https://download.pytorch.org/whl/cu126
Resolved 16 packages in 4.52s
error: Distribution `markupsafe==3.0.2 @ registry+https://download.pytorch.org/whl/cu126` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're using CPython 3.10 (`cp310`), but `markupsafe` (v3.0.2) only has wheels with the following Python implementation tag: `cp313`
```

### Platform

Windows 11 x86_64

### Version

uv 0.6.0 (591f38c25 2025-02-14)

---

_Label `question` added by @AbyssSkb on 2025-02-15 05:04_

---

_Comment by @charliermarsh on 2025-02-15 13:52_

Unfortunately I don't _think_ there's a way to avoid this right now due to how the PyTorch index is structured... They have a `markupsafe` wheel, but only for Linux. By default, if we see a package (like `markupsafe`) on an index that the user provided (like the PyTorch index), we avoid looking at _other_ indexes for the same package, as a security concern.

If we supported a `--constraints` flag here, you could do `--constraints markupsafe<3.0.2`, but even that isn't great. Hmm.

---

_Closed by @AbyssSkb on 2025-09-09 07:05_

---
