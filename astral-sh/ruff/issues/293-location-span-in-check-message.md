```yaml
number: 293
title: Location span in check / message
type: issue
state: closed
author: Seamooo
labels: []
assignees: []
created_at: 2022-09-30T17:58:19Z
updated_at: 2022-10-02T16:50:43Z
url: https://github.com/astral-sh/ruff/issues/293
synced_at: 2026-01-10T15:56:05Z
```

# Location span in check / message

---

_Issue opened by @Seamooo on 2022-09-30 17:58_

A nice-to-have for editor integration would be some sort of span indicated by the check or message.
The only information given at this stage is the start of the check, so the only inferred span possible is a single character.

Admittedly this would require additional work for every current check implemented, but if there's an agreed upon way of going about this I'm happy to put time into it



---

_Comment by @charliermarsh on 2022-09-30 18:03_

One challenge here is that unlike Python's AST module, RustPython doesn't include end locations for nodes, only for tokens.

---

_Closed by @charliermarsh on 2022-10-02 16:50_

---
