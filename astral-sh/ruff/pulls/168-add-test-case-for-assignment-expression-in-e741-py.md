```yaml
number: 168
title: "Add test case for assignment expression in `E741.py`"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: test-for-assignment-expression
created_at: 2022-09-12T11:20:41Z
updated_at: 2022-09-12T13:03:37Z
url: https://github.com/astral-sh/ruff/pull/168
synced_at: 2026-01-12T15:55:04Z
```

# Add test case for assignment expression in `E741.py`

---

_@harupy_

_No description provided._

---

_Review comment by @harupy on `resources/test/fixtures/E741.py`:74 on 2022-09-12 11:26_

`flake8` doesn't catch this violation (false negative) :)

```bash
> flake8 resources/test/fixtures/E741.py
...
resources/test/fixtures/E741.py:71:22: E741 ambiguous variable name 'l'

> cargo run resources/test/fixtures/E741.py
...
resources/test/fixtures/E741.py:71:1: E741 ambiguous variable name 'l'
resources/test/fixtures/E741.py:74:5: E741 ambiguous variable name 'l'
```

---

_@harupy reviewed on 2022-09-12 11:27_

---

_Merged by @charliermarsh on 2022-09-12 13:03_

---

_Closed by @charliermarsh on 2022-09-12 13:03_

---
