```yaml
number: 20946
title: Dogfood ty on py-fuzzer in CI
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: alex/dogfood-ty
created_at: 2025-10-17T18:56:48Z
updated_at: 2025-10-17T19:30:19Z
url: https://github.com/astral-sh/ruff/pull/20946
synced_at: 2026-01-10T17:34:34Z
```

# Dogfood ty on py-fuzzer in CI

---

_Pull request opened by @AlexWaygood on 2025-10-17 18:56_

## Summary

Ensure we feel the pain when we introduce new false positives or false negatives

## Test Plan

CI on this PR


---

_Label `ci` added by @AlexWaygood on 2025-10-17 18:56_

---

_Label `ty` added by @AlexWaygood on 2025-10-17 18:58_

---

_Marked ready for review by @AlexWaygood on 2025-10-17 19:05_

---

_Comment by @github-actions[bot] on 2025-10-17 19:13_

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

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:540 on 2025-10-17 19:25_

Is it worth adding mypy. Shouldn't we also feel the pain for false-negatives ;)

---

_@MichaReiser approved on 2025-10-17 19:25_

---

_Review comment by @sharkdp on `.github/workflows/ci.yaml`:281 on 2025-10-17 19:26_

Let's see how annoying it will be to add `# ty: ignore` comments...

---

_@sharkdp approved on 2025-10-17 19:26_

---

_@AlexWaygood reviewed on 2025-10-17 19:29_

---

_Review comment by @AlexWaygood on `.github/workflows/ci.yaml`:540 on 2025-10-17 19:29_

the mypy config is already there and I already run mypy locally when I make changes to the script 😄

---

_Merged by @AlexWaygood on 2025-10-17 19:30_

---

_Closed by @AlexWaygood on 2025-10-17 19:30_

---

_Branch deleted on 2025-10-17 19:30_

---
