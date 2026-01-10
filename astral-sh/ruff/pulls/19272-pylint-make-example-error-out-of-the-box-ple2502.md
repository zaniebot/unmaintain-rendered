```yaml
number: 19272
title: "[`pylint`] Make example error out-of-the-box (`PLE2502`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-10T23:56:35Z
updated_at: 2025-07-14T19:07:04Z
url: https://github.com/astral-sh/ruff/pull/19272
synced_at: 2026-01-10T17:58:13Z
```

# [`pylint`] Make example error out-of-the-box (`PLE2502`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-10 23:56_

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
Fixes #14346

This PR makes [bidirectional-unicode (PLE2502)](https://docs.astral.sh/ruff/rules/bidirectional-unicode/#bidirectional-unicode-ple2502)'s example error out-of-the-box, by converting it to use one of the test cases. The documentation in general is also updated to replace "bidirectional unicode character" with "bidirectional formatting character", as those are the only ones checked for, and the "unicode" suffix is redundant. The new example section looks like this:
<img width="1074" height="264" alt="image" src="https://github.com/user-attachments/assets/cc1d2cb4-b590-4f20-a4d2-15b744872cdd" />

The "References" section link is also updated to reflect the rule's actual behavior.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Comment by @github-actions[bot] on 2025-07-11 00:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dscorbett on 2025-07-11 01:22_

The PEP 672 link in the “References” section should be updated to point to [Bidirectional Marks, Embeddings, Overrides and Isolates](https://peps.python.org/pep-0672/#bidirectional-marks-embeddings-overrides-and-isolates), where the new example originates, instead of [Bidirectional Text](https://peps.python.org/pep-0672/#bidirectional-text).

“unicode” should be written “Unicode” throughput this rule’s documentation, or deleted as redundant because all characters in Python are Unicode characters.

---

_Comment by @MeGaGiGaGon on 2025-07-11 01:38_

Sounds reasonable to me, the link was wrong in the first place since the lint doesn't check for all RTL chars.

---

_@dscorbett reviewed on 2025-07-11 12:21_

---

_Label `documentation` added by @ntBre on 2025-07-11 13:09_

---

_@MeGaGiGaGon reviewed on 2025-07-11 15:00_

---

_@ntBre approved on 2025-07-14 18:46_

Thank you!

---

_Merged by @ntBre on 2025-07-14 18:46_

---

_Closed by @ntBre on 2025-07-14 18:46_

---

_Branch deleted on 2025-07-14 19:07_

---
