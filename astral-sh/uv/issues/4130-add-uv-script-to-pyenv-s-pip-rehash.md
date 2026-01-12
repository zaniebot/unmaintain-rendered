```yaml
number: 4130
title: "Add uv script to pyenv's pip-rehash"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - compatibility
assignees: []
created_at: 2024-06-07T13:47:06Z
updated_at: 2024-06-08T14:16:43Z
url: https://github.com/astral-sh/uv/issues/4130
synced_at: 2026-01-12T15:58:48Z
```

# Add uv script to pyenv's pip-rehash

---

_@zanieb_

See https://github.com/pyenv/pyenv/tree/master/pyenv.d/exec/pip-rehash

This is used to recalculate the environment on installation and there are shims for `pip`, `conda`, and `easy_install`.

This will resolve the problems described at https://github.com/astral-sh/uv/issues/1586

Tracking in `pyenv` at https://github.com/pyenv/pyenv/issues/2979

---

_Label `compatibility` added by @zanieb on 2024-06-07 13:48_

---

_Comment by @zanieb on 2024-06-08 14:15_

They've suggested we should provide a pyenv plugin that adds this functionality. If someone is interested in exploring that please let us know!

---

_Label `help wanted` added by @zanieb on 2024-06-08 14:16_

---
