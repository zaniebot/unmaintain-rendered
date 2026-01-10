```yaml
number: 20968
title: Use uv more in CI workflows
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
assignees: []
merged: true
base: main
head: alex/more-uv
created_at: 2025-10-19T13:11:32Z
updated_at: 2025-10-20T21:06:50Z
url: https://github.com/astral-sh/ruff/pull/20968
synced_at: 2026-01-10T17:34:34Z
```

# Use uv more in CI workflows

---

_Pull request opened by @AlexWaygood on 2025-10-19 13:11_

## Summary

More dogfooding of our own tools.

I didn't touch the build-binaries workflow (it's scary) or the publish-docs workflow (which doesn't run on PRs) or the ruff-lsp job in the ci.yaml workflow (ruff-lsp is deprecated; it doesn't seem worth making changes there).

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-19 13:11_

---

_Comment by @github-actions[bot] on 2025-10-19 13:28_

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

_Merged by @AlexWaygood on 2025-10-19 13:59_

---

_Closed by @AlexWaygood on 2025-10-19 13:59_

---

_Branch deleted on 2025-10-20 21:06_

---
