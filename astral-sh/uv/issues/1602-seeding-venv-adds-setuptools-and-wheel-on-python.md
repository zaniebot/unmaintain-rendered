```yaml
number: 1602
title: Seeding venv adds setuptools and wheel on Python 3.12
type: issue
state: closed
author: henryiii
labels:
  - good first issue
  - performance
  - compatibility
assignees: []
created_at: 2024-02-17T17:13:47Z
updated_at: 2024-02-18T21:20:22Z
url: https://github.com/astral-sh/uv/issues/1602
synced_at: 2026-01-12T15:58:30Z
```

# Seeding venv adds setuptools and wheel on Python 3.12

---

_@henryiii_

If you use `--seed` on a Python 3.12 venv, you still get setuptools and wheel, while both virutalenv and venv have dropped them if using Python 3.12 or newer. This causes `uv venv -p 3.12 --seed` (524ms) to be slower than `virtualenv .venv` (360ms), which is only adding `pip`.


---

_Comment by @zanieb on 2024-02-17 17:14_

Thanks! We can drop them too.

---

_Label `good first issue` added by @zanieb on 2024-02-17 17:14_

---

_Label `compatibility` added by @zanieb on 2024-02-17 17:14_

---

_Label `performance` added by @AlexWaygood on 2024-02-17 17:18_

---

_Comment by @charliermarsh on 2024-02-17 17:53_

Oh wow!

---

_Closed by @zanieb on 2024-02-18 21:20_

---
