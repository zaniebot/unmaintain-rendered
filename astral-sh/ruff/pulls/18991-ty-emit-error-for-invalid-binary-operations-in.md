```yaml
number: 18991
title: "[ty] Emit error for invalid binary operations in type expressions"
type: pull_request
state: merged
author: med1844
labels:
  - ty
assignees: []
merged: true
base: main
head: 706
created_at: 2025-06-27T18:04:19Z
updated_at: 2025-07-03T16:04:12Z
url: https://github.com/astral-sh/ruff/pull/18991
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Emit error for invalid binary operations in type expressions

---

_@med1844_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This PR adds diagnostic for invalid binary operators in type expressions. It should close https://github.com/astral-sh/ty/issues/706 if merged.

Please feel free to suggest better wordings for the diagnostic message.

## Test Plan

<!-- How was it tested? -->

I modified `mdtest/annotations/invalid.md` and added a test for each binary operator, and fixed tests that was broken by the new diagnostic.

---

_Review requested from @carljm by @med1844 on 2025-06-27 18:04_

---

_Review requested from @AlexWaygood by @med1844 on 2025-06-27 18:04_

---

_Review requested from @sharkdp by @med1844 on 2025-06-27 18:04_

---

_Review requested from @dcreager by @med1844 on 2025-06-27 18:04_

---

_Comment by @github-actions[bot] on 2025-06-27 18:08_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[invalid-type-form] src/hydra_zen/structured_configs/_implementations.py:932:34: Invalid binary operator `*` in type annotation
- Found 597 diagnostics
+ Found 598 diagnostics

```
</details>


---

_Converted to draft by @med1844 on 2025-06-27 18:11_

---

_Marked ready for review by @med1844 on 2025-06-27 18:40_

---

_Comment by @sharkdp on 2025-06-30 07:58_

> hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen) + error[invalid-type-form] src/hydra_zen/structured_configs/_implementations.py:932:34: Invalid binary operator `*` in type annotation

Hm, this is interesting.

The line in question is here: https://github.com/mit-ll-responsible-ai/hydra-zen/blob/dd2b50a9614c6f8c46c5866f283c8f7e7a960aa8/src/hydra_zen/structured_configs/_implementations.py#L932

This looks like an unrelated error to me where we treat this as a type expression when we should be treating it as a non-type expression? A similar error can be triggered on main for something like `tuple[1]`. 

Reported here: https://github.com/astral-sh/ty/issues/727

---

_@sharkdp approved on 2025-06-30 08:04_

Thank you very much!

---

_Merged by @sharkdp on 2025-06-30 08:06_

---

_Closed by @sharkdp on 2025-06-30 08:06_

---

_Label `ty` added by @ntBre on 2025-07-03 16:04_

---
