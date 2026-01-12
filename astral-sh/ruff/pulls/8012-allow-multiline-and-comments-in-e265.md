```yaml
number: 8012
title: "Allow multiline `#!` and `#%%` comments in E265"
type: pull_request
state: closed
author: charliermarsh
labels: []
assignees: []
base: main
head: charlie/E265
created_at: 2023-10-17T13:49:18Z
updated_at: 2023-10-17T15:06:04Z
url: https://github.com/astral-sh/ruff/pull/8012
synced_at: 2026-01-12T02:32:41Z
```

# Allow multiline `#!` and `#%%` comments in E265

---

_Pull request opened by @charliermarsh on 2023-10-17 13:49_

## Summary

Closes https://github.com/astral-sh/ruff/issues/8003. After reading that issue, I then found several requests for multiline shebangs and code cell separators (`#%%`) in the Pycodestyle repo, so added both.


---

_Renamed from "Allow multiline shebangs and code separators in E265" to "Allow multiline `#!` and `#%%` comments in E265" by @charliermarsh on 2023-10-17 13:49_

---

_@konstin approved on 2023-10-17 14:15_

For context, iirc pycharm switch from supporting only `#%%` to also supporting `# %%` due to formatters.

---

_Comment by @github-actions[bot] on 2023-10-17 14:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @charliermarsh on 2023-10-17 15:06_

Closing for now as originating issue was resolved (turns out we already support `#:`, the request for `#!` was a misunderstanding).

---

_Closed by @charliermarsh on 2023-10-17 15:06_

---
