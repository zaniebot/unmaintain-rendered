```yaml
number: 19113
title: "[`flake8-simplify`] Make example error out-of-the-box (`SIM110`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-9
created_at: 2025-07-03T04:54:16Z
updated_at: 2025-07-03T14:17:06Z
url: https://github.com/astral-sh/ruff/pull/19113
synced_at: 2026-01-12T15:56:32Z
```

# [`flake8-simplify`] Make example error out-of-the-box (`SIM110`)

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

This PR makes [reimplemented-builtin (SIM110)](https://docs.astral.sh/ruff/rules/reimplemented-builtin/#reimplemented-builtin-sim110)'s example error out-of-the-box

[Old example](https://play.ruff.rs/1c192e8b-13f8-4f07-8c35-9dcd516a4a02)
```py
for item in iterable:
    if predicate(item):
        return True
return False
```

[New example](https://play.ruff.rs/f77393ad-20b1-436f-a872-d3bccec7c829)
```py
def foo():
    for item in iterable:
        if predicate(item):
            return True
    return False
```

The "Use instead" section was also updated to reflect the change.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-03 05:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-03 13:57_

Thanks!

---

_Label `documentation` added by @ntBre on 2025-07-03 13:57_

---

_Merged by @ntBre on 2025-07-03 13:57_

---

_Closed by @ntBre on 2025-07-03 13:57_

---

_Branch deleted on 2025-07-03 14:17_

---
