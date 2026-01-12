```yaml
number: 21470
title: "[ty] Dataclasses: `__hash__` semantics and `unsafe_hash`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/dataclass-unsafe_hash
created_at: 2025-11-15T10:45:06Z
updated_at: 2025-11-16T09:52:31Z
url: https://github.com/astral-sh/ruff/pull/21470
synced_at: 2026-01-12T15:57:25Z
```

# [ty] Dataclasses: `__hash__` semantics and `unsafe_hash`

---

_@sharkdp_

## Summary

Implement the semantics of `__hash__` for dataclasses and add support for `unsafe_hash`

## Test Plan

New Markdown tests.

---

_Review requested from @carljm by @sharkdp on 2025-11-15 10:45_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-15 10:45_

---

_Review requested from @dcreager by @sharkdp on 2025-11-15 10:45_

---

_Label `ty` added by @sharkdp on 2025-11-15 10:45_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 10:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-15 10:48_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-15 10:49_

---

_Comment by @astral-sh-bot[bot] on 2025-11-15 10:54_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-dataclass-unsafe-hash.ecosystem-663.pages.dev/diff)** ([timing results](https://david-dataclass-unsafe-hash.ecosystem-663.pages.dev/timing))




---

_Comment by @sharkdp on 2025-11-15 11:23_

> ```diff
> + tests/test_dunders.py:1016:13: error[unresolved-attribute] Object of type `None` has no attribute `__code__`
> + tests/test_dunders.py:1028:13: error[unresolved-attribute] Object of type `None` has no attribute `__code__`
> + tests/test_dunders.py:1040:13: error[unresolved-attribute] Object of type `None` has no attribute `__code__`
> ```

~~Hmm, I think this might require https://github.com/astral-sh/ruff/pull/21457. Let's merge that first and then rerun the ecosystem check here.~~

No, I think it requires https://github.com/astral-sh/ruff/pull/21474

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-15 16:49_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-15 16:49_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-15 19:23_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-15 19:23_

---

_@AlexWaygood approved on 2025-11-15 20:06_

---

_Merged by @sharkdp on 2025-11-16 09:52_

---

_Closed by @sharkdp on 2025-11-16 09:52_

---

_Branch deleted on 2025-11-16 09:52_

---
