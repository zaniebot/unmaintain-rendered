```yaml
number: 14915
title: "[red-knot] Remove an unnecessary branch and a confusing TODO comment"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/unnecessary-redknot-branch
created_at: 2024-12-11T13:20:14Z
updated_at: 2024-12-11T16:57:42Z
url: https://github.com/astral-sh/ruff/pull/14915
synced_at: 2026-01-10T20:42:27Z
```

# [red-knot] Remove an unnecessary branch and a confusing TODO comment

---

_Pull request opened by @AlexWaygood on 2024-12-11 13:20_

## Summary

This branch seems no longer to be necessary; our tests all pass without it. It makes sense that it wouldn't be needed, because member access on an `Any`/`Unknown`/`Todo` type should produce `Any`/`Unknown`/`Todo`, and whether or not a type is iterable entirely depends on member access under the hood (we test whether it has an `__iter__` method, and whether the type returned by that `__iter__` method (for the "`__iter__` method of an `Any`/`Todo`/`Unknown` type", this will also be `Any`/`Unknown`/`Todo`) has a `__next__` method.

I don't fully understand the TODO comment here, but it is also true that we've now done the thing that the TODO comment says we needed to do.

---

_Label `red-knot` added by @AlexWaygood on 2024-12-11 13:20_

---

_Review requested from @carljm by @AlexWaygood on 2024-12-11 13:20_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-12-11 13:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-12-11 13:20_

---

_Comment by @github-actions[bot] on 2024-12-11 13:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2024-12-11 16:55_

---

_Merged by @AlexWaygood on 2024-12-11 16:57_

---

_Closed by @AlexWaygood on 2024-12-11 16:57_

---

_Branch deleted on 2024-12-11 16:57_

---
