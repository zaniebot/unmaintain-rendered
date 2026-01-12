```yaml
number: 998
title: Remove verbatim URL from path file location
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/v
created_at: 2024-01-19T03:37:24Z
updated_at: 2024-01-19T03:40:49Z
url: https://github.com/astral-sh/uv/pull/998
synced_at: 2026-01-12T16:04:21Z
```

# Remove verbatim URL from path file location

---

_@charliermarsh_

## Summary

I got confused by why `VerbatimUrl` was on `Path`. Since it's directly computed from it, I think we should just compute it as-needed. I think it's also possibly-buggy because the URL is the URL of the _directory_, not the artifact itself, which differs from other distributions.

---

_Label `bug` added by @charliermarsh on 2024-01-19 03:38_

---

_Merged by @charliermarsh on 2024-01-19 03:40_

---

_Closed by @charliermarsh on 2024-01-19 03:40_

---

_Branch deleted on 2024-01-19 03:40_

---
