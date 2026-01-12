```yaml
number: 20210
title: "[ty] Run ecosystem-analyzer in release mode"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/ecosystem-analyzer-release-mode
created_at: 2025-09-03T10:45:46Z
updated_at: 2025-09-03T10:51:56Z
url: https://github.com/astral-sh/ruff/pull/20210
synced_at: 2026-01-12T15:56:56Z
```

# [ty] Run ecosystem-analyzer in release mode

---

_@sharkdp_

## Summary

We already have `mypy_primer` running in debug mode, so this should provide some additional coverage, and allows us produce reasonable timing results. This also makes the ecosystem-analyzer workflow much faster (3 min vs 8 min).

## Test Plan

CI run on this PR

---

_Label `ci` added by @sharkdp on 2025-09-03 10:45_

---

_Label `ty` added by @sharkdp on 2025-09-03 10:45_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-09-03 10:45_

---

_Comment by @github-actions[bot] on 2025-09-03 10:50_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results

No diagnostic changes detected ✅
**[Full report with detailed diff](https://david-ecosystem-analyzer-rel.ecosystem-663.pages.dev/diff)**


---

_Merged by @sharkdp on 2025-09-03 10:51_

---

_Closed by @sharkdp on 2025-09-03 10:51_

---

_Branch deleted on 2025-09-03 10:51_

---
