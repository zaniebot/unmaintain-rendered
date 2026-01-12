```yaml
number: 10917
title: "Fix S310 `suspicious-url-open-usage` description"
type: pull_request
state: merged
author: hoel-bagard
labels:
  - documentation
assignees: []
merged: true
base: main
head: fix-S310-description-typo
created_at: 2024-04-13T10:12:30Z
updated_at: 2024-04-13T11:55:34Z
url: https://github.com/astral-sh/ruff/pull/10917
synced_at: 2026-01-12T15:55:33Z
```

# Fix S310 `suspicious-url-open-usage` description

---

_@hoel-bagard_

## Summary

The "What it does" section of the docstring is missing a verb, this PR adds it.

---

_Renamed from "Fixx S310 `suspicious-url-open-usage` description" to "Fix S310 `suspicious-url-open-usage` description" by @hoel-bagard on 2024-04-13 10:12_

---

_Marked ready for review by @hoel-bagard on 2024-04-13 10:12_

---

_Comment by @github-actions[bot] on 2024-04-13 10:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-04-13 10:40_

Thanks! Having "uses" and "use" in the same sentence reads a tiny bit oddly, but I'm not sure I have a great alternative. ("Pass"? "Employ"? Not sure)

---

_Comment by @hoel-bagard on 2024-04-13 10:58_

I had the same feeling about using twice the same word, but I don't have a great alternative either.

I thought about changing the first "uses" to "instances":
> Checks for instances of URL open functions that use unexpected schemes.


---

_Comment by @AlexWaygood on 2024-04-13 11:03_

Or maybe:

> Checks for instances where URL open functions are used with unexpected schemes.

?

---

_Merged by @AlexWaygood on 2024-04-13 11:52_

---

_Closed by @AlexWaygood on 2024-04-13 11:52_

---

_Label `documentation` added by @AlexWaygood on 2024-04-13 11:52_

---

_Comment by @AlexWaygood on 2024-04-13 11:52_

Thanks!

---

_Branch deleted on 2024-04-13 11:55_

---
