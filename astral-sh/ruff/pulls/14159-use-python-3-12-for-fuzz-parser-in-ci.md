```yaml
number: 14159
title: "Use Python 3.12 for `fuzz-parser` in CI"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/fix-ci
created_at: 2024-11-07T15:23:49Z
updated_at: 2024-11-07T15:51:07Z
url: https://github.com/astral-sh/ruff/pull/14159
synced_at: 2026-01-10T20:50:57Z
```

# Use Python 3.12 for `fuzz-parser` in CI

---

_Pull request opened by @AlexWaygood on 2024-11-07 15:23_

## Summary

Looks like merging #14156 broke CI on the `main` branch. I thought I was safe to enable automerge as I'd done extensive testing locally -- more fool me!

It looks like the problem is something to do with f-strings, so I'm guessing [PEP-701](https://peps.python.org/pep-0701/) is to blame. Let's see if upgrading our Python versions in CI to 3.13 (which is what I used for local testing) fixes things.


---

_Label `ci` added by @AlexWaygood on 2024-11-07 15:23_

---

_Marked ready for review by @AlexWaygood on 2024-11-07 15:26_

---

_@MichaReiser approved on 2024-11-07 15:27_

---

_Comment by @AlexWaygood on 2024-11-07 15:29_

Python 3.13 breaks `ruff-lsp`. Let's try 3.12!

---

_Renamed from "Use Python 3.13 for `fuzz-parser` in CI" to "Use Python 3.12 for `fuzz-parser` in CI" by @AlexWaygood on 2024-11-07 15:33_

---

_Comment by @github-actions[bot] on 2024-11-07 15:50_

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

_Merged by @AlexWaygood on 2024-11-07 15:51_

---

_Closed by @AlexWaygood on 2024-11-07 15:51_

---

_Branch deleted on 2024-11-07 15:51_

---
