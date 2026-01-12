```yaml
number: 2564
title: "Implement pylint's `too-many-return-statements` rule (`PLR0911`)"
type: pull_request
state: merged
author: chanman3388
labels:
  - rule
assignees: []
merged: true
base: main
head: PLR0911
created_at: 2023-02-04T11:31:17Z
updated_at: 2023-02-04T21:57:10Z
url: https://github.com/astral-sh/ruff/pull/2564
synced_at: 2026-01-12T15:55:08Z
```

# Implement pylint's `too-many-return-statements` rule (`PLR0911`)

---

_@chanman3388_

https://pylint.pycqa.org/en/latest/user_guide/messages/refactor/too-many-return-statements.html

Issue #970 
Required by #2414

---

_@charliermarsh reviewed on 2023-02-04 14:20_

---

_Review comment by @charliermarsh on `resources/test/fixtures/pylint/too_many_return_statements.py`:2 on 2023-02-04 14:20_

Do you mind instead including the GitHub permalink?

---

_Review comment by @charliermarsh on `src/rules/pylint/rules/too_many_return_statements.rs`:27 on 2023-02-04 14:22_

Could we instead reuse `ReturnStatementVisitor` here, from `src/rules/flake8_annotations/rules.rs`?

---

_@charliermarsh reviewed on 2023-02-04 14:22_

---

_Merged by @charliermarsh on 2023-02-04 21:56_

---

_Closed by @charliermarsh on 2023-02-04 21:56_

---

_Label `rule` added by @charliermarsh on 2023-02-04 21:57_

---
