```yaml
number: 15025
title: "--bare with --python= should also create .python-version"
type: issue
state: closed
author: jamesdeluk
labels:
  - question
assignees: []
created_at: 2025-08-02T06:58:40Z
updated_at: 2025-08-02T14:58:02Z
url: https://github.com/astral-sh/uv/issues/15025
synced_at: 2026-01-12T16:02:02Z
```

# --bare with --python= should also create .python-version

---

_@jamesdeluk_

### Summary

Say I want to create a project using Python 3.11. I can do this with `uv init --python=3.11`. This creates `pyproject.toml` with `requires-python = ">=3.11"` and `.python-version` with `3.11`. If I `uv sync` or similar, it creates a venv with Python 3.11. Perfect.

However, typically I only use `--bare`, as I don't want the sample Python file or a Git repo etc, so I use `uv init --bare --python=3.11`. Now, if I `uv sync` or similar, it creates a venv with Python 3.13 (or whichever is latest). Not perfect.

I propose the `--bare` command, if used with `--python=`, should also create the `.python-version` file, to ensure the argument is respected.

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.4

### Python version

This is the issue :)

---

_Label `bug` added by @jamesdeluk on 2025-08-02 06:58_

---

_Comment by @zanieb on 2025-08-02 12:02_

I think you want `uv init --pin-python --bare`. `--python` can't be opt-in to emitting the pin because that's also used for the `requires-python` range.

---

_Label `bug` removed by @zanieb on 2025-08-02 12:02_

---

_Label `question` added by @zanieb on 2025-08-02 12:02_

---

_Comment by @jamesdeluk on 2025-08-02 13:20_

Ah, that seems like it might work - I'll give it a go, thanks! I didn't realise that option existed as it doesn't seem to appear in `uv init --help` or https://docs.astral.sh/uv/reference/cli/#uv-init

---

_Comment by @zanieb on 2025-08-02 13:23_

We don't show default options, but we usually have inverses for `--no-<flag>` and `--<flag>`

---

_Comment by @jamesdeluk on 2025-08-02 13:54_

That's good to know!

---

_Closed by @zanieb on 2025-08-02 14:58_

---
