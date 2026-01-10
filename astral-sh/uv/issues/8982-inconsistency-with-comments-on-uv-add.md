---
number: 8982
title: Inconsistency with comments on uv add
type: issue
state: closed
author: inoa-jboliveira
labels:
  - bug
assignees: []
created_at: 2024-11-10T02:40:52Z
updated_at: 2025-06-22T14:58:40Z
url: https://github.com/astral-sh/uv/issues/8982
synced_at: 2026-01-10T01:24:35Z
---

# Inconsistency with comments on uv add

---

_Issue opened by @inoa-jboliveira on 2024-11-10 02:40_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv 0.5.1 (f399a5271 2024-11-08)

When you have a comment after a dependency that is *not the last one* and a package is to be added right after it, the new package will take the comment for itself
```
$ cat pyproject.toml
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "django>5",
    "numpy>=2.1.3", # Hello I like numpy
    "zeep",
]
```

Add new package that will be right after numpy
```
$ uv add pandas
Resolved 22 packages in 21ms
Installed 3 packages in 340ms
 + pandas==2.2.3
 + python-dateutil==2.9.0.post0
 + six==1.16.0
```

Unexpected
```
$ cat pyproject.toml
[project]
name = "myproject"
version = "0.1.0"
description = "Add your description here"
requires-python = ">=3.12"
dependencies = [
    "django>5",
    "numpy>=2.1.3",
    "pandas>=2.2.3", # Hello I like numpy
    "zeep",
]
```


---

_Label `bug` added by @charliermarsh on 2024-11-10 03:08_

---

_Comment by @charliermarsh on 2024-11-10 03:09_

Thanks! I hate comments.

---

_Comment by @inoa-jboliveira on 2024-11-10 16:28_

In case it helps, I added "zeep" to the example above because if numpy were the last package, the comment would not be misplaced.

---

_Comment by @flyaroundme on 2024-11-10 16:28_

Yeah it is like that because when parsing with `toml_edit` this kind of comment is not treated as "the suffix of the second entry", but as "the prefix to the last line", and the invariant that is sustained here after adding is that this is "the prefix to the last line" 🤯

---

_Referenced in [astral-sh/uv#9758](../../astral-sh/uv/issues/9758.md) on 2024-12-10 00:43_

---

_Referenced in [astral-sh/uv#13966](../../astral-sh/uv/issues/13966.md) on 2025-06-11 13:51_

---

_Comment by @christeefy on 2025-06-20 12:44_

I think this is resolved with #12360.

---

_Comment by @konstin on 2025-06-22 14:58_

I confirmed this is fixed now.

---

_Closed by @konstin on 2025-06-22 14:58_

---
