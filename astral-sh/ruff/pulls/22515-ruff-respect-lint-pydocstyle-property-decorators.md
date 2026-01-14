```yaml
number: 22515
title: "[`ruff`] Respect `lint.pydocstyle.property-decorators` in `RUF066`"
type: pull_request
state: merged
author: terror
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: custim-proper-without-return
created_at: 2026-01-12T01:24:13Z
updated_at: 2026-01-14T21:02:24Z
url: https://github.com/astral-sh/ruff/pull/22515
synced_at: 2026-01-14T21:43:07Z
```

# [`ruff`] Respect `lint.pydocstyle.property-decorators` in `RUF066`

---

_@terror_

Resolves #22216.

This diff makes `RUF066` (`property-without-return`) respect the `lint.pydocstyle.property-decorators` setting. `RUF066` is now consistent with other rules that check for property methods like `PLR0206`, `D401`, `PLR6301`, and `DOC201`

---

_Comment by @astral-sh-bot[bot] on 2026-01-12 01:34_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_@ntBre approved on 2026-01-14 21:01_

Awesome, thank you!

---

_Label `rule` added by @ntBre on 2026-01-14 21:01_

---

_Label `preview` added by @ntBre on 2026-01-14 21:01_

---

_Renamed from "Respect `lint.pydocstyle.property-decorators` in `RUF066`" to "[`ruff`] Respect `lint.pydocstyle.property-decorators` in `RUF066`" by @ntBre on 2026-01-14 21:02_

---

_Merged by @ntBre on 2026-01-14 21:02_

---

_Closed by @ntBre on 2026-01-14 21:02_

---
