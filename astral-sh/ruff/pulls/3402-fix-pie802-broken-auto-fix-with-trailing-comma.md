```yaml
number: 3402
title: Fix PIE802 broken auto-fix with trailing comma
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: broken-pie802-auto-fix
created_at: 2023-03-08T14:13:14Z
updated_at: 2023-03-08T18:05:39Z
url: https://github.com/astral-sh/ruff/pull/3402
synced_at: 2026-01-12T04:39:44Z
```

# Fix PIE802 broken auto-fix with trailing comma

---

_Pull request opened by @JonathanPlasse on 2023-03-08 14:13_

This code
```python
any(
    [x.id for x in bar],
)
```
before this PR (the trailing comma is invalid)
```python
any(
    x.id for x in bar,
)
```
after this PR
```python
any(
    x.id for x in bar)
```

The range of the diagnostic is now only on the first argument instead of the entire call.

---

_@charliermarsh reviewed on 2023-03-08 14:28_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/flake8_pie/PIE802.py`:15 on 2023-03-08 14:28_

Do you mind adding a version with a trailing comment after the `,`?

---

_@JonathanPlasse reviewed on 2023-03-08 15:33_

---

_Review comment by @JonathanPlasse on `crates/ruff/resources/test/fixtures/flake8_pie/PIE802.py`:15 on 2023-03-08 15:33_

Will do.

---

_Review comment by @JonathanPlasse on `crates/ruff/resources/test/fixtures/flake8_pie/PIE802.py`:15 on 2023-03-08 16:59_

Good call, comments after the comma would have been removed.

---

_@JonathanPlasse reviewed on 2023-03-08 16:59_

---

_Merged by @charliermarsh on 2023-03-08 17:49_

---

_Closed by @charliermarsh on 2023-03-08 17:49_

---

_Label `bug` added by @charliermarsh on 2023-03-08 17:49_

---

_Label `autofix` added by @charliermarsh on 2023-03-08 17:49_

---

_Branch deleted on 2023-03-08 18:05_

---
