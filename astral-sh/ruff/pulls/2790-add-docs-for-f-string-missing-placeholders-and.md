```yaml
number: 2790
title: Add docs for f-string-missing-placeholders and unused-variable
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs
created_at: 2023-02-12T02:40:52Z
updated_at: 2023-02-12T04:03:30Z
url: https://github.com/astral-sh/ruff/pull/2790
synced_at: 2026-01-12T04:52:01Z
```

# Add docs for f-string-missing-placeholders and unused-variable

---

_Pull request opened by @charliermarsh on 2023-02-12 02:40_

_No description provided._

---

_Label `documentation` added by @charliermarsh on 2023-02-12 02:40_

---

_Merged by @charliermarsh on 2023-02-12 02:48_

---

_Closed by @charliermarsh on 2023-02-12 02:48_

---

_Branch deleted on 2023-02-12 02:48_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pyflakes/rules/f_string_missing_placeholders.rs`:17 on 2023-02-12 04:02_

Again nit-picking but this doesn't say why using f-strings without placeholders is bad ... just the fact that you don't have to use something doesn't say that using it is bad.

Disadvantages I can think of: There's probably some runtime overhead since Python has to parse the string looking for placeholders, it's a bit misleading/confusing for readers since you'd expect a placeholder and it could indicate that you forgot to add a placeholder.

---

_@not-my-profile reviewed on 2023-02-12 04:02_

---
