```yaml
number: 2550
title: "Support `enum.global_enum`"
type: issue
state: open
author: dscorbett
labels:
  - wish
  - enums
assignees: []
created_at: 2026-01-17T20:15:58Z
updated_at: 2026-01-18T10:54:45Z
url: https://github.com/astral-sh/ty/issues/2550
synced_at: 2026-01-18T11:18:08Z
```

# Support `enum.global_enum`

---

_@dscorbett_

### Summary

[`enum.global_enum`](https://docs.python.org/3/library/enum.html#enum.global_enum) adds an enum’s members to the global namespace. ty reports an error for any use of global constants defined that way. [Example](https://play.ty.dev/e082cbf9-9bb7-4e2e-bf54-3188abb812ee):
```console
$ cat >module.py <<'# EOF'
from enum import Enum, auto, global_enum


@global_enum
class E(Enum):
    X = auto()
# EOF

$ cat >main.py <<'# EOF'
from module import X
# EOF

$ ty check main.py --output-format concise
main.py:1:20: error[unresolved-import] Module `module` has no member `X`
Found 1 diagnostic
```

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Label `enums` added by @AlexWaygood on 2026-01-17 23:50_

---

_Comment by @AlexWaygood on 2026-01-18 00:05_

Thanks! I'd love to support this at some point, but (to help with our prioritisation here): do any other type checkers support this feature currently?

---

_Comment by @dscorbett on 2026-01-18 01:01_

I don’t know of any.
```console
$ mypy main.py module.py
main.py:1: error: Module "module" has no attribute "X"  [attr-defined]
Found 1 error in 1 file (checked 2 source files)

$ pyrefly check --output-format min-text main.py module.py
ERROR main.py:1:20-21: Could not import `X` from `module` [missing-module-attribute]
 INFO 1 error

$ pyright main.py module.py | sed 's:/.*/\([^/]*\.py\):\1:'
main.py
  main.py:1:20 - error: "X" is unknown import symbol (reportAttributeAccessIssue)
1 error, 0 warnings, 0 informations

$ zuban check main.py module.py
main.py:1: error: Module "module" has no attribute "X"  [attr-defined]
Found 1 error in 1 file (checked 2 source files)
```

---

_Comment by @AlexWaygood on 2026-01-18 01:02_

Thanks, that's helpful context

---

_Label `wish` added by @AlexWaygood on 2026-01-18 01:02_

---

_Comment by @MichaReiser on 2026-01-18 10:54_

Related to [this](https://github.com/astral-sh/ruff/issues/7695) Ruff issue

---
