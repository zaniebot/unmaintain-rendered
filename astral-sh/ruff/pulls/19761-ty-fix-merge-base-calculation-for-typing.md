```yaml
number: 19761
title: "[ty] Fix merge base calculation for typing-conformance workflow"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/fix-typing_conformance-merge-base
created_at: 2025-08-05T12:07:59Z
updated_at: 2025-08-05T12:32:48Z
url: https://github.com/astral-sh/ruff/pull/19761
synced_at: 2026-01-12T15:56:46Z
```

# [ty] Fix merge base calculation for typing-conformance workflow

---

_@sharkdp_

## Summary

Use `$GITHUB_SHA` (the merged state of `feature` + `main` branch) instead of `{{ github.event.pull_request.head.sha }}` (just the latest `feature` commit) for building the "new" version of `ty` in the typing conformance workflow.

## Test Plan

None.


---

_Label `ci` added by @sharkdp on 2025-08-05 12:08_

---

_Label `ty` added by @sharkdp on 2025-08-05 12:08_

---

_Comment by @github-actions[bot] on 2025-08-05 12:09_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @sharkdp on 2025-08-05 12:09_

---

_Review comment by @AlexWaygood on `.github/workflows/typing_conformance.yaml`:68 on 2025-08-05 12:30_

we're deleting this line because we should already have the `$GITHUB_SHA` commit checked out?

---

_@AlexWaygood approved on 2025-08-05 12:31_

---

_@sharkdp reviewed on 2025-08-05 12:32_

---

_Review comment by @sharkdp on `.github/workflows/typing_conformance.yaml`:68 on 2025-08-05 12:32_

yes

---

_Merged by @sharkdp on 2025-08-05 12:32_

---

_Closed by @sharkdp on 2025-08-05 12:32_

---

_Branch deleted on 2025-08-05 12:32_

---
