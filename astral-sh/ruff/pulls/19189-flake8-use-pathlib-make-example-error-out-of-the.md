```yaml
number: 19189
title: "[`flake8-use-pathlib`] Make example error out-of-the-box (`PTH210`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-07T20:28:23Z
updated_at: 2025-07-07T21:07:27Z
url: https://github.com/astral-sh/ruff/pull/19189
synced_at: 2026-01-12T15:56:33Z
```

# [`flake8-use-pathlib`] Make example error out-of-the-box (`PTH210`)

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

This PR makes [invalid-pathlib-with-suffix (PTH210)](https://docs.astral.sh/ruff/rules/invalid-pathlib-with-suffix/#invalid-pathlib-with-suffix-pth210)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/d45720cc-fd08-4443-820f-b3bc9756ac59)
```py
path.with_suffix("py")
```

[New example](https://play.ruff.rs/4103669e-19c5-464a-a3fb-6e7d190ce5fd)
```py
from pathlib import Path

path = Path()

path.with_suffix("py")
```

The "Use instead" section was also modified similarly.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-07 20:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-07-07 21:03_

---

_@ntBre approved on 2025-07-07 21:04_

Thanks!

---

_Merged by @ntBre on 2025-07-07 21:04_

---

_Closed by @ntBre on 2025-07-07 21:04_

---

_Branch deleted on 2025-07-07 21:07_

---
