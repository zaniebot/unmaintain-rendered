```yaml
number: 19680
title: "[ty] Remove unused lint registry functionality?"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/remove-unused-lint-functions
created_at: 2025-08-01T07:50:35Z
updated_at: 2025-08-01T08:37:42Z
url: https://github.com/astral-sh/ruff/pull/19680
synced_at: 2026-01-12T15:56:45Z
```

# [ty] Remove unused lint registry functionality?

---

_@sharkdp_

## Summary

I'm assuming we want to keep this functionality for later, but it's currently not used(?).

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-08-01 07:50_

---

_Comment by @github-actions[bot] on 2025-08-01 07:52_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-01 07:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @MichaReiser on 2025-08-01 08:15_

I'd like to keep it. It was added in preparation for when we need to rename our first rule (that day will come!). 

There are other concepts like removed and deprecated rules that aren't used today as well, but will become relevant in the near future.

Planning ahead to support those features was an explicit ask of @zanieb 😅 

---

_Closed by @sharkdp on 2025-08-01 08:21_

---
