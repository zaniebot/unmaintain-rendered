```yaml
number: 5948
title: Create snake_case file if linter is Pylint
type: pull_request
state: merged
author: tjkuson
labels:
  - internal
assignees: []
merged: true
base: main
head: add_rule_script
created_at: 2023-07-21T15:08:27Z
updated_at: 2023-07-22T14:05:35Z
url: https://github.com/astral-sh/ruff/pull/5948
synced_at: 2026-01-12T15:55:20Z
```

# Create snake_case file if linter is Pylint

---

_@tjkuson_

## Summary

The `add_rule.py` script would create a test case that pointed to a file that didn't exist when the linter is set to `"pylint"`. This PR fixes that.

## Test Plan

`python scripts/add_rule.py --name DoTheThing --prefix PL --code C0999 --linter pylint`


---

_Merged by @charliermarsh on 2023-07-22 02:13_

---

_Closed by @charliermarsh on 2023-07-22 02:13_

---

_Label `internal` added by @charliermarsh on 2023-07-22 02:13_

---

_Comment by @charliermarsh on 2023-07-22 02:13_

Thx.

---

_Branch deleted on 2023-07-22 14:05_

---
