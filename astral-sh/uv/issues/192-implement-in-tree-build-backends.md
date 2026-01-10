---
number: 192
title: Implement in-tree build backends
type: issue
state: closed
author: konstin
labels:
  - enhancement
assignees: []
created_at: 2023-10-26T08:46:46Z
updated_at: 2023-11-10T10:54:25Z
url: https://github.com/astral-sh/uv/issues/192
synced_at: 2026-01-10T01:23:03Z
---

# Implement in-tree build backends

---

_Issue opened by @konstin on 2023-10-26 08:46_

https://peps.python.org/pep-0517/#in-tree-build-backends

This is required to resolve home-assistant

---

_Referenced in [astral-sh/uv#5](../../astral-sh/uv/issues/5.md) on 2023-10-26 09:10_

---

_Label `enhancement` added by @charliermarsh on 2023-10-26 13:29_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-10-26 13:29_

---

_Assigned to @konstin by @charliermarsh on 2023-10-26 13:30_

---

_Comment by @charliermarsh on 2023-10-26 17:41_

@konstin - I don't know if this is the same, but if not, can you file a new issue for it?

```
error: Failed to build source distribution tokenizers-0.0.11.tar.gz
  Caused by: Failed building wheel through setup.py:
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpSVAxpB/extracted/tokenizers-0.0.11/setup.py", line 2, in <module>
    from setuptools_rust import Binding, RustExtension
ModuleNotFoundError: No module named 'setuptools_rust'
```

---

_Comment by @konstin on 2023-10-26 18:55_

Done: #207

---

_Referenced in [astral-sh/uv#385](../../astral-sh/uv/pulls/385.md) on 2023-11-10 09:14_

---

_Closed by @konstin on 2023-11-10 10:54_

---
