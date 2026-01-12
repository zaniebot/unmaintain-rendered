```yaml
number: 5943
title: "Make `--upgrade` imply `--refresh`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/refresh-policy
created_at: 2024-08-08T23:29:46Z
updated_at: 2024-08-09T00:11:33Z
url: https://github.com/astral-sh/uv/pull/5943
synced_at: 2026-01-12T16:07:07Z
```

# Make `--upgrade` imply `--refresh`

---

_@charliermarsh_

## Summary

I think this seems reasonable... Otherwise, we might not go back to PyPI to revalidate the list of available versions despite the user passing `--upgrade`.


---

_Label `cli` added by @charliermarsh on 2024-08-08 23:29_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-08 23:29_

---

_@zanieb approved on 2024-08-09 00:10_

---

_Comment by @zanieb on 2024-08-09 00:10_

Seems like a gotcha otherwise.

---

_Merged by @charliermarsh on 2024-08-09 00:11_

---

_Closed by @charliermarsh on 2024-08-09 00:11_

---

_Branch deleted on 2024-08-09 00:11_

---
