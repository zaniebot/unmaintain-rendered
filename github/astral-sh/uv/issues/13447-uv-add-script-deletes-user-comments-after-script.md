---
number: 13447
title: "`uv add --script` deletes user comments after `script`-comment-block"
type: issue
state: closed
author: qwerbzuio
labels:
  - bug
assignees: []
created_at: 2025-05-14T08:57:17Z
updated_at: 2025-05-14T21:54:22Z
url: https://github.com/astral-sh/uv/issues/13447
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv add --script` deletes user comments after `script`-comment-block

---

_Issue opened by @qwerbzuio on 2025-05-14 08:57_

### Summary

When updating inline script metadata of a python script via `uv add --script`, user's comments after the `script`-comment-block are deleted.

Reproducer:

Create python script `foo.py` with contents:
```py
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "numpy",
# ]
# ///
#
# Some description
```

Run `uv add --script test.py matplotlib`. `foo.py`:
```py
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "matplotlib",
#     "numpy",
# ]
# ///
```

As you can see, `# Some description` has been deleted by `uv`, even though it is placed after the closing `# ///` of the `script`-block.

Adding an empy line (no leading `#`) helps to preserve the user-comment, but I believe that `uv` should not touch anything outside the block-boundaries.

### Platform

Windows 11 x86_64

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

_No response_

---

_Label `bug` added by @qwerbzuio on 2025-05-14 08:57_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-05-14 18:49_

---

_Referenced in [astral-sh/uv#13460](../../astral-sh/uv/pulls/13460.md) on 2025-05-14 19:21_

---

_Closed by @charliermarsh on 2025-05-14 21:54_

---
