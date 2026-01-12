```yaml
number: 10807
title: "[`flake8-pyi`] Various improvements to PYI034"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: metaclass-improvements
created_at: 2024-04-06T21:33:24Z
updated_at: 2024-04-06T23:15:50Z
url: https://github.com/astral-sh/ruff/pull/10807
synced_at: 2026-01-12T15:55:33Z
```

# [`flake8-pyi`] Various improvements to PYI034

---

_@AlexWaygood_

## Summary

Currently, the logic for PYI034 is pretty-much copied straight from the original Y034 implementation in flake8-pyi. As such, the implementation reflects the limited semantic analysis that flake8-pyi is capable of -- but Ruff, with its more sophisticated semantic model, can do better here.

Specific improvements made:
- `is_metaclass` now recognises that subclasses of subclasses of `type`/`enum.EnumMeta`/`abc.ABCMeta` are also metaclasses. (The current logic only recognises direct subclasses of these subclasses as being metaclasses. Any metaclass should be excluded from PYI034, as PEP 673 specifies that no methods in metaclasses may use `Self` in annotations.)
- Subclasses of subclasses of `Iterator[T]` are also flagged if their `__iter__` methods are annotated as returning `Iterator[T]` rather than `Self`. Currently only direct subclasses of `Iterator` are flagged. 
- Subclasses of subclasses of `AsyncIterator[T]` are also flagged if their `__aiter__` methods are annotated as returning `AsyncIterator[T]` rather than `Self`. Currently only direct subclasses of `AsyncIterator` are flagged.

## Test Plan

`cargo test`.


---

_Comment by @github-actions[bot] on 2024-04-06 21:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @AlexWaygood on 2024-04-06 22:02_

---

_@charliermarsh reviewed on 2024-04-06 23:11_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:224 on 2024-04-06 23:11_

IIRC this will recursively resolve subclasses (within the file). I'm not sure if that's intended in those case or not given the bullet in your PR summary.

---

_@charliermarsh approved on 2024-04-06 23:11_

Great!

---

_@AlexWaygood reviewed on 2024-04-06 23:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:224 on 2024-04-06 23:12_

Superclasses rather than subclasses, right? ;)

But yup, that's what I want! Sorry if theh PR summary was unclear

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:224 on 2024-04-06 23:15_

Uhh yes superclasses.

---

_@charliermarsh reviewed on 2024-04-06 23:15_

---

_@charliermarsh reviewed on 2024-04-06 23:15_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:224 on 2024-04-06 23:15_

Oh right, "The current logic" refers to `main`, not the current logic of _this_ PR. Of course.

---

_Merged by @AlexWaygood on 2024-04-06 23:15_

---

_Closed by @AlexWaygood on 2024-04-06 23:15_

---

_Branch deleted on 2024-04-06 23:15_

---
