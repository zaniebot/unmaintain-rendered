```yaml
number: 19051
title: "[`flake8-bugbear`] Make `B911` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-6
created_at: 2025-06-30T18:35:27Z
updated_at: 2025-06-30T20:57:21Z
url: https://github.com/astral-sh/ruff/pull/19051
synced_at: 2026-01-10T18:39:09Z
```

# [`flake8-bugbear`] Make `B911` example error out-of-the-box

---

_Pull request opened by @MeGaGiGaGon on 2025-06-30 18:35_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #18972

This PR makes [batched-without-explicit-strict (B911)](https://docs.astral.sh/ruff/rules/batched-without-explicit-strict/#batched-without-explicit-strict-b911)'s example error out-of-the-box

[Old example](https://play.ruff.rs/a897d96b-0749-4291-8a62-dfd4caf290a0)
```py
itertools.batched(iterable, n)
```

[New example](https://play.ruff.rs/1c1e0ab7-014c-4dc2-abed-c2cb6cd01f70)
```py
import itertools

itertools.batched(iterable, n)
```

Imports were also added to the "use instead" sections

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-30 18:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-30 20:47_

Thank you!

---

_Merged by @dylwil3 on 2025-06-30 20:48_

---

_Closed by @dylwil3 on 2025-06-30 20:48_

---

_Label `documentation` added by @dylwil3 on 2025-06-30 20:48_

---

_Branch deleted on 2025-06-30 20:57_

---
