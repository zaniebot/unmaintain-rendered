```yaml
number: 16502
title: N806, N815, and N816 ignore case patterns
type: issue
state: open
author: dscorbett
labels:
  - bug
  - help wanted
assignees: []
created_at: 2025-03-04T16:28:21Z
updated_at: 2025-03-04T18:33:50Z
url: https://github.com/astral-sh/ruff/issues/16502
synced_at: 2026-01-12T15:54:55Z
```

# N806, N815, and N816 ignore case patterns

---

_@dscorbett_

### Summary

[`non-lowercase-variable-in-function` (N806)](https://docs.astral.sh/ruff/rules/non-lowercase-variable-in-function/), [`mixed-case-variable-in-class-scope` (N815)](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-class-scope/), and [`mixed-case-variable-in-global-scope` (N816)](https://docs.astral.sh/ruff/rules/mixed-case-variable-in-global-scope/) have false negatives for variables assigned in case patterns. Each of the following examples includes `tP` as a true positive to show that the rule is running plus one or two false negatives.

N806:
```console
$ cat >n806.py <<'# EOF'
def f():
    tP = 0
    match tP:
        case fN: ...
# EOF

$ ruff --isolated check --output-format concise --select N806 n806.py
n806.py:2:5: N806 Variable `tP` in function should be lowercase
Found 1 error.
```

N815:
```console
$ cat >n815.py <<'# EOF'
class C:
    tP = 0
    match tP:
        case int(fN): ...
# EOF

$ ruff --isolated check --output-format concise --select N815 n815.py
n815.py:2:5: N815 Variable `tP` in class scope should not be mixedCase
Found 1 error.
```

N816:
```console
$ cat >n816.py <<'# EOF'
tP = 0
match tP:
    case fN1 as fN2: ...
# EOF

$ ruff --isolated check --output-format concise --select N816 n816.py
n816.py:1:1: N816 Variable `tP` in global scope should not be mixedCase
Found 1 error.
```

### Version

ruff 0.9.9 (091d0af2a 2025-02-28)

---

_Label `bug` added by @ntBre on 2025-03-04 18:33_

---

_Label `help wanted` added by @ntBre on 2025-03-04 18:33_

---
