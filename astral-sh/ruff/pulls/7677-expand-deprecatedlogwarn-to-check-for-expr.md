```yaml
number: 7677
title: "Expand `DeprecatedLogWarn` to check for `Expr::Atrribute` calls "
type: pull_request
state: merged
author: qdegraaf
labels: []
assignees: []
merged: true
base: main
head: fix/pgh002
created_at: 2023-09-27T14:08:53Z
updated_at: 2023-09-27T15:38:52Z
url: https://github.com/astral-sh/ruff/pull/7677
synced_at: 2026-01-12T02:39:10Z
```

# Expand `DeprecatedLogWarn` to check for `Expr::Atrribute` calls 

---

_Pull request opened by @qdegraaf on 2023-09-27 14:08_

## Summary

`PGH002`, which checks for use of deprecated `logging.warn` calls, did not check for calls made on the attribute `warn` yet. Since https://github.com/astral-sh/ruff/pull/7521 we check both cases for similar rules wherever possible. To be consistent this PR expands PGH002 to do the same.

## Test Plan

Expanded existing fixtures with `logger.warn()` calls

## Issue links

Fixes final inconsistency mentioned in https://github.com/astral-sh/ruff/issues/7502 


---

_Comment by @github-actions[bot] on 2023-09-27 14:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-09-27 15:38_

Thanks!

---

_Merged by @charliermarsh on 2023-09-27 15:38_

---

_Closed by @charliermarsh on 2023-09-27 15:38_

---
