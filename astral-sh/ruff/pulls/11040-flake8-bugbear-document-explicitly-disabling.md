```yaml
number: 11040
title: "[flake8-bugbear] Document explicitly disabling strict zip (`B905`)"
type: pull_request
state: merged
author: jfrost-mo
labels:
  - documentation
assignees: []
merged: true
base: main
head: B905_clarification
created_at: 2024-04-19T13:27:46Z
updated_at: 2024-04-19T13:55:25Z
url: https://github.com/astral-sh/ruff/pull/11040
synced_at: 2026-01-12T15:55:34Z
```

# [flake8-bugbear] Document explicitly disabling strict zip (`B905`)

---

_@jfrost-mo_

Occasionally you intentionally have iterables of differing lengths. The rule permits this by explicitly adding `strict=False`, but this was not documented.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The rule does not currently document how to avoid it when having differing length iterables is intentional. This PR adds that to the rule documentation.

## Test Plan

<!-- How was it tested? -->

Small documentation change, untested.

---

_Review comment by @VascoSch92 on `crates/ruff_linter/src/rules/flake8_bugbear/rules/zip_without_explicit_strict.rs`:20 on 2024-04-19 13:29_

```suggestion
/// non-uniform length. If the iterables are intentionally different lengths, the
```

---

_@VascoSch92 reviewed on 2024-04-19 13:30_

---

_@jfrost-mo reviewed on 2024-04-19 13:34_

---

_Review comment by @jfrost-mo on `crates/ruff_linter/src/rules/flake8_bugbear/rules/zip_without_explicit_strict.rs`:20 on 2024-04-19 13:34_

> If the iterables are intentionally different lengths the parameter should be explicitly set to False.

> If the iterables are intentionally different lengths, the parameter should be explicitly set to False.

I feel like there shouldn't be a comma there, but I'm not 100% sure.

---

_Review comment by @jfrost-mo on `crates/ruff_linter/src/rules/flake8_bugbear/rules/zip_without_explicit_strict.rs`:20 on 2024-04-19 13:36_

I guess it is a separate subclause, so comma it is.

---

_@jfrost-mo reviewed on 2024-04-19 13:36_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-19 13:41_

---

_Label `documentation` added by @charliermarsh on 2024-04-19 13:41_

---

_Comment by @charliermarsh on 2024-04-19 13:41_

Thanks!

---

_Merged by @charliermarsh on 2024-04-19 13:50_

---

_Closed by @charliermarsh on 2024-04-19 13:50_

---

_Comment by @github-actions[bot] on 2024-04-19 13:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2024-04-19 13:55_

---
