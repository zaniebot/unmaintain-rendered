```yaml
number: 20801
title: "[`ruff`] Use DiagnosticTag for more pyflakes and panda rules"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: add-DiagnosticTag-to-pyflakes-rules
created_at: 2025-10-10T10:41:35Z
updated_at: 2025-10-19T09:07:17Z
url: https://github.com/astral-sh/ruff/pull/20801
synced_at: 2026-01-10T17:34:34Z
```

# [`ruff`] Use DiagnosticTag for more pyflakes and panda rules

---

_Pull request opened by @TaKO8Ki on 2025-10-10 10:41_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes a part of #20590 and follow up to #20734

Rolling out DiagnosticTag to every existing rule in one go would complicate the review, so in this pull request I’ve limited the changes to flake8 and numpy. (RUF100, PD011, F811, F842, RUF029)

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-10-10 10:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-10-12 06:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pandas_vet/rules/subscript.rs`:168 on 2025-10-12 06:17_

Hmm. I'm not sure about using deprecated here. The method isn't deprecated according to [pandas documentation](https://docs.astral.sh/ruff/rules/pandas-use-of-dot-values/); they only advise against it. To me, that makes this rule similar to other rules that recommend one pattern over another because it's less error prone

---

_@MichaReiser reviewed on 2025-10-12 06:22_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unused_async.rs`:198 on 2025-10-12 06:22_

I think using the unnecessary tag for this rule is problematic because it makes the function appear as unused when this is inaccurate (it's not the async keyword that's rendered as unused).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/unused_annotation.rs`:49 on 2025-10-12 06:23_

Is my understanding correct that `range` is the range for the entire binding? If so, I think we shoulnd't use the unnecessary tag because it makes the whole binding appear as unnecessary, when it's just the annotation that's unnecessary.

---

_@MichaReiser reviewed on 2025-10-12 06:23_

---

_Label `diagnostics` added by @MichaReiser on 2025-10-12 06:23_

---

_@TaKO8Ki reviewed on 2025-10-17 18:56_

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/pyflakes/rules/unused_annotation.rs`:49 on 2025-10-17 18:56_

You're right. I will remove the tag here.

---

_Review comment by @TaKO8Ki on `crates/ruff_linter/src/rules/pandas_vet/rules/subscript.rs`:168 on 2025-10-17 19:05_

This PandasUseOfDotIx rule is other one PD007 https://docs.astral.sh/ruff/rules/pandas-use-of-dot-ix. It says:

> The .ix method is deprecated as its behavior is ambiguous.

So it seems that the method is deprecated?

---

_@TaKO8Ki reviewed on 2025-10-17 19:05_

---

_Review requested from @MichaReiser by @TaKO8Ki on 2025-10-17 19:05_

---

_Comment by @TaKO8Ki on 2025-10-17 19:05_

@MichaReiser Thank you for the review. I have addressed your comments.

---

_Comment by @MichaReiser on 2025-10-19 09:07_

Thanks

---

_Merged by @MichaReiser on 2025-10-19 09:07_

---

_Closed by @MichaReiser on 2025-10-19 09:07_

---
