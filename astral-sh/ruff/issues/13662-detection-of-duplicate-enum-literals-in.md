```yaml
number: 13662
title: Detection of duplicate enum literals in dictionary keys (F601)
type: issue
state: open
author: tsx
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-10-07T14:05:16Z
updated_at: 2024-10-07T15:43:21Z
url: https://github.com/astral-sh/ruff/issues/13662
synced_at: 2026-01-10T11:09:55Z
```

# Detection of duplicate enum literals in dictionary keys (F601)

---

_Issue opened by @tsx on 2024-10-07 14:05_

[multi-value-repeated-key-literal (F601)](https://docs.astral.sh/ruff/rules/multi-value-repeated-key-literal/#multi-value-repeated-key-literal-f601) only works with primitive literals like keys and strings. In our codebase we use enums extensively for increased type safety over strings, and that includes mappings of enum values to other values. I'd love ruff to find issues in those too.

This triggers F601:
```python
string_keys = {"a": 1, "a": 2}
```

This doesn't trigger but should:
```python
from enum import Enum
class E(Enum):
    A = "a"

enum_keys = {E.A: 1, E.A: 2}
```

(ruff 0.6.9)

Thanks for the awesome tool!

---

_Label `multifile-analysis` added by @MichaReiser on 2024-10-07 14:11_

---

_Label `type-inference` added by @MichaReiser on 2024-10-07 14:11_

---

_Label `rule` added by @MichaReiser on 2024-10-07 14:11_

---

_Comment by @MichaReiser on 2024-10-07 14:12_

That makes sense but isn't something that ruff can support today without proper multifile analysis (to support enums imported from other files) and type inference (to get the types of enum constants). 

---

_Comment by @tsx on 2024-10-07 15:00_

Would it make sense to make a separate rule that checks for identical expressions in keys? It can have false positives if the expression has side effects and changes value on every evaluation, but one can argue that using those as "keys" in a dictionary is very confusing and obscures the code intent.

---

_Comment by @MichaReiser on 2024-10-07 15:41_

Good point. We could possibly extend the [multi-value-repeated-key-variable](https://docs.astral.sh/ruff/rules/multi-value-repeated-key-variable/) rule to support identical attribute chains. The rule already supports simple names. But that would be a change in scope.

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-07 15:41_

---
