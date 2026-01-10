```yaml
number: 16761
title: "[red-knot] Extend ecosystem checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/extend-ecosystem-checks
created_at: 2025-03-14T20:35:37Z
updated_at: 2025-03-14T21:17:40Z
url: https://github.com/astral-sh/ruff/pull/16761
synced_at: 2026-01-10T19:49:02Z
```

# [red-knot] Extend ecosystem checks

---

_Pull request opened by @sharkdp on 2025-03-14 20:35_

## Summary

The ecosystem checks have proven useful so far, so I'm extending the list a bit. My main selection criteria are:

- Few dependencies (we don't understand -stubs/-types packages yet)
- Fewer than 1000 diagnostics
- No panics

## Test Plan

Ran it locally. We now have ~2k diagnostics in total, across 12 projects


---

_Label `red-knot` added by @sharkdp on 2025-03-14 20:35_

---

_Comment by @github-actions[bot] on 2025-03-14 20:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-03-14 20:47_

---

_Merged by @sharkdp on 2025-03-14 21:17_

---

_Closed by @sharkdp on 2025-03-14 21:17_

---

_Branch deleted on 2025-03-14 21:17_

---
