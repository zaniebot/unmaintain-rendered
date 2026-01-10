```yaml
number: 14267
title: "[red-knot] Shorten the paths for some mdtest files"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/shorten-mdtest-paths
created_at: 2024-11-11T11:15:52Z
updated_at: 2024-11-11T11:34:36Z
url: https://github.com/astral-sh/ruff/pull/14267
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Shorten the paths for some mdtest files

---

_Pull request opened by @AlexWaygood on 2024-11-11 11:15_

## Summary

The feedback in #14213 was that it was overall an improvement, but that some lines in mdtest's output were now very long due to how long some of the Markdown-file paths were relative to the workspace root. A simple way of mitigating this is to use shorter filenames for the Markdown files where it makes sense to do so.

## Test Plan

- `cargo test -p red_knot_python_semantic`
- `pre-commit run -a`


---

_Label `red-knot` added by @AlexWaygood on 2024-11-11 11:15_

---

_Review requested from @carljm by @AlexWaygood on 2024-11-11 11:15_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-11 11:15_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-11-11 11:15_

---

_Comment by @github-actions[bot] on 2024-11-11 11:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @AlexWaygood on 2024-11-11 11:34_

---

_Closed by @AlexWaygood on 2024-11-11 11:34_

---

_Branch deleted on 2024-11-11 11:34_

---
