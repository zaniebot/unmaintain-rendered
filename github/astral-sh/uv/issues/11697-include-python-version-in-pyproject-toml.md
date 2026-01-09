---
number: 11697
title: Include python-version in pyproject.toml
type: issue
state: closed
author: tomaszbk
labels:
  - question
assignees: []
created_at: 2025-02-21T12:29:34Z
updated_at: 2025-02-21T17:02:12Z
url: https://github.com/astral-sh/uv/issues/11697
synced_at: 2026-01-07T13:12:18-06:00
---

# Include python-version in pyproject.toml

---

_Issue opened by @tomaszbk on 2025-02-21 12:29_

### Question

Wouldn't it be cleaner to include the python version to use in the `pyproject.toml` file, instead of creating a separate `.python-version` file?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @tomaszbk on 2025-02-21 12:29_

---

_Comment by @my1e5 on 2025-02-21 13:08_

See

* https://github.com/astral-sh/uv/issues/9494
* https://github.com/astral-sh/uv/issues/4970
* https://github.com/astral-sh/uv/issues/4359

---

_Comment by @Gankra on 2025-02-21 17:02_

[This comment](https://github.com/astral-sh/uv/issues/8247#issuecomment-2416680597) by Charlie I think summarizes the situation well:

> The .python-version file is not quite obsolete with requires-python -- they're subtly different. requires-python is the range of versions supported by your project. .python-version is the exact version you want to use when developing. For example, if you're working on a library, your requires-python might be >= "3.10". But when developing on your machine, you might want to use 3.12.4 by default -- so you'd set that in a .python-version.

Since these two values are different, and pyproject one should ideally be looser, I think this continues to be something we don't plan to change.

---

_Closed by @Gankra on 2025-02-21 17:02_

---
