```yaml
number: 19105
title: "[`flake8-pytest-style`] Make example error out-of-the-box (`PT030`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-4
created_at: 2025-07-02T22:06:54Z
updated_at: 2025-07-03T15:54:00Z
url: https://github.com/astral-sh/ruff/pull/19105
synced_at: 2026-01-10T18:33:12Z
```

# [`flake8-pytest-style`] Make example error out-of-the-box (`PT030`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-02 22:06_

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

This PR makes [pytest-warns-too-broad (PT030)](https://docs.astral.sh/ruff/rules/pytest-warns-too-broad/#pytest-warns-too-broad-pt030)'s example error out-of-the-box

[Old example](https://play.ruff.rs/2296ae7e-c775-427a-a020-6fb25321f3f7)
```py
import pytest


def test_foo():
    with pytest.warns(RuntimeWarning):
        ...

    # empty string is also an error
    with pytest.warns(RuntimeWarning, match=""):
        ...
```

[New example](https://play.ruff.rs/af35a482-1c2f-47ee-aff3-ff1e9fa447de)
```py
import pytest


def test_foo():
    with pytest.warns(Warning):
        ...

    # empty string is also an error
    with pytest.warns(Warning, match=""):
        ...
```

`RuntimeWarning` is not in the default [warns-require-match-for](https://docs.astral.sh/ruff/settings/#lint_flake8-pytest-style_warns-require-match-for) list, while `Warning` is. The "Use instead" section was also updated similarly

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-02 22:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @ntBre on 2025-07-03 14:26_

---

_@ntBre approved on 2025-07-03 14:27_

---

_Merged by @ntBre on 2025-07-03 14:27_

---

_Closed by @ntBre on 2025-07-03 14:27_

---

_Branch deleted on 2025-07-03 15:54_

---
