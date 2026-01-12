```yaml
number: 15806
title: "New rule prohibiting modification of `sys.path`"
type: issue
state: open
author: vraintanakarei
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-29T09:08:17Z
updated_at: 2025-02-02T18:47:54Z
url: https://github.com/astral-sh/ruff/issues/15806
synced_at: 2026-01-12T15:54:55Z
```

# New rule prohibiting modification of `sys.path`

---

_@vraintanakarei_

### Description

I think modification of `sys.path` is a bad practice because it breaks type checking, code completion, code highlighting, etc.
Therefore, I would like to ban it in my team, but I could not find a rule to check it.

If such a rule does not already exist, please consider adding a rule prohibiting changes to `sys.path`.
For my purpose, these codes should be prohibited with a few exceptions.

- `sys.path.append(...)`
- `sys.path.extend(...)`
- `sys.path = ...`
- etc.

Thank you very much.

---

_Label `rule` added by @MichaReiser on 2025-02-02 18:45_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-02 18:45_

---

_Comment by @MichaReiser on 2025-02-02 18:47_

This seems reasonable, although I'd prefer to wait to add it until https://github.com/astral-sh/ruff/issues/1774 is complete because we should then have a good place for "restriction" rules -- rules that disallow a valid code pattern mainly because "We don't like it for reasons"

---
