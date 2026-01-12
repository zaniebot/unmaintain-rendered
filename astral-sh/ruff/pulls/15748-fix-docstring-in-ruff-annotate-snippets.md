```yaml
number: 15748
title: Fix docstring in ruff_annotate_snippets
type: pull_request
state: merged
author: naslundx
labels:
  - internal
assignees: []
merged: true
base: main
head: fix-docstring
created_at: 2025-01-26T16:08:20Z
updated_at: 2025-01-27T13:34:07Z
url: https://github.com/astral-sh/ruff/pull/15748
synced_at: 2026-01-12T15:55:52Z
```

# Fix docstring in ruff_annotate_snippets

---

_@naslundx_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Found a comment that looks to be intended as docstring but accidentally is just a normal comment.

Didn't create an issue as the readme said it's not neccessary for trivial changes.

## Test Plan

<!-- How was it tested? -->
Can be tested by regenerating the docs.


---

_Review requested from @BurntSushi by @naslundx on 2025-01-26 16:08_

---

_Comment by @github-actions[bot] on 2025-01-26 16:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @AlexWaygood on 2025-01-26 17:33_

---

_Merged by @charliermarsh on 2025-01-27 03:25_

---

_Closed by @charliermarsh on 2025-01-27 03:25_

---

_Comment by @BurntSushi on 2025-01-27 13:34_

Note that I specifically didn't make changes like this when I vendored [upstream `annotate-snippets`](https://github.com/rust-lang/annotate-snippets-rs/blob/32dc46465e7b76295a3dc29c3e89b3e3f9f936aa/src/renderer/mod.rs#L100-L101) in order to minimize the diff with upstream as much as possible. Otherwise, there is a lot I would have changed and fixed up in the vendored code. :-)

---
