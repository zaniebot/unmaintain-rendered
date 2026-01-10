```yaml
number: 12583
title: Consider more stdlib decorators to be property-like
type: pull_request
state: merged
author: AlexWaygood
labels:
  - linter
assignees: []
merged: true
base: main
head: alex/is-property-3
created_at: 2024-07-30T15:40:05Z
updated_at: 2024-07-30T17:27:45Z
url: https://github.com/astral-sh/ruff/pull/12583
synced_at: 2026-01-10T21:47:02Z
```

# Consider more stdlib decorators to be property-like

---

_Pull request opened by @AlexWaygood on 2024-07-30 15:40_

## Summary

This PR is stacked on top of #12581.

We currently consider `@builtins.property` and `@functools.cached_property` to be "property-like" decorators, but there are several other similar stdlib decorators that it makes sense to consider when it comes to whether a decorated function has property-like semantics: `enum.property`, `abc.abstractproperty` and `types.DynamicClassAttribute`. This PR updates `ruff_python_semantic::analyze::visibility::is_property` to consider those as well.

## Test Plan

Added some fixtures. `cargo test`.

---

_Label `linter` added by @AlexWaygood on 2024-07-30 15:40_

---

_Comment by @github-actions[bot] on 2024-07-30 15:53_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-07-30 16:24_

---

_Comment by @charliermarsh on 2024-07-30 16:24_

Nice.

---

_Merged by @AlexWaygood on 2024-07-30 17:18_

---

_Closed by @AlexWaygood on 2024-07-30 17:18_

---

_Branch deleted on 2024-07-30 17:18_

---
