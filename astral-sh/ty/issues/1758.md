```yaml
number: 1758
title: "Auto-import should probably union `__all__` definitions in some cases"
type: issue
state: open
author: BurntSushi
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-04T15:55:59Z
updated_at: 2025-12-04T15:55:59Z
url: https://github.com/astral-sh/ty/issues/1758
synced_at: 2026-01-10T01:56:41Z
```

# Auto-import should probably union `__all__` definitions in some cases

---

_Issue opened by @BurntSushi on 2025-12-04 15:55_

Ref: https://github.com/astral-sh/ruff/pull/21779/files/d314aa859dcf9a6ffb006a594d7ca0431dd4fab2#r2586995420

Specifically, the auto-import AST scanner will assume that all conditionals are true. This could lead to some symbols not being declared as exported when they ought to be.

Instead, we might consider preferring false positives to false negatives by unioning `__all__` definitions in such cases. We might suggest some things incorrectly, but thisis probably better than _not_ suggesting correct symbols.

---

_Label `server` added by @BurntSushi on 2025-12-04 15:56_

---

_Label `completions` added by @BurntSushi on 2025-12-04 15:56_

---
