```yaml
number: 19190
title: "[`pycodestyle`] Make example not raise unnecessary `SyntaxError` (`E114`)"
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-07-07T22:46:38Z
updated_at: 2025-07-08T14:34:29Z
url: https://github.com/astral-sh/ruff/pull/19190
synced_at: 2026-01-10T18:33:12Z
```

# [`pycodestyle`] Make example not raise unnecessary `SyntaxError` (`E114`)

---

_Pull request opened by @MeGaGiGaGon on 2025-07-07 22:46_

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

This PR makes [indentation-with-invalid-multiple-comment (E114)](https://docs.astral.sh/ruff/rules/indentation-with-invalid-multiple-comment/#indentation-with-invalid-multiple-comment-e114)'s example not raise a syntax error by adding a 4 space indented `...`. The example still gave `E114` without this, but adding the `...` both makes the change in indentation of the comment clearer, and makes it not give a `SyntaxError`.

## Test Plan

<!-- How was it tested? -->

N/A, no functionality/tests affected

---

_Renamed from "[`pycodestyle`] Make example error out-of-the-box (`E114`)" to "[`pycodestyle`] Make example not raise unnecessary `SyntaxError` (`E114`)" by @MeGaGiGaGon on 2025-07-07 22:47_

---

_Comment by @github-actions[bot] on 2025-07-07 23:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review requested from @ntBre by @ntBre on 2025-07-08 02:32_

---

_Label `documentation` added by @ntBre on 2025-07-08 02:32_

---

_@ntBre approved on 2025-07-08 14:00_

---

_Merged by @ntBre on 2025-07-08 14:00_

---

_Closed by @ntBre on 2025-07-08 14:00_

---

_Branch deleted on 2025-07-08 14:34_

---
