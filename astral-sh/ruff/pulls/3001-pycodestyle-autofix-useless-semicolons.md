```yaml
number: 3001
title: "[`pycodestyle`] autofix useless semicolons"
type: pull_request
state: merged
author: sbrugman
labels:
  - fixes
assignees: []
merged: true
base: main
head: autofix-useless-semicolon
created_at: 2023-02-17T23:34:10Z
updated_at: 2023-02-18T00:07:56Z
url: https://github.com/astral-sh/ruff/pull/3001
synced_at: 2026-01-12T15:55:12Z
```

# [`pycodestyle`] autofix useless semicolons

---

_@sbrugman_

_No description provided._

---

_@charliermarsh reviewed on 2023-02-17 23:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/tokens.rs`:120 on 2023-02-17 23:42_

Nit: lets make `autofix` the second argument, to match the pattern below (`flake8_quotes::rules::from_tokens`, etc.).

I think we also need to pass in `settings`, because we have to verify that this _rule_ has autofix enabled. (`flake8_quotes::rules::from_tokens` probably has an example of that internally.)

---

_Merged by @charliermarsh on 2023-02-17 23:52_

---

_Closed by @charliermarsh on 2023-02-17 23:52_

---

_Label `autofix` added by @charliermarsh on 2023-02-17 23:52_

---

_Branch deleted on 2023-02-18 00:07_

---
