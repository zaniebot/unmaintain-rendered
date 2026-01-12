```yaml
number: 21142
title: B006 doesn’t flag mutable defaults when passed via a named constant
type: issue
state: open
author: yehiahesham
labels:
  - rule
  - preview
assignees: []
created_at: 2025-10-30T15:09:30Z
updated_at: 2025-10-30T18:25:16Z
url: https://github.com/astral-sh/ruff/issues/21142
synced_at: 2026-01-12T15:54:57Z
```

# B006 doesn’t flag mutable defaults when passed via a named constant

---

_@yehiahesham_

Ruff’s B006 (mutable-argument-default) correctly flags obvious mutable defaults written inline (e.g., {}, [], set()), but it does not warn when the default is a reference to a module-level named constant that is a mutable object (e.g., a set). This yields a false negative and can lead to shared mutable state across calls/instances without any lint warning.

This appears to be a false negative due to B006’s syntax-local detection not resolving identifiers to determine mutability.

# Environment
Ruff version: ruff 0.9.6
Python version: Python 3.13.7
pyproject.toml file Config has: select = ["ALL"]; B006 not ignored
```
[tool.ruff.lint]
select = ["ALL"]
exclude = [".github/**"]
```

# Minimal Reproducer
```
Python
# Module-level mutable “constant”
MY_SET = {"ABC", "DEF"}  # plain set

# NOT triggering B006
def func_A(s: set[str] = MY_SET):
    return s

# Triggering B006
def func_B(s: set[str] = {"ABC", "DEF"}):
    return s
```
# Observed Behavior

 - Ruff flags `func_B` with B006 (as expected).
 - Ruff does not flag `func_A` even though they share the same mutable default object via `MY_SET`.


# Expected Behavior

- B006 should flag mutable default arguments even when the default is provided via a referenced identifier bound at module scope to a mutable object (list/set/dict). 


---

_Comment by @ntBre on 2025-10-30 18:25_

I think it would make sense to extend the rule in this way, in preview.

---

_Label `rule` added by @ntBre on 2025-10-30 18:25_

---

_Label `preview` added by @ntBre on 2025-10-30 18:25_

---
