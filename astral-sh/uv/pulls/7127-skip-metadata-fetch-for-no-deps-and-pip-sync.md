```yaml
number: 7127
title: "Skip metadata fetch for `--no-deps` and `pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/sync-skip
created_at: 2024-09-06T15:50:33Z
updated_at: 2024-09-07T01:26:30Z
url: https://github.com/astral-sh/uv/pull/7127
synced_at: 2026-01-12T16:07:42Z
```

# Skip metadata fetch for `--no-deps` and `pip sync`

---

_@charliermarsh_

## Summary

I think a better tradeoff here is to skip fetching metadata, even though we can't validate the extras.

It will help with situations like https://github.com/astral-sh/uv/issues/5073#issuecomment-2334235588 in which, otherwise, we have to download the wheels twice.


---

_Label `performance` added by @charliermarsh on 2024-09-06 15:50_

---

_@zanieb approved on 2024-09-06 15:59_

---

_Merged by @charliermarsh on 2024-09-07 01:26_

---

_Closed by @charliermarsh on 2024-09-07 01:26_

---

_Branch deleted on 2024-09-07 01:26_

---
