---
number: 14942
title: "tokenizers fails to install on macOS via `pyproject.toml`"
type: issue
state: closed
author: prrao87
labels:
  - bug
assignees: []
created_at: 2025-07-28T15:42:55Z
updated_at: 2025-07-28T15:52:36Z
url: https://github.com/astral-sh/uv/issues/14942
synced_at: 2026-01-10T01:25:50Z
---

# tokenizers fails to install on macOS via `pyproject.toml`

---

_Issue opened by @prrao87 on 2025-07-28 15:42_

### Summary

I'm using a uv-managed version of Python 3.13 globally. I am unable to install the popular Hugging Face `tokenizers` library using `pyproject.toml`.

```
$ uv add tokenizers
Using CPython 3.13.5
Creating virtual environment at: .venv
Resolved 16 packages in 6ms
error: Distribution `tokenizers==0.21.4 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on macOS (`macosx_15_0_arm64`), but `tokenizers` (v0.21.4) only has wheels for the following platforms: `manylinux_2_17_aarch64`, `manylinux_2_17_armv7l`, `manylinux_2_17_ppc64le`, `manylinux_2_17_s390x`, `manylinux2014_aarch64`, `manylinux2014_armv7l`, `manylinux2014_ppc64le`, `manylinux2014_s390x`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```
However, using the standard pip API and avoiding `pyproject.toml` seems to work:
```
$ uv venv
$ source .venv/bin/activate
$ uv pip install tokenizers
```
This doesn't complain and it installs. What's happening during dependency resolution when using the command `uv add tokenizers`?

### Platform

macOS 15 arm64

### Version

uv 0.8.3 (7e78f54e7 2025-07-24)

### Python version

Python 3.13.5

---

_Label `bug` added by @prrao87 on 2025-07-28 15:42_

---

_Renamed from "tokenizers fails to install on macOS" to "tokenizers fails to install on macOS via `pyproject.toml`" by @prrao87 on 2025-07-28 15:43_

---

_Comment by @prrao87 on 2025-07-28 15:50_

Update: This seems to be broken because an upstream change to the `tokenizers` library. The issue seems to be resolved if we downgrade the version of tokenizers.
```
uv add 'tokenizers==0.21.2'
```
It's something that's likely resolvable on their end. This isn't a uv issue, so am closing this.
https://github.com/huggingface/tokenizers/issues/1836

---

_Closed by @prrao87 on 2025-07-28 15:51_

---
