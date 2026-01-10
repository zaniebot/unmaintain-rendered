```yaml
number: 21950
title: "[ty] Enable even more goto-definition on inlay hints"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: alex/more-inlay-goto
created_at: 2025-12-12T14:54:26Z
updated_at: 2025-12-12T17:57:40Z
url: https://github.com/astral-sh/ruff/pull/21950
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Enable even more goto-definition on inlay hints

---

_Pull request opened by @AlexWaygood on 2025-12-12 14:54_

## Summary

Working on py-fuzzer recently (AKA, a Python project!) reminded me how cool our "inlay hint goto-definition feature" is. So this PR adds a bunch more of that!

I also made a couple of other minor changes to type display. For example, in the playground, this snippet:

```py
def f(): ...
reveal_type(f.__get__)
```

currently leads to this diagnostic:

```
Revealed type: `<method-wrapper `__get__` of `f`>` (revealed-type) [Ln 2, Col 13]
```

But the fact that we have backticks both around the type display and inside the type display isn't _great_ there. This PR changes it to

```
Revealed type: `<method-wrapper '__get__' of function 'f'>` (revealed-type) [Ln 2, Col 13]
```

which avoids the nested-backticks issue in diagnostics, and is more similar to our display for various other `Type` variants such as class-literal types (`<class 'Foo'>`, etc., not ``<class `Foo`>``).

## Test Plan

inlay snapshots added; mdtests updated


---

_Review requested from @Gankra by @AlexWaygood on 2025-12-12 14:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-12-12 14:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-12 14:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-12 14:54_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-12-12 14:54_

---

_Label `server` added by @AlexWaygood on 2025-12-12 14:54_

---

_Label `ty` added by @AlexWaygood on 2025-12-12 14:54_

---

_Comment by @astral-sh-bot[bot] on 2025-12-12 14:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-12 15:01_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@Gankra approved on 2025-12-12 17:57_

Wow, incredible!

---

_Merged by @Gankra on 2025-12-12 17:57_

---

_Closed by @Gankra on 2025-12-12 17:57_

---

_Branch deleted on 2025-12-12 17:57_

---
