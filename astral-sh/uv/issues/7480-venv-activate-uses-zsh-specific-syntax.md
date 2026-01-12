```yaml
number: 7480
title: venv/activate uses zsh-specific syntax
type: issue
state: open
author: prez
labels:
  - bug
assignees: []
created_at: 2024-09-17T23:43:27Z
updated_at: 2025-01-15T17:37:34Z
url: https://github.com/astral-sh/uv/issues/7480
synced_at: 2026-01-12T15:59:14Z
```

# venv/activate uses zsh-specific syntax

---

_@prez_

I'm using the yash shell, which is strictly POSIX-compliant and errors out on `. .venv/bin/activate`.

The snippet using a zsh-specific extension is `SCRIPT_PATH="${(%):-%x}"` in line 34.

Please take a look at the original issue (I filed this with yash at first) for some context and a potential fix: https://github.com/magicant/yash/issues/62

Thank you.

---

_Label `bug` added by @charliermarsh on 2024-09-18 02:47_

---

_Closed by @Gankra on 2025-01-08 18:12_

---

_Closed by @Gankra on 2025-01-08 18:12_

---

_Comment by @notatallshaw on 2025-01-11 18:20_

Fix was reverted

---

_Reopened by @notatallshaw on 2025-01-11 18:20_

---

_Assigned to @Gankra by @Gankra on 2025-01-15 17:37_

---
