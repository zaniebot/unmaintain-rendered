---
number: 13497
title: "Support exact `requires-python` version in generating `pylock.toml`"
type: issue
state: open
author: ketozhang
labels:
  - enhancement
assignees: []
created_at: 2025-05-16T19:54:58Z
updated_at: 2025-09-22T20:23:26Z
url: https://github.com/astral-sh/uv/issues/13497
synced_at: 2026-01-07T13:12:18-06:00
---

# Support exact `requires-python` version in generating `pylock.toml`

---

_Issue opened by @ketozhang on 2025-05-16 19:54_

### Summary

Currently, `uv pip compile SRC_FILE -o pylock.toml  --python-version 3.12.10` resolves to: 

```toml
# ...
lock-version = "1.0"
created-by = "uv"
requires-python = ">=3.12.10"
# ...
```

The problem for me is I do not intend to deploy 3.13 regardless if 3.13 is compatible with this lock file.

In the use case of deploying reproducible environments including the installation of Python, it would be nice to not need to also specify `--python` or the `.python_version` file. 

### Example

_No response_

---

_Label `enhancement` added by @ketozhang on 2025-05-16 19:54_

---

_Renamed from "Support exact Python version in generating `pylock.toml`" to "Support exact `requires-python` version in generating `pylock.toml`" by @ketozhang on 2025-05-16 19:55_

---

_Comment by @AndydeCleyre on 2025-09-22 16:31_

I'm confused about how `requires-python` is determined. I also think it should exactly match the specification used in the input. In the case of `pyproject.toml`, for example, that can include

```toml
requires-python = ">=3.10,<3.11"
```

but then `pylock.toml` seems to use the currently installed minor version as a minimum:

```toml
requires-python = ">=3.10.18"
```

and subsequently an attempt to `uv pip install -r pylock.toml` will fail in a Python 3.10.12 environment, against my expectations. 

---

_Comment by @ketozhang on 2025-09-22 20:22_

I think there isn't much support for this feature request since [PEP 751](https://peps.python.org/pep-0751/#requires-python) defines `requires-python` as "minimum viable Python version for the lock file". I may be misunderstanding its purpose. The only de-facto standard for setting Python version is the `.python-version` file.

I'm fine with closing this issue if that's the case.

---

_Comment by @ketozhang on 2025-09-22 20:22_

@AndydeCleyre I think what you're seeing warrants another issue. I agree if the source is `pyproject.toml`, then it should copy over it's `requires-python` field. 

---

_Referenced in [astral-sh/uv#15995](../../astral-sh/uv/issues/15995.md) on 2025-09-22 21:42_

---
