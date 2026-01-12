```yaml
number: 3512
title: PLE1507 false positives
type: issue
state: closed
author: davidnemec
labels:
  - bug
assignees: []
created_at: 2023-03-14T14:27:54Z
updated_at: 2023-03-14T17:46:36Z
url: https://github.com/astral-sh/ruff/issues/3512
synced_at: 2026-01-12T15:54:43Z
```

# PLE1507 false positives

---

_@davidnemec_

Hi :wave:
with latest version that adds PLE1507 I'm getting errors with a following usage. But I would say they are false positives.

```python
import os

def dummy(env_prefix: str = ''):
    os.getenv(f'{env_prefix}NAME', '') # fail
    os.getenv(env_prefix + 'NAME', '') # fail
```

---

_Comment by @charliermarsh on 2023-03-14 14:30_

Yeah definitely.

---

_Label `bug` added by @charliermarsh on 2023-03-14 14:30_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-14 14:30_

---

_Comment by @charliermarsh on 2023-03-14 14:30_

I thought we were pretty conservative with those but I must've overlooked, will fix.

---

_Closed by @charliermarsh on 2023-03-14 17:46_

---
