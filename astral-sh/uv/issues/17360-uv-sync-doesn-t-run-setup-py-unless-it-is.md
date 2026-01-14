```yaml
number: 17360
title: "`uv sync` doesn't run setup.py unless it is manually touched/modified"
type: issue
state: open
author: kunaltyagi
labels:
  - question
assignees: []
created_at: 2026-01-08T14:20:29Z
updated_at: 2026-01-14T09:04:52Z
url: https://github.com/astral-sh/uv/issues/17360
synced_at: 2026-01-14T09:35:14Z
```

# `uv sync` doesn't run setup.py unless it is manually touched/modified

---

_@kunaltyagi_

### Summary

I can not list extension files under `tool.setuptools.ext-modules` in `pyproject.toml` for uv to track, so I expect `uv sync` to take extra time on every call to see if `setup.py` needs to update any extension. However, even when there's work to be done, `uv sync` doesn't call `setup.py` unless I `touch`/modify it specifically.

Any solution/fix would be appreciated

### Platform

Linux and Windows

### Version

0.9.5

### Python version

3.12.12

---

_Label `bug` added by @kunaltyagi on 2026-01-08 14:20_

---

_Comment by @zanieb on 2026-01-08 14:40_

Please see https://docs.astral.sh/uv/reference/settings/#cache-keys

---

_Label `bug` removed by @zanieb on 2026-01-08 14:41_

---

_Label `question` added by @zanieb on 2026-01-08 14:41_

---

_Comment by @kunaltyagi on 2026-01-08 16:04_

I don't think `dir` is working as expected. Files changed under the directory don't trigger a call to `setup.py`

---

_Comment by @konstin on 2026-01-08 18:19_

Can you provide a minimal reproducible example for how it fails?

---

_Comment by @kunaltyagi on 2026-01-14 09:04_

Let me create one

---
