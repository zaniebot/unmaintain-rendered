```yaml
number: 10647
title: "[`refurb`] Implement `unnecessary-from-float` (`FURB164`)"
type: pull_request
state: merged
author: hikaru-kajita
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: verbose-decimal-fraction-construction
created_at: 2024-03-28T14:07:02Z
updated_at: 2024-03-30T11:04:02Z
url: https://github.com/astral-sh/ruff/pull/10647
synced_at: 2026-01-10T22:47:02Z
```

# [`refurb`] Implement `unnecessary-from-float` (`FURB164`)

---

_Pull request opened by @hikaru-kajita on 2024-03-28 14:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implement FURB164 in the issue #1348.
Relevant Refurb docs is here: https://github.com/dosisod/refurb/blob/v2.0.0/docs/checks.md#furb164-no-from-float

I've changed the name from `no-from-float` to `verbose-decimal-fraction-construction`.

## Test Plan

<!-- How was it tested? -->

I've written it in the `FURB164.py`.

---

_Comment by @charliermarsh on 2024-03-28 14:23_

Maybe `verbose_fraction_construction`?

---

_Label `rule` added by @charliermarsh on 2024-03-28 14:24_

---

_Label `preview` added by @charliermarsh on 2024-03-28 14:24_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-28 14:24_

---

_Comment by @github-actions[bot] on 2024-03-28 14:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @hikaru-kajita on 2024-03-28 14:27_

I have thought of that, but should we incorporate `Decimal` as it is checks for both constructions of `Decimal` and `Fraction`?

---

_Comment by @hikaru-kajita on 2024-03-28 14:29_

Also I'm not very sure why `cargo test` for wasm is failing... Let me check on that.

---

_Comment by @MichaReiser on 2024-03-28 15:18_

It sometimes has this error. Let me re-run it to see if it resolves the issue.

---

_@charliermarsh reviewed on 2024-03-30 01:23_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_fraction_construction.rs`:36 on 2024-03-30 01:23_

Should the inner `"4.2"` be `4.2`?

---

_Renamed from "[`refurb`] Implement `verbose-decimal-fraction-construction` (`FURB164`)" to "[`refurb`] Implement `unnecessary-from-float` (`FURB164`)" by @charliermarsh on 2024-03-30 02:14_

---

_@charliermarsh approved on 2024-03-30 02:14_

Thanks!

---

_Comment by @hikaru-kajita on 2024-03-30 10:52_

Sorry for the late response and thanks for the review!

---

_Merged by @charliermarsh on 2024-03-30 11:04_

---

_Closed by @charliermarsh on 2024-03-30 11:04_

---
