```yaml
number: 2550
title: "Support `enum.global_enum`"
type: issue
state: open
author: dscorbett
labels:
  - enums
assignees: []
created_at: 2026-01-17T20:15:58Z
updated_at: 2026-01-18T00:05:43Z
url: https://github.com/astral-sh/ty/issues/2550
synced_at: 2026-01-18T00:17:55Z
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
