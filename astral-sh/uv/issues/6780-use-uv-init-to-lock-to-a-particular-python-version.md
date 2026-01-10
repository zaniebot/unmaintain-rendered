---
number: 6780
title: "Use `uv init` to lock to a particular Python version"
type: issue
state: closed
author: SamEdwardes
labels:
  - projects
assignees: []
created_at: 2024-08-28T22:26:00Z
updated_at: 2024-08-29T14:56:34Z
url: https://github.com/astral-sh/uv/issues/6780
synced_at: 2026-01-10T01:24:05Z
---

# Use `uv init` to lock to a particular Python version

---

_Issue opened by @SamEdwardes on 2024-08-28 22:26_

First of all, awesome work uv team! I am really enjoying using uv :)

## Background

I have one idea for the `uv init` command. Take this example:

```bash
uv init --app --python 3.12.5
```

It will generate the following:

```toml
[project]
name = "example-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12.5"
dependencies = []
```

I would like to have a way to specify a specific Python version. E.g. `requires-python = "==3.12.5"`.

```toml
[project]
name = "example-project"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12.5"
dependencies = []
```

## Why

Why do I think this would be helpful?

- in `--app` mode I am not targeting to support a matrix of Python versions and package versions. I want to deploy my app in the most stable way possible. I should always get the same Python packages (uv.lock handles this) and the same Python version.

## Possible solutions

- Support adding version specifiers to uv init. E.g. `uv init --app --python ==3.12.5`
- When using `--app` mode default to locking in a single version of Python (my guess is that this is what most people want, but I could be wrong!).

---

_Comment by @zanieb on 2024-08-28 23:08_

Hi! Thanks for the clear description of what you're looking for.

It is specifically _not_ recommended to pin a single Python version in the `requires-python` field — it's against the intent of the metadata. Instead, you should use `uv python pin` which creates a `.python-version` file. We could / should create one of those in `uv init`. There's a separate issue for storing Python version pins in the `pyproject.toml` #4970 

---

_Label `projects` added by @zanieb on 2024-08-28 23:09_

---

_Comment by @SamEdwardes on 2024-08-29 14:53_

Thank you @zanieb! I think python pin fits my needs. I also like the idea of storing this in pyproject.toml so there is one less file floating around :)

---

_Closed by @SamEdwardes on 2024-08-29 14:53_

---

_Comment by @SamEdwardes on 2024-08-29 14:56_

I also like the idea of creating the .python-version file when uv init is in "app" mode. 

---

_Referenced in [astral-sh/uv#6821](../../astral-sh/uv/issues/6821.md) on 2024-08-29 16:44_

---

_Referenced in [astral-sh/uv#14505](../../astral-sh/uv/issues/14505.md) on 2025-07-08 12:27_

---
