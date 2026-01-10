```yaml
number: 17955
title: "[ty] Make `unused-ignore-comment` disabled by default for now"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: alex/unused-ignore
created_at: 2025-05-08T16:07:37Z
updated_at: 2025-05-08T16:24:03Z
url: https://github.com/astral-sh/ruff/pull/17955
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Make `unused-ignore-comment` disabled by default for now

---

_Pull request opened by @AlexWaygood on 2025-05-08 16:07_

## Summary

Right now it seems like there are too many situations where:

1. There is a typing error on a line that all other type checkers detect
2. The user has suppressed that error for Reasons with a type: ignore comment
3. But we don't yet detect the typing error due to a missing feature, leading us to complain about the unused ignore comment

The rule feels like more of a lint anyway, and mypy's version is opt-in. I'd like for it to be enabled by default for the GA release, but right now it feels like we're getting a lot of user questions about it. Let's make it disabled-by-default until we're closer to feature-parity with other type checkers.

## Test Plan

- `cargo test`
- I ran ty locally on a file that just had `x = 1 + 2  # type: ignore` on it and verified that no diagnostics were reported by ty


---

_Review requested from @carljm by @AlexWaygood on 2025-05-08 16:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-08 16:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-08 16:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-08 16:07_

---

_Label `ty` added by @AlexWaygood on 2025-05-08 16:07_

---

_@MichaReiser approved on 2025-05-08 16:10_

Thanks. Would you mind opening an issue so that we don't forget to re-enable the rule.

---

_Label `configuration` added by @MichaReiser on 2025-05-08 16:10_

---

_Comment by @github-actions[bot] on 2025-05-08 16:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-08 16:16_

> Thanks. Would you mind opening an issue so that we don't forget to re-enable the rule.

Will do

---

_Merged by @AlexWaygood on 2025-05-08 16:21_

---

_Closed by @AlexWaygood on 2025-05-08 16:21_

---

_Branch deleted on 2025-05-08 16:21_

---

_Comment by @github-actions[bot] on 2025-05-08 16:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-05-08 16:24_

> > Thanks. Would you mind opening an issue so that we don't forget to re-enable the rule.
> 
> Will do

https://github.com/astral-sh/ty/issues/278

---
