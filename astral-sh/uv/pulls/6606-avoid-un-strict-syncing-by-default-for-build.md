```yaml
number: 6606
title: Avoid un-strict syncing by-default for build isolation
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/build-iso
created_at: 2024-08-25T14:16:49Z
updated_at: 2024-08-26T18:05:00Z
url: https://github.com/astral-sh/uv/pull/6606
synced_at: 2026-01-10T13:09:51Z
```

# Avoid un-strict syncing by-default for build isolation

---

_Pull request opened by @charliermarsh on 2024-08-25 14:16_

## Summary

Closes https://github.com/astral-sh/uv/issues/6580.

Closes https://github.com/astral-sh/uv/issues/6599.


---

_Label `bug` added by @charliermarsh on 2024-08-25 14:16_

---

_Comment by @charliermarsh on 2024-08-25 14:16_

(Needs docs.)

---

_Comment by @charliermarsh on 2024-08-25 14:33_

(Docs separately, they go beyond the changes here.)

---

_Review requested from @zanieb by @zanieb on 2024-08-25 14:35_

---

_@zanieb approved on 2024-08-26 16:58_

You'll need to update the docs for `--inexact` too

https://github.com/astral-sh/uv/blob/9d1cd8e48c534544ada11ac2c95832b7b687382f/crates/uv-cli/src/lib.rs#L2265-L2267

---

_Merged by @charliermarsh on 2024-08-26 18:04_

---

_Closed by @charliermarsh on 2024-08-26 18:04_

---

_Branch deleted on 2024-08-26 18:05_

---
