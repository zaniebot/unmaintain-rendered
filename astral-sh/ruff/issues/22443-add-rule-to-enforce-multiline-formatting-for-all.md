```yaml
number: 22443
title: "Add rule to enforce multiline formatting for `__all__` declarations"
type: issue
state: open
author: Danipulok
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2026-01-07T18:55:04Z
updated_at: 2026-01-08T08:24:49Z
url: https://github.com/astral-sh/ruff/issues/22443
synced_at: 2026-01-10T11:10:00Z
```

# Add rule to enforce multiline formatting for `__all__` declarations

---

_Issue opened by @Danipulok on 2026-01-07 18:55_

### Summary
Add a new rule that enforces multiline formatting for `__all__` declarations when they contain multiple items

### Example

**Original code:**
```python
__all__ = ["a", "b", "c"]
```

**Changed to:**
```python
__all__ = [
    "a",
    "b",
    "c",
]
```

### Problem
Currently there are multiple ways to write `__all__`. I've been using `ruff` for two years (I guess) and I would really like to have a rule that makes only one way to write `__all__`. It would be really convenient to enforce it in large code bases 

### Rationale
- Improves readability for modules with many exported items
- Reduces merge conflicts when adding/removing exports
- Makes diffs cleaner - each addition/removal is a single line
- Only one way to format `__all__`

### Configuration options (suggestion)
- `min-items`: Minimum number of items to trigger multiline formatting (default: 2)
- `always-multiline`: Force multiline even for single items (default: false)

### Related
ruf022: https://docs.astral.sh/ruff/rules/unsorted-dunder-all/#unsorted-dunder-all-ruf022

---

_Renamed from "Add rule to enforce multiline formatting for `__all__` declarations" to "[RuleRequest] Add rule to enforce multiline formatting for `__all__` declarations" by @Danipulok on 2026-01-07 18:55_

---

_Renamed from "[RuleRequest] Add rule to enforce multiline formatting for `__all__` declarations" to "Add rule to enforce multiline formatting for `__all__` declarations" by @MichaReiser on 2026-01-08 08:24_

---

_Label `rule` added by @MichaReiser on 2026-01-08 08:24_

---

_Label `needs-decision` added by @MichaReiser on 2026-01-08 08:24_

---
