```yaml
number: 16535
title: use named expressions in conditions
type: issue
state: closed
author: dorschw
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-03-06T14:33:46Z
updated_at: 2025-03-06T17:41:35Z
url: https://github.com/astral-sh/ruff/issues/16535
synced_at: 2026-01-12T15:54:55Z
```

# use named expressions in conditions

---

_@dorschw_

### Summary

Similar to Sourcery's [use-named-expression](https://docs.sourcery.ai/Coding-Assistant/Reference/Rules-and-In-Line-Suggestions/Python/Default-Rules/use-named-expression/): 

```diff
- env_base = os.environ.get("PYTHONUSERBASE", None)
- if env_base:
+ if env_base := os.environ.get("PYTHONUSERBASE", None):
    return env_base
```

I'm aware [not everyone is fan of walrus](https://github.com/astral-sh/ruff/issues/3464), but I find it useful.

---

_Renamed from "(🎁) Encourage using named expressions in conditions" to "(🎁) use named expressions in conditions" by @dorschw on 2025-03-06 15:37_

---

_Label `rule` added by @ntBre on 2025-03-06 16:28_

---

_Label `needs-decision` added by @ntBre on 2025-03-06 16:28_

---

_Renamed from "(🎁) use named expressions in conditions" to "use named expressions in conditions" by @MichaReiser on 2025-03-06 17:40_

---

_Closed by @MichaReiser on 2025-03-06 17:41_

---
