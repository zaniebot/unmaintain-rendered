```yaml
number: 18635
title: "`SourceOrderVisitor` should visit the `Identifier` part of the `PatternKeyword` node"
type: pull_request
state: merged
author: grievejia
labels:
  - internal
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-11T20:31:15Z
updated_at: 2025-06-12T06:20:14Z
url: https://github.com/astral-sh/ruff/pull/18635
synced_at: 2026-01-12T15:56:23Z
```

# `SourceOrderVisitor` should visit the `Identifier` part of the `PatternKeyword` node

---

_@grievejia_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

I've noticed that when using a custom `SourceOrderVisitor` to visit a `PatternKeyword` node, enter/exit is not invoked on `Identifier` part of that node. E.g.
```python
match x:
  case Foo(y=1): ...
#          ^ This is not visited
```
This PR proposes a fix for it. 

## Test Plan

`cargo test`


---

_Marked ready for review by @grievejia on 2025-06-11 20:33_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-11 20:49_

---

_Comment by @github-actions[bot] on 2025-06-11 20:54_

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

_Label `internal` added by @MichaReiser on 2025-06-12 06:20_

---

_Merged by @MichaReiser on 2025-06-12 06:20_

---

_Closed by @MichaReiser on 2025-06-12 06:20_

---
