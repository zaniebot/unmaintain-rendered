```yaml
number: 6287
title: "Avoid treating `uv add -r` as `--raw-sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
assignees: []
merged: true
base: main
head: charlie/rev
created_at: 2024-08-21T00:04:02Z
updated_at: 2024-08-21T16:28:04Z
url: https://github.com/astral-sh/uv/pull/6287
synced_at: 2026-01-10T13:09:51Z
```

# Avoid treating `uv add -r` as `--raw-sources`

---

_Pull request opened by @charliermarsh on 2024-08-21 00:04_

## Summary

I suspect this was added because there's no way for users to pass (e.g.) `--tag`, so the references are ambiguous. I think it's better to write them as `rev` than to fail, though. It's just less efficient when we fetch.

Closes https://github.com/astral-sh/uv/issues/6276.

Closes https://github.com/astral-sh/uv/issues/6275.


---

_Label `cli` added by @charliermarsh on 2024-08-21 00:04_

---

_Review requested from @zanieb by @charliermarsh on 2024-08-21 00:04_

---

_Review requested from @konstin by @charliermarsh on 2024-08-21 00:04_

---

_Renamed from "Avoid treating `uv add -r` as `--raw-sources" to "Avoid treating `uv add -r` as `--raw-sources`" by @charliermarsh on 2024-08-21 00:04_

---

_Comment by @zanieb on 2024-08-21 03:02_

Can you leave #6275 open or ensure this is covered in the documentation?

---

_@zanieb approved on 2024-08-21 03:02_

---

_@konstin approved on 2024-08-21 06:46_

---

_Merged by @zanieb on 2024-08-21 16:28_

---

_Closed by @zanieb on 2024-08-21 16:28_

---

_Branch deleted on 2024-08-21 16:28_

---
