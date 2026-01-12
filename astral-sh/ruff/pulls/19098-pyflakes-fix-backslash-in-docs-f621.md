```yaml
number: 19098
title: "[`pyflakes`] Fix backslash in docs (`F621`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-5
created_at: 2025-07-02T18:36:28Z
updated_at: 2025-07-02T19:02:18Z
url: https://github.com/astral-sh/ruff/pull/19098
synced_at: 2026-01-12T15:56:31Z
```

# [`pyflakes`] Fix backslash in docs (`F621`)

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

This fixes the docs for [expressions-in-star-assignment (F621)](https://docs.astral.sh/ruff/rules/expressions-in-star-assignment/#expressions-in-star-assignment-f621) having a backslash `\` before the left shifts `<<`. I'm not sure why this happened in the first place, as the docstring looks fine, but putting the `<<` inside a code block fixes it. I was not able to track down the source of the issue either. The only other rule with a `<<` is [missing-whitespace-around-bitwise-or-shift-operator (E227)](https://docs.astral.sh/ruff/rules/missing-whitespace-around-bitwise-or-shift-operator/#missing-whitespace-around-bitwise-or-shift-operator-e227), which already has it in a code block.

Old docs page:
![image](https://github.com/user-attachments/assets/993106c6-5d83-4aed-836b-e252f5b64916)
> In Python 3, no more than 1 \\<< 8 assignments are allowed before a starred expression, and no more than 1 \\<< 24 expressions are allowed after a starred expression.

New docs page:
![image](https://github.com/user-attachments/assets/3b40b35f-f39e-49f1-8b2e-262dda4085b4)
> In Python 3, no more than `1 << 8` assignments are allowed before a starred expression, and no more than `1 << 24` expressions are allowed after a starred expression.

## Test Plan

<!-- How was it tested? -->

N/A, no tests/functionality affected.

---

_Comment by @github-actions[bot] on 2025-07-02 18:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-07-02 19:00_

Nice, thanks! I wonder if mkdocs is trying to escape an HTML tag or something.

---

_Label `documentation` added by @ntBre on 2025-07-02 19:00_

---

_Merged by @ntBre on 2025-07-02 19:00_

---

_Closed by @ntBre on 2025-07-02 19:00_

---

_Branch deleted on 2025-07-02 19:02_

---
