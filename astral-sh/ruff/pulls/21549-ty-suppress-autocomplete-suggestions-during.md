```yaml
number: 21549
title: "[ty] suppress autocomplete suggestions during variable binding"
type: pull_request
state: merged
author: RasmusNygren
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: suppress-suggestions-when-naming
created_at: 2025-11-20T21:50:57Z
updated_at: 2026-01-15T18:26:55Z
url: https://github.com/astral-sh/ruff/pull/21549
synced_at: 2026-01-15T18:49:33Z
```

# [ty] suppress autocomplete suggestions during variable binding

---

_@RasmusNygren_

Statements such as `def foo(p<CURSOR>`,
`def foo[T<CURSOR>` and `for foo<CURSOR>`
should not generate any suggestions as these
cases are introducing new names.

If it's not possible to determine that suggestions should be omitted using token matching in an easy way, we turn
to traversing the AST to determine the context.

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes https://github.com/astral-sh/ty/issues/1563

It keeps using the existing token matching pattern for the easy cases (nothing typed and most recent token is a definition token) and fallbacks to AST traveral for the slightly more difficult cases where token matching becomes difficult and error prone.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
New test cases and sanity-checking in the ty playground
<!-- How was it tested? -->


---

_Review requested from @carljm by @RasmusNygren on 2025-11-20 21:50_

---

_Review requested from @MichaReiser by @RasmusNygren on 2025-11-20 21:50_

---

_Review requested from @AlexWaygood by @RasmusNygren on 2025-11-20 21:50_

---

_Review requested from @sharkdp by @RasmusNygren on 2025-11-20 21:50_

---

_Review requested from @dcreager by @RasmusNygren on 2025-11-20 21:50_

---

_Review request for @dcreager removed by @MichaReiser on 2025-11-20 21:51_

---

_Review request for @carljm removed by @MichaReiser on 2025-11-20 21:51_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-11-20 21:51_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-11-20 21:51_

---

_Review requested from @BurntSushi by @MichaReiser on 2025-11-20 21:51_

---

_Label `server` added by @AlexWaygood on 2025-11-20 21:53_

---

_Label `ty` added by @AlexWaygood on 2025-11-20 21:53_

---

_Renamed from "[ty] suppress suggestions during variable binding" to "[ty] suppress autocomplete suggestions during variable binding" by @AlexWaygood on 2025-11-20 21:54_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 21:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-20 21:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@BurntSushi approved on 2025-11-21 17:06_

Nice, this makes sense to me!

---

_Merged by @BurntSushi on 2025-11-21 17:06_

---

_Closed by @BurntSushi on 2025-11-21 17:06_

---

_Comment by @BurntSushi on 2025-11-21 17:06_

Thank you!!!

---

_Branch deleted on 2026-01-15 18:26_

---
