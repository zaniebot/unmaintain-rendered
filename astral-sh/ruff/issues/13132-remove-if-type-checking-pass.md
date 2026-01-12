```yaml
number: 13132
title: "Remove `if TYPE_CHECKING: pass`"
type: issue
state: closed
author: hauntsaninja
labels: []
assignees: []
created_at: 2024-08-28T00:59:33Z
updated_at: 2024-08-28T02:28:07Z
url: https://github.com/astral-sh/ruff/issues/13132
synced_at: 2026-01-12T15:54:52Z
```

# Remove `if TYPE_CHECKING: pass`

---

_@hauntsaninja_

You can get these as a result of F401 cleanup. It would be nice for the F401 fix to automatically do this (or potentially this could be its own rule)

---

_Comment by @charliermarsh on 2024-08-28 01:19_

I think this might exist already as https://docs.astral.sh/ruff/rules/empty-type-checking-block/.

---

_Comment by @hauntsaninja on 2024-08-28 01:33_

Ahhhh I should have --select ALL -ed it, sorry for the noise!

---

_Closed by @hauntsaninja on 2024-08-28 01:33_

---

_Comment by @charliermarsh on 2024-08-28 02:28_

Haha no worries. Sometimes I just drop code in the playground and enable `ALL` to see if we already have diagnostics for something.

---
