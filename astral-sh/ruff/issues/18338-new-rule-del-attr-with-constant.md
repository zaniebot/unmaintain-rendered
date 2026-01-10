---
number: 18338
title: "New rule `del-attr-with-constant`"
type: issue
state: open
author: janosh
labels:
  - rule
assignees: []
created_at: 2025-05-27T18:01:04Z
updated_at: 2025-05-28T11:06:58Z
url: https://github.com/astral-sh/ruff/issues/18338
synced_at: 2026-01-10T01:22:59Z
---

# New rule `del-attr-with-constant`

---

_Issue opened by @janosh on 2025-05-27 18:01_

### Summary

new sibling for [B009](https://docs.astral.sh/ruff/rules/get-attr-with-constant) (`get-attr-with-constant`):

```py
# bad
delattr(cls, "_repr_mimebundle_")

# good
del cls._repr_mimebundle_
```

---

_Referenced in [janosh/pymatviz#299](../../janosh/pymatviz/pulls/299.md) on 2025-05-27 18:02_

---

_Label `rule` added by @MichaReiser on 2025-05-28 07:08_

---

_Comment by @fennr on 2025-05-28 10:58_

@MichaReiser can you assign issue to me?

---

_Comment by @MichaReiser on 2025-05-28 11:06_

It could make sense to open a request for this rule upstream before adding it to Ruff, to avoid re-coding in the future (We can't add it to the bugbear category)

---

_Referenced in [PyCQA/flake8-bugbear#513](../../PyCQA/flake8-bugbear/issues/513.md) on 2025-05-28 12:00_

---
