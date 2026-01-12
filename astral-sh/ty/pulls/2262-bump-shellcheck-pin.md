```yaml
number: 2262
title: Bump shellcheck pin
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: alex/bump-shellcheck
created_at: 2025-12-29T18:39:19Z
updated_at: 2025-12-29T18:41:50Z
url: https://github.com/astral-sh/ty/pull/2262
synced_at: 2026-01-12T15:54:28Z
```

# Bump shellcheck pin

---

_@AlexWaygood_

## Summary

This dependency isn't updated by Renovate because Renovate can only update `additional_dependencies` in `.pre-commit-config.yaml` if they're additional Python dependencies for a Python pre-commit hook, or additional node dependencies for a node pre-commit hook. In this case, it's an additional Go dependency for a Go pre-commit hook (x-ref https://github.com/renovatebot/renovate/pull/39469).

## Test Plan

`uvx prek run -a --hook-stage=manual`

---

_Label `internal` added by @AlexWaygood on 2025-12-29 18:39_

---

_Merged by @AlexWaygood on 2025-12-29 18:41_

---

_Closed by @AlexWaygood on 2025-12-29 18:41_

---

_Branch deleted on 2025-12-29 18:41_

---
