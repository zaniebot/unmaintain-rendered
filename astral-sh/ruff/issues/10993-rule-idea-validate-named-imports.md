```yaml
number: 10993
title: "Rule idea: Validate named imports"
type: issue
state: open
author: silverwind
labels:
  - type-inference
assignees: []
created_at: 2024-04-17T10:41:10Z
updated_at: 2024-10-24T14:33:03Z
url: https://github.com/astral-sh/ruff/issues/10993
synced_at: 2026-01-12T15:54:50Z
```

# Rule idea: Validate named imports

---

_@silverwind_

A rule to validate whether a named import exists on a imported file would be very useful, equivalent to [this rule](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/named.md) in JS. For example:

foo.py
```py
def foo:
  pass
```

bar.py
```py
from foo import foo, baz # error: named import 'baz' does not exist
```

---

_Label `multifile-analysis` added by @AlexWaygood on 2024-04-17 11:04_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
