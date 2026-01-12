```yaml
number: 18950
title: RUF033 fix removes necessary parentheses from around the default expression
type: issue
state: closed
author: dscorbett
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-06-26T01:24:16Z
updated_at: 2025-07-24T13:45:50Z
url: https://github.com/astral-sh/ruff/issues/18950
synced_at: 2026-01-12T15:54:56Z
```

# RUF033 fix removes necessary parentheses from around the default expression

---

_@dscorbett_

### Summary

The fix for [`post-init-default` (RUF033)](https://docs.astral.sh/ruff/rules/post-init-default/) introduces a syntax error when it removes necessary parentheses from around the default expression.

[Assignment expression](https://play.ruff.rs/108b3f90-4b8e-47ea-8526-779f760f41c7):
```console
$ cat >ruf033_1.py <<'# EOF'
from dataclasses import dataclass
@dataclass
class Foo:
    def __post_init__(self, bar: int = (x := 1)) -> None:
        pass
# EOF

$ ruff check --isolated ruf033_1.py --select RUF033 --unsafe-fixes --diff 2>&1 | grep error:
error: Fix introduced a syntax error. Reverting all changes.
```

[Multiline expression](https://play.ruff.rs/dc54ed79-fdf5-415b-9fc7-9f8d97076da5):
```console
$ cat >ruf033_2.py <<'# EOF'
from dataclasses import dataclass
@dataclass
class Foo:
    def __post_init__(self, bar: int = (-
        1
    )) -> None:
        pass
# EOF

$ ruff check --isolated ruf033_2.py --select RUF033 --unsafe-fixes --diff 2>&1 | grep error: 
error: Fix introduced a syntax error. Reverting all changes.
```

### Version

ruff 0.12.0 (87f0feb21 2025-06-17)

---

_Label `bug` added by @ntBre on 2025-06-26 01:54_

---

_Label `fixes` added by @ntBre on 2025-06-26 01:54_

---

_Label `help wanted` added by @ntBre on 2025-06-26 01:54_

---

_Closed by @ntBre on 2025-07-24 13:45_

---
