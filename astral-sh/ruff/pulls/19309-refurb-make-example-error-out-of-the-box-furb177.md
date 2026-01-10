```yaml
number: 19309
title: "[refurb] Make example error out-of-the-box (FURB177)"
type: pull_request
state: merged
author: close2code-palm
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/refurb_implicit_cwd
created_at: 2025-07-13T14:52:36Z
updated_at: 2025-07-14T16:23:02Z
url: https://github.com/astral-sh/ruff/pull/19309
synced_at: 2026-01-10T18:33:12Z
```

# [refurb] Make example error out-of-the-box (FURB177)

---

_Pull request opened by @close2code-palm on 2025-07-13 14:52_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Part of #18972
This PR makes [implicit-cwd(FURB177)](https://docs.astral.sh/ruff/rules/implicit-cwd/)'s example error out-of-the-box.

[Old example](https://play.ruff.rs/a0bef229-9626-426f-867f-55cb95ee64d8)
```python
cwd = Path().resolve()
```
[New example](https://play.ruff.rs/bdbea4af-e276-4603-a1b6-88757dfaa399)
```python
from pathlib import Path

cwd = Path().resolve()
```
<!-- What's the purpose of the change? What does it do, and why? -->


## Test Plan

<!-- How was it tested? -->
N/A, no functionality/tests affected

---

_Marked ready for review by @close2code-palm on 2025-07-13 14:53_

---

_Comment by @github-actions[bot] on 2025-07-13 15:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[refurb] Make FURB177 example error out-of-the-box" to "[refurb] Make example error out-of-the-box (FURB177)" by @close2code-palm on 2025-07-13 16:11_

---

_Label `documentation` added by @MichaReiser on 2025-07-14 07:14_

---

_@dylwil3 approved on 2025-07-14 16:22_

Thank you!

---

_Merged by @dylwil3 on 2025-07-14 16:23_

---

_Closed by @dylwil3 on 2025-07-14 16:23_

---
