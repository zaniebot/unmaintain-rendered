```yaml
number: 8013
title: "[docs] fix typo"
type: pull_request
state: merged
author: eikowagenknecht
labels:
  - documentation
assignees: []
merged: true
base: main
head: eikowagenknecht-patch-1
created_at: 2023-10-17T13:52:34Z
updated_at: 2023-10-17T14:23:55Z
url: https://github.com/astral-sh/ruff/pull/8013
synced_at: 2026-01-12T02:32:41Z
```

# [docs] fix typo

---

_Pull request opened by @eikowagenknecht on 2023-10-17 13:52_

## Summary

Fix a typo in the docs for quote style.

> a = "a string without any quotes"
> b = "It's monday morning"
> Ruff will change a to use single quotes when using quote-style = "single". However, a will be unchanged, as converting to single quotes would require the inner ' to be escaped, which leads to less readable code: 'It\'s monday morning'.

It should read "However, **b** will be unchanged".

## Test Plan

N/A.


---

_Label `documentation` added by @charliermarsh on 2023-10-17 13:56_

---

_@charliermarsh approved on 2023-10-17 13:56_

Thank you!

---

_Merged by @charliermarsh on 2023-10-17 14:16_

---

_Closed by @charliermarsh on 2023-10-17 14:16_

---

_Comment by @github-actions[bot] on 2023-10-17 14:23_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
