```yaml
number: 16308
title: "Respect `UV_VENV_SEED` when using `uv sync`"
type: issue
state: open
author: treyhunner
labels:
  - enhancement
assignees: []
created_at: 2025-10-15T04:33:34Z
updated_at: 2025-12-18T19:59:58Z
url: https://github.com/astral-sh/uv/issues/16308
synced_at: 2026-01-12T16:02:28Z
```

# Respect `UV_VENV_SEED` when using `uv sync`

---

_@treyhunner_

### Summary

When the `UV_VENV_SEED` environment variable is set, creating the `.venv` directory with `uv venv` will install the `pip` command in the virtual environment.

When using the  `uv sync` command to create the environment, the `.venv` directory will not have a `pip` command in it.

### Example

Currently:

```bash
$ UV_VENV_SEED=1 uv sync
$ ls .venv/bin/pip
ls: cannot access '.venv/bin/pip': No such file or directory
```

After this change:


```bash
$ UV_VENV_SEED=1 uv sync
$ ls .venv/bin/pip
.venv/bin/pip
```

---

_Label `enhancement` added by @treyhunner on 2025-10-15 04:33_

---

_Comment by @woutervh on 2025-10-25 17:22_

As a workaround, you can add pip to your dependencies  if you already have a pyproject.toml.

---

_Comment by @zanieb on 2025-10-25 17:58_

What's your use-case?

---

_Comment by @treyhunner on 2025-12-18 19:59_

I believe I was wanting to activate the virtual environment and play with changing packages within it using `pip` and realized that there was no `pip` in the virtual environment that uv had made even though I had `UV_VENV_SEED=1` set.

---
