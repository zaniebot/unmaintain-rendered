```yaml
number: 18228
title: switch the playground repo button to ty repo
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/repo-button
created_at: 2025-05-20T17:44:47Z
updated_at: 2025-05-21T06:35:14Z
url: https://github.com/astral-sh/ruff/pull/18228
synced_at: 2026-01-12T15:56:15Z
```

# switch the playground repo button to ty repo

---

_@carljm_

## Summary

We should point to `ty` repo, not `ruff` repo, for ty.

## Test Plan

Ran the playground locally (`npm start --workspace ty-playground`) and saw the updated link target.


---

_Review requested from @MichaReiser by @carljm on 2025-05-20 17:44_

---

_Label `internal` added by @carljm on 2025-05-20 17:44_

---

_@MichaReiser requested changes on 2025-05-20 18:09_

The same button is used for ruff. We'll need to pass the repository from outside as a prop

---

_Comment by @carljm on 2025-05-20 18:23_

Ah, ok, I figured I was probably going to do this wrong :) I suspect that doing it right would take me a lot longer than it would take you, so maybe I'll leave this for you to commandeer and update (or close and replace).

---

_Assigned to @MichaReiser by @carljm on 2025-05-20 18:23_

---

_Label `ty` added by @AlexWaygood on 2025-05-20 20:34_

---

_@MichaReiser approved on 2025-05-21 06:32_

---

_Merged by @MichaReiser on 2025-05-21 06:35_

---

_Closed by @MichaReiser on 2025-05-21 06:35_

---

_Branch deleted on 2025-05-21 06:35_

---
