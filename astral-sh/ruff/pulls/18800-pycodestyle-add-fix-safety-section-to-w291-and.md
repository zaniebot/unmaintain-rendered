```yaml
number: 18800
title: "[`pycodestyle`] Add fix safety section to `W291` and `W293` "
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-safety-section-trailing_whitespace
created_at: 2025-06-19T19:27:03Z
updated_at: 2025-06-19T22:15:56Z
url: https://github.com/astral-sh/ruff/pull/18800
synced_at: 2026-01-10T18:39:09Z
```

# [`pycodestyle`] Add fix safety section to `W291` and `W293` 

---

_Pull request opened by @MeGaGiGaGon on 2025-06-19 19:27_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Part of #15584

This PR adds fix safety sections to `W291` and `W293`

The unsafe caveat was added in #10049

https://github.com/astral-sh/ruff/blob/10a1d9f01e899201c8d37d29071fe68752d09544/crates/ruff_linter/src/rules/pycodestyle/rules/trailing_whitespace.rs#L92

Code example demonstrating unsafety:
```
PS ~\Desktop\New_folder\ruff>Get-Content issue.py
```
```py
# W291
"""
1
"""

# W293
"""

"""
```
```
PS ~\Desktop\New_folder\ruff>Get-Escaped-Content issue.py
```
```
# W291\n"""\n1 \n"""\n\n# W293\n"""\n \n"""\r\n
```
```
PS ~\Desktop\New_folder\ruff>uvx ruff check issue.py --isolated --select W
```
```snap
issue.py:3:2: W291 Trailing whitespace
  |
1 | # W291
2 | """
3 | 1
  |  ^ W291
4 | """
  |
  = help: Remove trailing whitespace

issue.py:8:1: W293 Blank line contains whitespace
  |
6 | # W293
7 | """
8 |
  | ^ W293
9 | """
  |
  = help: Remove whitespace from blank line

Found 2 errors.
No fixes available (2 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

## Test Plan

<!-- How was it tested? -->

N/A, no tests affected.


---

_Renamed from "[pycodestyle] Add fix safety section to trailing_whitespace.rs" to "[`pycodestyle`] Add fix safety section to `W291` and `W293` " by @MeGaGiGaGon on 2025-06-19 19:49_

---

_Assigned to @dylwil3 by @MichaReiser on 2025-06-19 20:55_

---

_@dylwil3 approved on 2025-06-19 21:33_

Thank you!

---

_Label `documentation` added by @dylwil3 on 2025-06-19 21:33_

---

_Comment by @github-actions[bot] on 2025-06-19 21:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @dylwil3 on 2025-06-19 21:48_

---

_Closed by @dylwil3 on 2025-06-19 21:48_

---

_Branch deleted on 2025-06-19 22:15_

---
