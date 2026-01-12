```yaml
number: 8801
title: "[`flake8-simplify`] Omit select context managers from `SIM117`"
type: pull_request
state: merged
author: maltevesper
labels:
  - rule
assignees: []
merged: true
base: main
head: maltev/explicitWithBlockExceptions
created_at: 2023-11-21T06:05:27Z
updated_at: 2023-11-21T12:01:00Z
url: https://github.com/astral-sh/ruff/pull/8801
synced_at: 2026-01-10T23:40:55Z
```

# [`flake8-simplify`] Omit select context managers from `SIM117`

---

_Pull request opened by @maltevesper on 2023-11-21 06:05_

Semantically it makes sense to put certain contextmanagers into separate with statements. Currently asyncio.timeout and its relatives in anyio and trio are exempt from SIM117.

Closes https://github.com/astral-sh/ruff/issues/8606

## Summary

Exempt asyncio.timeout and related functions from SIM117 (Collapse with statements where possible).
See https://github.com/astral-sh/ruff/issues/8606 for more.

## Test Plan

Extended the insta tests.

---

_Comment by @github-actions[bot] on 2023-11-21 06:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @maltevesper on 2023-11-21 09:01_

I have not figured out how to make ruff work if the context manager is assigned to a variable.

```
x = asyncio.timeout(1)
async with x:
     async with A():
          ...
```

A quick poke around revealed that x is a binding, but I did not see how to extract type information for x, given that ruff is a linter/formatter and not a typechecker I assume this functionality is currently not included.
Should I bee mistaken, this PR would need fixing up.

---

_@charliermarsh approved on 2023-11-21 11:49_

Thank you!

---

_Label `rule` added by @charliermarsh on 2023-11-21 11:49_

---

_Renamed from "Omit select context managers from SIM117 (#8606)" to "[`flake8-simplify`] Omit select context managers from `SIM117`" by @charliermarsh on 2023-11-21 11:49_

---

_Merged by @charliermarsh on 2023-11-21 11:53_

---

_Closed by @charliermarsh on 2023-11-21 11:53_

---
