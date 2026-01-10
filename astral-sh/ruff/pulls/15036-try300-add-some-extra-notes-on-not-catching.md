```yaml
number: 15036
title: "TRY300: Add some extra notes on not catching exceptions you didn't expect"
type: pull_request
state: merged
author: wlach
labels:
  - documentation
assignees: []
merged: true
base: main
head: try-consider-else-extra-note
created_at: 2024-12-17T13:31:41Z
updated_at: 2025-01-18T16:23:44Z
url: https://github.com/astral-sh/ruff/pull/15036
synced_at: 2026-01-10T20:05:43Z
```

# TRY300: Add some extra notes on not catching exceptions you didn't expect

---

_Pull request opened by @wlach on 2024-12-17 13:31_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Added some extra notes on why you should have focused try...except blocks to [TRY300](https://docs.astral.sh/ruff/rules/try-consider-else/).

When fixing a violation of this rule, a co-worker of mine (very understandably) asked why this was better. The current docs just say putting the return in the else is "more explicit", but if you look at the [linked reference in the python documentation](https://docs.python.org/3/tutorial/errors.html) they are more clear on why violations like this is bad:

> The use of the else clause is better than adding additional code to the [try](https://docs.python.org/3/reference/compound_stmts.html#try) clause because it avoids accidentally catching an exception that wasn’t raised by the code being protected by the try … except statement.

This is my attempt at adding more context to the docs on this. Open to suggestions for wording!

## Test Plan

N/A, just docs

---

_Comment by @github-actions[bot] on 2024-12-17 13:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-17 15:55_

---

_Label `documentation` added by @MichaReiser on 2024-12-17 15:55_

---

_@dylwil3 approved on 2025-01-18 16:17_

---

_Review request for @AlexWaygood removed by @dylwil3 on 2025-01-18 16:17_

---

_Comment by @dylwil3 on 2025-01-18 16:21_

Thanks for this! Tweaked the wording and added an additional justification.

---

_Merged by @dylwil3 on 2025-01-18 16:23_

---

_Closed by @dylwil3 on 2025-01-18 16:23_

---
