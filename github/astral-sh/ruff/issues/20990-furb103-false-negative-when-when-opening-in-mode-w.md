---
number: 20990
title: "FURB103 false negative when when opening in mode \"w+\""
type: issue
state: open
author: berland
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-20T11:53:21Z
updated_at: 2025-10-21T06:59:19Z
url: https://github.com/astral-sh/ruff/issues/20990
synced_at: 2026-01-07T13:12:16-06:00
---

# FURB103 false negative when when opening in mode "w+"

---

_Issue opened by @berland on 2025-10-20 11:53_

### Summary

We found code that did:

```python
with open("foo.sh", "w+", encoding="utf-8") as f:
    f.write("something")
```
which is not picked up by [write-whole-file (FURB103)](https://docs.astral.sh/ruff/rules/write-whole-file/) due to the `+` in the mode argument, making the file opened both in write and read mode. But the code is only writing, meaning the presence of `+` is superfluous. Should the superfluous `+` be detected by FURB103 or should some other rule catch that?

---

_Comment by @ntBre on 2025-10-20 14:10_

That's interesting, thanks for the report! It does seem a bit tricky for FURB103 to be sure the `+` isn't used, so maybe a separate rule like `unused-open-modes` is a good idea. We have [redundant-open-modes (UP015)](https://docs.astral.sh/ruff/rules/redundant-open-modes/#redundant-open-modes-up015), but I don't think the `+` is redundant in this case, just unused.

Anyway, I'm curious what others think!

---

_Label `rule` added by @ntBre on 2025-10-20 14:10_

---

_Label `needs-decision` added by @ntBre on 2025-10-20 14:10_

---

_Comment by @amyreese on 2025-10-21 00:21_

I agree that this feels like it belongs in a new rule, that ensures both a read/write variant method is used when opened with "+" mode.

---
