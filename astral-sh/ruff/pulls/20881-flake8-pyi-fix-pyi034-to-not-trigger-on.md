```yaml
number: 20881
title: "[`flake8-pyi`] Fix PYI034 to not trigger on metaclasses (`PYI034`)"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-20781
created_at: 2025-10-15T04:05:44Z
updated_at: 2025-10-24T13:47:11Z
url: https://github.com/astral-sh/ruff/pull/20881
synced_at: 2026-01-12T15:57:11Z
```

# [`flake8-pyi`] Fix PYI034 to not trigger on metaclasses (`PYI034`)

---

_@danparizher_

## Summary

Fixes #20781


---

_Review requested from @AlexWaygood by @danparizher on 2025-10-15 04:05_

---

_Comment by @github-actions[bot] on 2025-10-15 04:13_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI034.pyi`:261 on 2025-10-15 12:31_

This test passes on `main`. Are you able to add a test that does not pass on `main`, but does pass on this PR branch?

---

_@AlexWaygood requested changes on 2025-10-15 12:31_

Thanks!

---

_@danparizher reviewed on 2025-10-15 22:07_

---

_Review comment by @danparizher on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI034.pyi`:261 on 2025-10-15 22:07_

I've updated the test case to use `type(Protocol)` which triggers `IsMetaclass::Maybe`. The fix skips `IsMetaclass::Yes` and `IsMetaclass::Maybe` cases as required by PEP 673.

---

_@AlexWaygood reviewed on 2025-10-15 22:09_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI034.pyi`:261 on 2025-10-15 22:09_

Thank you! Before we land this, I'd still like to wait for a bit more information on the issue to see if this is sufficient to fix the problem the user was experiencing

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/flake8_pyi/PYI034.pyi`:261 on 2025-10-21 13:20_

It sounds like the consensus on the issue is that we should implement option (2) I outlined in https://github.com/astral-sh/ruff/issues/20781#issuecomment-3410351921. @danparizher, are you interested in updating this PR to include that change? I think we should probably work the heuristic into the `ruff_python_semantic::analyze::class::is_metaclass()` function. If we see that the class (or any of its known superclasses) has a `__new__` method that matches that heuristic, we should declare that the class is "maybe a metaclass" (but not "definitely a metaclass").

---

_@AlexWaygood reviewed on 2025-10-21 13:20_

---

_Review requested from @AlexWaygood by @danparizher on 2025-10-24 03:19_

---

_@AlexWaygood approved on 2025-10-24 10:32_

Thanks, I pushed a few edits. The implementation LGTM now.

@ntBre, I think you mentioned on the issue that it might be good to document this heuristic? That does seem like it might be worthwhile. I'll punt this back to you now, if that's okay 😄

---

_Review requested from @ntBre by @AlexWaygood on 2025-10-24 10:33_

---

_Label `bug` added by @ntBre on 2025-10-24 13:12_

---

_Label `rule` added by @ntBre on 2025-10-24 13:12_

---

_Comment by @ntBre on 2025-10-24 13:26_

Thanks @AlexWaygood, this looks great! And you did all the hard work, I basically just copied your docstring into the rule docs :smile: 

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/non_self_return_type.rs`:61 on 2025-10-24 13:32_

```suggestion
/// The rule attempts to avoid flagging methods on metaclasses, since
/// [PEP 673] specifies that `Self` is disallowed in metaclasses. Ruff can
/// detect a class as being a metaclass if it inherits from a stdlib
/// metaclass such as `builtins.type` or `abc.ABCMeta`, and additionally
/// infers that a class may be a metaclass if it has a `__new__` method
/// with a similar signature to `type.__new__`. The heuristic used to
/// identify a metaclass-like `__new__` method signature is that it:
///
/// 1. Has exactly 5 parameters (including `cls`)
/// 1. Has a second parameter annotated with `str`
/// 1. Has a third parameter annotated with a `tuple` type
/// 1. Has a fourth parameter annotated with a `dict` type
/// 1. Has a fifth parameter is keyword-variadic (`**kwargs`)
```

---

_@AlexWaygood reviewed on 2025-10-24 13:33_

---

_Comment by @AlexWaygood on 2025-10-24 13:33_

> Thanks @AlexWaygood, this looks great! And you did all the hard work, I basically just copied your docstring into the rule docs 😄

Ah I have to share credit with @danparizher for that docstring, he wrote half of it!!

---

_Merged by @ntBre on 2025-10-24 13:40_

---

_Closed by @ntBre on 2025-10-24 13:40_

---

_Branch deleted on 2025-10-24 13:46_

---
