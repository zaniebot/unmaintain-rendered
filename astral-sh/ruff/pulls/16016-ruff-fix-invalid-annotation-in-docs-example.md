```yaml
number: 16016
title: "[`ruff`] Fix invalid annotation in docs example"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
assignees: []
merged: true
base: main
head: alex/invalid-docs-annotation
created_at: 2025-02-07T10:42:10Z
updated_at: 2025-02-07T10:48:39Z
url: https://github.com/astral-sh/ruff/pull/16016
synced_at: 2026-01-10T19:57:22Z
```

# [`ruff`] Fix invalid annotation in docs example

---

_Pull request opened by @AlexWaygood on 2025-02-07 10:42_

## Summary

Unfortunately the edits made by https://github.com/astral-sh/ruff/pull/15982 mean that the annotations in the second "Use instead" example are now incorrect, and would be rejected by a type checker. This is because `instance_dict` is declared as a `ClassVar` variable in the class body, which declares to the type checker that it is illegal to set the attribute on instances of the class (it can only be set on the class object itself). The attribute is then assigned to `self` in the `__init__` instance method, violating the annotation in the class body.

This PR fixes the incorrect annotation and adds a couple more examples.

## Test Plan

N/A

---

_Label `documentation` added by @AlexWaygood on 2025-02-07 10:42_

---

_Merged by @AlexWaygood on 2025-02-07 10:45_

---

_Closed by @AlexWaygood on 2025-02-07 10:45_

---

_Branch deleted on 2025-02-07 10:45_

---

_Comment by @github-actions[bot] on 2025-02-07 10:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
