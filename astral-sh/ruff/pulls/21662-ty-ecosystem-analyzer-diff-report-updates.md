```yaml
number: 21662
title: "[ty] Ecosystem analyzer: diff report updates"
type: pull_request
state: merged
author: sharkdp
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: david/ecosystem-analyzer-updates
created_at: 2025-11-27T15:41:48Z
updated_at: 2025-11-27T15:47:03Z
url: https://github.com/astral-sh/ruff/pull/21662
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Ecosystem analyzer: diff report updates

---

_Pull request opened by @sharkdp on 2025-11-27 15:41_

## Summary

Pulls in an ecosystem-analyzer change with a few updates to the diff report:

* Breakdown of added/removed/changed diagnostics by project
* Option to filter diagnostics by project
* Small button to copy a file path to the clipboard
* `(-R +A ~C)` indicators in the filter dropdowns (removed, added, changed)
* More concise layout, less scrolling

## Test Plan

Tested on https://github.com/astral-sh/ruff/pull/21553 => https://david-generic-implicit-alias.ecosystem-663.pages.dev/diff

<img width="1178" height="1248" alt="image" src="https://github.com/user-attachments/assets/70a507fe-393e-451a-8663-3e31735ce389" />


---

_Label `ci` added by @sharkdp on 2025-11-27 15:41_

---

_Label `ty` added by @sharkdp on 2025-11-27 15:41_

---

_Merged by @sharkdp on 2025-11-27 15:47_

---

_Closed by @sharkdp on 2025-11-27 15:47_

---

_Branch deleted on 2025-11-27 15:47_

---
