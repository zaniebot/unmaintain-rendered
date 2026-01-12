```yaml
number: 5223
title: "Avoid TOCTOU errors in `.python-version` reads"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/toc
created_at: 2024-07-19T14:57:31Z
updated_at: 2024-07-19T15:08:21Z
url: https://github.com/astral-sh/uv/pull/5223
synced_at: 2026-01-12T16:06:42Z
```

# Avoid TOCTOU errors in `.python-version` reads

---

_@charliermarsh_

## Summary

Not a big deal, but better to try the operation and handle the failure case than to check if the file exists and _then_ read it.


---

_Label `bug` added by @charliermarsh on 2024-07-19 14:57_

---

_Label `preview` added by @charliermarsh on 2024-07-19 14:57_

---

_Marked ready for review by @charliermarsh on 2024-07-19 14:57_

---

_@zanieb approved on 2024-07-19 15:02_

Thanks!

---

_Merged by @charliermarsh on 2024-07-19 15:08_

---

_Closed by @charliermarsh on 2024-07-19 15:08_

---

_Branch deleted on 2024-07-19 15:08_

---
