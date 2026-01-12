```yaml
number: 19462
title: "[ty] Implicit instance attributes declared `Final`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/final-implicit-instance-attributes
created_at: 2025-07-21T13:31:03Z
updated_at: 2025-07-21T18:01:09Z
url: https://github.com/astral-sh/ruff/pull/19462
synced_at: 2026-01-12T15:56:40Z
```

# [ty] Implicit instance attributes declared `Final`

---

_@sharkdp_

## Summary

Adds proper type inference for implicit instance attributes that are declared with a "bare" `Final` and adds `invalid-assignment` diagnostics for all implicit instance attributes that are declared `Final` or `Final[…]`.

## Test Plan

New and updated MD tests.

## Ecosystem analysis

```diff
pytest (https://github.com/pytest-dev/pytest)
+ error[invalid-return-type] src/_pytest/fixtures.py:1662:24: Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~((...) -> object) & ~str) | (((str, Config, /) -> Unknown) & ~((...) -> object) & ~str) | (Unknown & ~str)
```

The definition of the `scope` attribute is [here](
https://github.com/pytest-dev/pytest/blob/5f993856350d1cff144e48810b1c0137c8ec45c5/src/_pytest/fixtures.py#L1020-L1028). Looks like this is a new false positive due to missing `TypeAlias` support that is surfaced here because we now infer a more precise type for `FixtureDef._scope`.

---

_Label `ty` added by @sharkdp on 2025-07-21 13:31_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-21 13:33_

---

_Comment by @github-actions[bot] on 2025-07-21 13:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
+ error[invalid-return-type] src/_pytest/fixtures.py:1662:24: Return type does not match returned value: expected `Scope`, found `Scope | (Unknown & ~None & ~((...) -> object) & ~str) | (((str, Config, /) -> Unknown) & ~((...) -> object) & ~str) | (Unknown & ~str)`
- Found 512 diagnostics
+ Found 513 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-21 13:41_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 1 | 0 | 0 |
| **Total** | **1** | **0** | **0** |

**[Full report with detailed diff](https://david-final-implicit-instanc.ecosystem-663.pages.dev/diff)**


---

_Marked ready for review by @sharkdp on 2025-07-21 13:47_

---

_Review requested from @carljm by @sharkdp on 2025-07-21 13:47_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-21 13:47_

---

_Review requested from @dcreager by @sharkdp on 2025-07-21 13:47_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:95 on 2025-07-21 13:52_

interesting -- pyright and pyrefly are both fine with this, but mypy objects:
- https://mypy-play.net/?mypy=latest&python=3.12&gist=3e5bb64897404a64402680cf40c3349a
- https://pyright-play.net/?pythonVersion=3.13&strict=true&enableExperimentalFeatures=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQ5AxqcQM51QCCAXOVB1ACYCmwUAfQGokMIQAo6PUsACUUALQA%2BKADkwKHm046oUmQDoAHi0IkKuzvuDGoAXigBGckA
- https://pyrefly.org/sandbox/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoBiqAhgDYBQ5AxqcQM51QCCAXOVB1ACYCmwUAfQGokMIQAo6PUsACUUALQA+KADkwKHm046oUmQDoAHi0IkKuzvuDGoAXigBGckA

I can sort-of see arguments both ways here? Is the spec explicit on this point?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1424 on 2025-07-21 13:56_

I suspect this is what's causing the increase in memory usage. Possibly you could consider only adding it as a standalone expression if you're inside a function scope inside a class scope, and use `infer_maybe_standalone_expression` in `infer.rs`?

---

_@AlexWaygood approved on 2025-07-21 13:56_

---

_@sharkdp reviewed on 2025-07-21 14:44_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:95 on 2025-07-21 14:44_

> I can sort-of see arguments both ways here? Is the spec explicit on this point?

Haven't seen anything. I guess it doesn't hurt to support this? And it might make sense if you try to avoid a `typing` import at runtime:
```py
if TYPE_CHECKING:
    from typing import Final

class C:
    def __init__(self):
        if TYPE_CHECKING:
            self.x: Final
        self.x = 1
```

---

_@AlexWaygood reviewed on 2025-07-21 15:51_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/final.md`:95 on 2025-07-21 15:51_

yeah that makes sense. I guess it might be nice to note in the test that existing type checkers have diverging behaviour here? But the behaviour you have definitely seems fine.

---

_@sharkdp reviewed on 2025-07-21 17:49_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1424 on 2025-07-21 17:49_

>  No memory usage changes detected ✅ 

Yes, that's it. Thanks for the suggestion!

---

_@AlexWaygood reviewed on 2025-07-21 17:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1424 on 2025-07-21 17:50_

fantastic, glad it worked! 😃

---

_Merged by @sharkdp on 2025-07-21 18:01_

---

_Closed by @sharkdp on 2025-07-21 18:01_

---

_Branch deleted on 2025-07-21 18:01_

---
