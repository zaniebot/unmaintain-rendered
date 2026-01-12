```yaml
number: 7627
title: Avoid searching for bracketed comments in unparenthesized generators
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: charlie/gen
created_at: 2023-09-24T01:57:32Z
updated_at: 2023-09-24T02:08:46Z
url: https://github.com/astral-sh/ruff/pull/7627
synced_at: 2026-01-12T15:55:24Z
```

# Avoid searching for bracketed comments in unparenthesized generators

---

_@charliermarsh_

Similar to tuples, a generator _can_ be parenthesized or unparenthesized. Only search for bracketed comments if it contains its own parentheses.

Closes https://github.com/astral-sh/ruff/issues/7623.


---

_Label `bug` added by @charliermarsh on 2023-09-24 01:57_

---

_Label `formatter` added by @charliermarsh on 2023-09-24 01:57_

---

_Merged by @charliermarsh on 2023-09-24 02:08_

---

_Closed by @charliermarsh on 2023-09-24 02:08_

---

_Branch deleted on 2023-09-24 02:08_

---
