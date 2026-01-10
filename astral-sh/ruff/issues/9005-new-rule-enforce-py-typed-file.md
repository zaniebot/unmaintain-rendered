---
number: 9005
title: "new rule - enforce `py.typed` file"
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-12-05T07:36:19Z
updated_at: 2024-03-03T18:40:51Z
url: https://github.com/astral-sh/ruff/issues/9005
synced_at: 2026-01-10T01:22:48Z
---

# new rule - enforce `py.typed` file

---

_Issue opened by @DetachHead on 2023-12-05 07:36_

a `py.typed` file is required for type checkers to consider the package typed. see https://peps.python.org/pep-0561/#packaging-type-information

it would be nice if ruff had a rule to enforce it

---

_Comment by @sciyoshi on 2023-12-11 18:18_

This seems like it would be out of scope for Ruff, which is for checking Python code, not for packaging verification. Can you explain why you would want this check in the first place, and why you want Ruff to check for this instead of adding a step in your CI that verifies that the `py.typed` file exists?

---

_Label `rule` added by @charliermarsh on 2024-01-08 03:44_

---

_Label `needs-decision` added by @charliermarsh on 2024-01-08 03:44_

---

_Comment by @adamtheturtle on 2024-03-03 18:40_

FWIW I use `pyright --verifytypes`.
This will error if there is no `py.typed` file.

---
