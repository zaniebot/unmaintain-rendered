```yaml
number: 22515
title: "Respect `lint.pydocstyle.property-decorators` in `RUF066`"
type: pull_request
state: open
author: terror
labels: []
assignees: []
base: main
head: custim-proper-without-return
created_at: 2026-01-12T01:24:13Z
updated_at: 2026-01-12T01:34:05Z
url: https://github.com/astral-sh/ruff/pull/22515
synced_at: 2026-01-12T02:26:21Z
```

# Respect `lint.pydocstyle.property-decorators` in `RUF066`

---

_Pull request opened by @terror on 2026-01-12 01:24_

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
