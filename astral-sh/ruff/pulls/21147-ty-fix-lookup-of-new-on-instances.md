```yaml
number: 21147
title: "[ty] Fix lookup of `__new__` on instances"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/fix-dunder-new-lookup
created_at: 2025-10-30T16:54:25Z
updated_at: 2025-10-30T17:42:48Z
url: https://github.com/astral-sh/ruff/pull/21147
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Fix lookup of `__new__` on instances

---

_Pull request opened by @AlexWaygood on 2025-10-30 16:54_

## Summary

We weren't correctly modeling it as a `staticmethod` in all cases, leading us to incorrectly infer that the `cls` argument would be bound if it was accessed on an instance (rather than the class object).

## Test Plan

Added mdtests that fail on `main`. The primer output also looks good!


---

_Label `ty` added by @AlexWaygood on 2025-10-30 16:54_

---

_Comment by @github-actions[bot] on 2025-10-30 16:56_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-30 16:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pip (https://github.com/pypa/pip)
- src/pip/_vendor/rich/style.py:478:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- src/pip/_vendor/rich/style.py:630:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- src/pip/_vendor/rich/style.py:653:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- src/pip/_vendor/rich/style.py:676:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- src/pip/_vendor/rich/style.py:734:41: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 582 diagnostics
+ Found 577 diagnostics

rich (https://github.com/Textualize/rich)
- rich/style.py:478:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- rich/style.py:630:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- rich/style.py:653:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- rich/style.py:676:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- rich/style.py:734:41: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 321 diagnostics
+ Found 316 diagnostics

zope.interface (https://github.com/zopefoundation/zope.interface)
- src/zope/interface/interface.py:739:16: error[missing-argument] No arguments provided for required parameters `bases`, `attrs` of bound method `__new__`
+ src/zope/interface/interface.py:739:16: error[missing-argument] No arguments provided for required parameters `name`, `bases`, `attrs` of function `__new__`

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/Hash/CMAC.py:173:28: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 1775 diagnostics
+ Found 1774 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/physics/secondquant.py:1983:45: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 2, got 3
- Found 14031 diagnostics
+ Found 14030 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-10-30 17:08_

---

_Review requested from @carljm by @AlexWaygood on 2025-10-30 17:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-10-30 17:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-10-30 17:08_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-10-30 17:08_

---

_Label `bug` added by @AlexWaygood on 2025-10-30 17:09_

---

_Renamed from "[ty] Fix lookup of `__new__`" to "[ty] Fix lookup of `__new__` on instances" by @AlexWaygood on 2025-10-30 17:09_

---

_@carljm approved on 2025-10-30 17:22_

---

_Merged by @AlexWaygood on 2025-10-30 17:42_

---

_Closed by @AlexWaygood on 2025-10-30 17:42_

---

_Branch deleted on 2025-10-30 17:42_

---
