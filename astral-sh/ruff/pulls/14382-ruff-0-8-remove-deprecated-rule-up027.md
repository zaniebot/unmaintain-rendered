```yaml
number: 14382
title: "[ruff 0.8] Remove deprecated rule UP027"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
  - breaking
assignees: []
merged: true
base: ruff-0.8
head: remove-up027
created_at: 2024-11-16T17:50:17Z
updated_at: 2024-11-18T11:40:41Z
url: https://github.com/astral-sh/ruff/pull/14382
synced_at: 2026-01-10T20:50:57Z
```

# [ruff 0.8] Remove deprecated rule UP027

---

_Pull request opened by @AlexWaygood on 2024-11-16 17:50_

## Summary

Remove UP027 in Ruff 0.8. UP027 has been deprecated since https://github.com/astral-sh/ruff/pull/12843, which landed as part of Ruff 0.6 (released on August 15th).

## Test Plan

- `cargo test -p ruff_linter` passes
- `git grep UP027`, `git grep UnpackedListComprehension` and `git grep unpacked_list_comprehension` all show no references to the rule remain outside of the changelog
- Wait for the ecosystem report to see how much impact this has


---

_Label `rule` added by @AlexWaygood on 2024-11-16 17:50_

---

_Label `breaking` added by @AlexWaygood on 2024-11-16 17:50_

---

_Marked ready for review by @AlexWaygood on 2024-11-16 18:00_

---

_@charliermarsh approved on 2024-11-16 18:00_

---

_Comment by @AlexWaygood on 2024-11-16 18:03_

> Wait for the ecosystem report to see how much impact this has

Seems like it's not possible to get an ecosystem report here, because of the fact that this PR targets the `ruff-0.8` branch, and will be the first commit on the `ruff-0.8` branch that deviates from `main`. Once this is merged into the `ruff-0.8` branch, I'll open a PR to merge that branch into `main` and take a look at the ecosystem report on that PR.

---

_Merged by @AlexWaygood on 2024-11-16 18:03_

---

_Closed by @AlexWaygood on 2024-11-16 18:03_

---

_Branch deleted on 2024-11-16 18:03_

---

_Added to milestone `v0.8` by @AlexWaygood on 2024-11-18 11:40_

---
