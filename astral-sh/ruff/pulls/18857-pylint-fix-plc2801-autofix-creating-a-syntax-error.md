```yaml
number: 18857
title: "[`pylint`] Fix `PLC2801` autofix creating a syntax error"
type: pull_request
state: merged
author: LaBatata101
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-PLC2801
created_at: 2025-06-21T21:22:51Z
updated_at: 2025-06-23T14:19:51Z
url: https://github.com/astral-sh/ruff/pull/18857
synced_at: 2026-01-12T15:56:26Z
```

# [`pylint`] Fix `PLC2801` autofix creating a syntax error

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary
This PR fixes `PLC2801` autofix creating a syntax error due to lack of padding if it is directly after a keyword.  

Fixes https://github.com/astral-sh/ruff/issues/18813
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Add regression test
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-21 21:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-06-23 14:15_

Very nice, thank you! I didn't know about `pad` either.

---

_Label `bug` added by @ntBre on 2025-06-23 14:15_

---

_Label `fixes` added by @ntBre on 2025-06-23 14:15_

---

_Merged by @ntBre on 2025-06-23 14:15_

---

_Closed by @ntBre on 2025-06-23 14:15_

---

_Branch deleted on 2025-06-23 14:19_

---
