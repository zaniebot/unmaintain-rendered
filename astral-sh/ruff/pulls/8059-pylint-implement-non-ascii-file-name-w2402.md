```yaml
number: 8059
title: "[pylint] - implement `non-ascii-file-name` (`W2402`)"
type: pull_request
state: closed
author: diceroll123
labels: []
assignees: []
base: main
head: add-W2402
created_at: 2023-10-19T04:21:18Z
updated_at: 2023-10-20T21:51:41Z
url: https://github.com/astral-sh/ruff/pull/8059
synced_at: 2026-01-12T02:32:41Z
```

# [pylint] - implement `non-ascii-file-name` (`W2402`)

---

_Pull request opened by @diceroll123 on 2023-10-19 04:21_

## Summary

Implement [`non-ascii-file-name`/`W2402`](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/non-ascii-file-name.html)

See #970

## Test Plan

`cargo test`

---

_Comment by @github-actions[bot] on 2023-10-19 04:38_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-20 18:20_

I think this may be redundant with `N999`, which includes ASCII validation as part of a broader check.

---

_Closed by @diceroll123 on 2023-10-20 21:51_

---
