```yaml
number: 19054
title: "[`flake8-bugbear`] Make `B028` example error out-of-the-box"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-06-30T20:08:04Z
updated_at: 2025-06-30T20:57:26Z
url: https://github.com/astral-sh/ruff/pull/19054
synced_at: 2026-01-12T15:56:30Z
```

# [`flake8-bugbear`] Make `B028` example error out-of-the-box

---

_@MeGaGiGaGon_

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

This PR makes [no-explicit-stacklevel (B028)](https://docs.astral.sh/ruff/rules/no-explicit-stacklevel/#no-explicit-stacklevel-b028)'s example error out-of-the-box

[Old example](https://play.ruff.rs/1ee80aec-2d6e-4a3f-8e98-da82b6a9f544)
```py
warnings.warn("This is a warning")
```

[New example](https://play.ruff.rs/343593aa-38a0-4d76-a32b-5abd0a4306cc)
```py
import warnings

warnings.warn("This is a warning")
```

Imports were also added to the "use instead" section

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-06-30 20:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-06-30 20:49_

Thank you!

---

_Merged by @dylwil3 on 2025-06-30 20:49_

---

_Closed by @dylwil3 on 2025-06-30 20:49_

---

_Label `documentation` added by @dylwil3 on 2025-06-30 20:54_

---

_Branch deleted on 2025-06-30 20:57_

---
