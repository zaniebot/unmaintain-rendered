```yaml
number: 3692
title: "`ignore` without `select` seems to work in reverse"
type: issue
state: closed
author: bersbersbers
labels: []
assignees: []
created_at: 2023-03-23T19:21:09Z
updated_at: 2023-03-23T20:21:17Z
url: https://github.com/astral-sh/ruff/issues/3692
synced_at: 2026-01-10T11:09:46Z
```

# `ignore` without `select` seems to work in reverse

---

_Issue opened by @bersbersbers on 2023-03-23 19:21_

I'll show here various numbers of errors reported by `ruff` based on `pyproject.toml` settings:

(Nothing): 12

`[tool.ruff]`: 0

`[tool.ruff]; select = ["ALL"]`:  359
`[tool.ruff]; select = ["ALL"]; ignore = ["D101", "D102", "D103", "ANN101", "RET504"]`:  109

`[tool.ruff]; ignore = ["D101", "D102", "D103", "ANN101", "RET504"]`:  250 (curiously, equals 359 - 109)

`[tool.ruff]; select = ["D101"]`:  7
`[tool.ruff]; ignore = ["D101"]`:  7

`[tool.ruff]; select = ["D102"]`:  95
`[tool.ruff]; ignore = ["D102"]`:  95

etc.

In summary, I think if nothing is `select`ed at all, `ignore` selects rather than ignores.

ruff 0.0.258

---

_Comment by @charliermarsh on 2023-03-23 19:26_

Hey sorry — this should be fixed in 0.0.259 which we just released. There was a regression here in 0.0.258. Just LMK if you’re still seeing this after upgrading!

---

_Comment by @bersbersbers on 2023-03-23 20:21_

All good now, thanks :)

---

_Closed by @bersbersbers on 2023-03-23 20:21_

---
