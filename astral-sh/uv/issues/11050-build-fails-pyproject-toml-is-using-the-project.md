```yaml
number: 11050
title: "Build fails: `pyproject.toml` is using the `[project]` table, but the required `project.name` field is not set"
type: issue
state: closed
author: mxlei01
labels:
  - question
assignees: []
created_at: 2025-01-29T02:46:42Z
updated_at: 2025-01-29T06:23:25Z
url: https://github.com/astral-sh/uv/issues/11050
synced_at: 2026-01-10T03:50:31Z
```

# Build fails: `pyproject.toml` is using the `[project]` table, but the required `project.name` field is not set

---

_Issue opened by @mxlei01 on 2025-01-29 02:46_

### Summary

On newer versions of `uv`, it seems that all projects under dependencies require to have name in their pyproject.toml?

An example of one that doesn't have: https://github.com/arvidn/libtorrent/blob/RC_2_0/pyproject.toml

```
[project]
name = "test"
version = "0.1.0"
description = ""
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "libtorrent @ git+https://github.com/arvidn/libtorrent.git@v2.0.10#subdirectory=bindings/python"
]
```

Something like this will fail in 0.5.24 but will not in 0.4.3.

Are there any flags you can set? This seems to be due to PEP 621. The workaround is I could fork this repo and install from it, but I'd rather not.

### Platform

Ubuntu

### Version

uv 0.5.24

### Python version

3.12

---

_Label `bug` added by @mxlei01 on 2025-01-29 02:46_

---

_Comment by @zanieb on 2025-01-29 05:13_

You should report this as a bug to that project. This field is required per the specification — I think it's correct for us to enforce it is present.

---

_Label `bug` removed by @zanieb on 2025-01-29 05:13_

---

_Label `question` added by @zanieb on 2025-01-29 05:13_

---

_Closed by @mxlei01 on 2025-01-29 06:23_

---
