```yaml
number: 7192
title: "Add `uv run --no-sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/no-sync
created_at: 2024-09-08T16:43:28Z
updated_at: 2024-09-10T21:29:45Z
url: https://github.com/astral-sh/uv/pull/7192
synced_at: 2026-01-12T16:07:43Z
```

# Add `uv run --no-sync`

---

_@charliermarsh_

## Summary

When `--no-sync` is provided, we won't lock or sync, but we will run the command in the project environment.

Closes https://github.com/astral-sh/uv/issues/7165.


---

_Review requested from @zanieb by @charliermarsh on 2024-09-08 16:43_

---

_Label `cli` added by @charliermarsh on 2024-09-08 16:43_

---

_@zanieb approved on 2024-09-10 18:10_

Can you document this in the project concept page?

---

_Comment by @charliermarsh on 2024-09-10 21:29_

Unrelated Windows failure: `PermissionError: [Errno 13] Permission denied: 'E:\/uv-tmp\/[TMP]/specifiers.py'`

---

_Merged by @charliermarsh on 2024-09-10 21:29_

---

_Closed by @charliermarsh on 2024-09-10 21:29_

---

_Branch deleted on 2024-09-10 21:29_

---
