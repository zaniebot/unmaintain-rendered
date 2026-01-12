```yaml
number: 19592
title: "[ty] Fix \"peek definition\" in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
  - playground
  - ty
assignees: []
merged: true
base: main
head: fix/peek-definition-empty-previews
created_at: 2025-07-28T07:44:34Z
updated_at: 2025-07-29T20:12:11Z
url: https://github.com/astral-sh/ruff/pull/19592
synced_at: 2026-01-12T15:56:43Z
```

# [ty] Fix "peek definition" in playground

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ty/issues/907

The implementation now creates the models when maping the navigation targets. 
This is also closer to what we do in the LSP server.

## Test Plan

https://github.com/user-attachments/assets/159c41b8-ba22-493e-aa91-ec74060823d2


---

_Renamed from "[ty] Fix definition peek" to "[ty] Fix "peek definition" in playground" by @MichaReiser on 2025-07-28 07:45_

---

_Label `bug` added by @MichaReiser on 2025-07-28 07:46_

---

_Label `playground` added by @MichaReiser on 2025-07-28 07:46_

---

_@sharkdp approved on 2025-07-28 08:01_

Thank you!

---

_Merged by @MichaReiser on 2025-07-28 08:13_

---

_Closed by @MichaReiser on 2025-07-28 08:13_

---

_Branch deleted on 2025-07-28 08:13_

---

_Label `ty` added by @ntBre on 2025-07-29 20:12_

---
