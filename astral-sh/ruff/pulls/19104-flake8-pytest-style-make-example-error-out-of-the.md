```yaml
number: 19104
title: "[`flake8-pytest-style`] Make example error out-of-the-box (`PT023`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-3
created_at: 2025-07-02T21:50:58Z
updated_at: 2025-07-03T15:53:57Z
url: https://github.com/astral-sh/ruff/pull/19104
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-pytest-style`] Make example error out-of-the-box (`PT023`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-02 21:50_

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

This PR makes [pytest-incorrect-mark-parentheses-style (PT023)](https://docs.astral.sh/ruff/rules/pytest-incorrect-mark-parentheses-style/#pytest-incorrect-mark-parentheses-style-pt023)'s example error out-of-the-box

[Old example](https://play.ruff.rs/48989153-6d4a-493a-a287-07f330f270bc)
```py
import pytest


@pytest.mark.foo
def test_something(): ...
```

[New example](https://play.ruff.rs/741f4d19-4607-4777-a77e-4ea6c62845e1)
```py
import pytest


@pytest.mark.foo()
def test_something(): ...
```

This just swaps the parenthesis in the "Example" and "Use instead" sections since the default configuration is no parenthesis

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-02 22:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-03 14:29_

---

_Label `documentation` added by @ntBre on 2025-07-03 14:29_

---

_Merged by @ntBre on 2025-07-03 14:29_

---

_Closed by @ntBre on 2025-07-03 14:29_

---

_Branch deleted on 2025-07-03 15:53_

---
