---
number: 16375
title: "Is there a way to use a PEP 751 pylock.toml as input for `uv lock` or `uv sync`?"
type: issue
state: closed
author: astrojuanlu
labels:
  - question
assignees: []
created_at: 2025-10-20T13:14:54Z
updated_at: 2025-10-21T08:40:57Z
url: https://github.com/astral-sh/uv/issues/16375
synced_at: 2026-01-10T01:26:05Z
---

# Is there a way to use a PEP 751 pylock.toml as input for `uv lock` or `uv sync`?

---

_Issue opened by @astrojuanlu on 2025-10-20 13:14_

### Question

As per title.

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @astrojuanlu on 2025-10-20 13:14_

---

_Comment by @astrojuanlu on 2025-10-20 13:19_

For example, #12992 says

> We accept `pylock.toml` as a requirements file (e.g., `uv sync pylock.toml`

But `uv sync pylock.toml` fails on recent uv:

```
$ uv sync pylock.toml
error: unexpected argument 'pylock.toml' found

Usage: uv sync [OPTIONS]

For more information, try '--help'.
$ uv --version
uv 0.9.3
```

---

_Comment by @konstin on 2025-10-20 14:43_

You can use it with `uv pip install -r`, the description in https://github.com/astral-sh/uv/pull/12992 is wrong. `uv lock` and `uv sync` only work with the project interface and `uv.lock`, `uv pip` support various kinds of inputs and outputs including `pylock.toml`.

---

_Comment by @astrojuanlu on 2025-10-21 08:40_

Understood, thank you!

---

_Closed by @astrojuanlu on 2025-10-21 08:40_

---
