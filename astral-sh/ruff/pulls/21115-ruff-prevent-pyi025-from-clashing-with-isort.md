```yaml
number: 21115
title: "[`ruff`] Prevent PYI025 from clashing with isort-required check"
type: pull_request
state: closed
author: TaKO8Ki
labels:
  - bug
  - fixes
assignees: []
base: main
head: fix-piy025-isort-loop
created_at: 2025-10-28T22:11:37Z
updated_at: 2026-01-21T10:30:23Z
url: https://github.com/astral-sh/ruff/pull/21115
synced_at: 2026-01-21T10:59:59Z
```

# [`ruff`] Prevent PYI025 from clashing with isort-required check

---

_@TaKO8Ki_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #20891

## Test Plan

<!-- How was it tested? -->

Added `required_import_skips_pyi025`.


---

_Review requested from @AlexWaygood by @TaKO8Ki on 2025-10-28 22:11_

---

_Comment by @github-actions[bot] on 2025-10-28 22:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `fixes` added by @amyreese on 2025-10-28 23:53_

---

_Label `bug` added by @amyreese on 2025-10-28 23:53_

---

_Review requested from @ntBre by @amyreese on 2025-10-28 23:53_

---

_@ntBre reviewed on 2025-10-29 18:59_

Thank you!

I confirmed locally that this fixes the issue, but this test doesn't reproduce the error from the issue when I revert the rule change. I think you need to include a flag like `--fix` or `--diff` in the `Command` to reproduce the infinite loop.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unaliased_collections_abc_set_import.rs`:90 on 2025-10-29 19:07_

I don't think this is the best fix. To me I think it would make more sense to reject this configuration entirely at an earlier stage. Fundamentally, it just doesn't make sense to tell Ruff that you simultaneously want `from collections.abc import Set` to always appear in every module, and also `from collections.abc import Set` to never appear in any module.

That implies that we'd want to edit this part of the code: https://github.com/astral-sh/ruff/blob/aca8ba76a489f2ec43958868254de2ce14c8e9d2/crates/ruff_workspace/src/configuration.rs#L1614-L1651

---

_@AlexWaygood reviewed on 2025-10-29 19:07_

---

_@ntBre reviewed on 2025-10-29 19:12_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/unaliased_collections_abc_set_import.rs`:90 on 2025-10-29 19:12_

I thought about that too. This seemed like a pretty niche issue, so I didn't feel too strongly, but this also makes sense to me.

---

_@AlexWaygood reviewed on 2025-10-29 19:15_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/unaliased_collections_abc_set_import.rs`:90 on 2025-10-29 19:15_

I generally have a pretty strong preference towards rejecting invalid configurations rather than silently accepting them and assuming we understand what the user _actually_ wanted 😄

---

_@ntBre reviewed on 2025-10-29 19:21_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/unaliased_collections_abc_set_import.rs`:90 on 2025-10-29 19:21_

We've done a bit of both with these conflicts, I think. https://github.com/astral-sh/ruff/issues/16802 is one example, but maybe that's different :slightly_smiling_face: 

---

_@TaKO8Ki reviewed on 2025-10-31 07:16_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/flake8_pyi/rules/unaliased_collections_abc_set_import.rs`:90 on 2025-10-31 07:16_

Ok. I will check it.

---

_Comment by @MichaReiser on 2026-01-21 10:30_

Thanks for working on this. I'll close this PR due to inactivity. Feel free to resubmit the changes with the feedback addressed.

---

_Closed by @MichaReiser on 2026-01-21 10:30_

---
