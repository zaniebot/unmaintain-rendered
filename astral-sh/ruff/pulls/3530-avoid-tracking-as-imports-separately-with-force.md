```yaml
number: 3530
title: Avoid tracking as-imports separately with force-single-line
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/force-single-line
created_at: 2023-03-15T02:20:08Z
updated_at: 2023-03-15T02:31:13Z
url: https://github.com/astral-sh/ruff/pull/3530
synced_at: 2026-01-12T04:39:45Z
```

# Avoid tracking as-imports separately with force-single-line

---

_Pull request opened by @charliermarsh on 2023-03-15 02:20_

## Summary

This fixes a subtle bug in `force-single-line`.

With default settings (`force-single-line = false` and `combine-as-imports = false`), we track the `as` imports in a separate struct, and place those after all of the non-`as` imports. When `combine-as-imports = true`, we track them all together, which works fine too.

Now, if `force-single-line = true` but `combine-as-imports = false`, we still track the `as` imports separately, but then split them out later, which causes them to all appear at the end, after the non-`as` imports. Thus, the `as` imports actually appear out-of-order.

The fix here is to track the `as` imports alongside all the rest when `force-single-line` is enabled.

Closes #3528.


---

_Label `bug` added by @charliermarsh on 2023-03-15 02:20_

---

_Merged by @charliermarsh on 2023-03-15 02:26_

---

_Closed by @charliermarsh on 2023-03-15 02:26_

---

_Branch deleted on 2023-03-15 02:26_

---

_Comment by @github-actions[bot] on 2023-03-15 02:31_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---
