```yaml
number: 4255
title: "Remove `--legacy-setup-py` command-line argument"
type: pull_request
state: merged
author: charliermarsh
labels:
  - cli
  - breaking
assignees: []
merged: true
base: release/3
head: charlie/legacy-setup-py
created_at: 2024-06-11T23:45:01Z
updated_at: 2024-08-19T20:58:01Z
url: https://github.com/astral-sh/uv/pull/4255
synced_at: 2026-01-10T13:09:50Z
```

# Remove `--legacy-setup-py` command-line argument

---

_Pull request opened by @charliermarsh on 2024-06-11 23:45_

## Summary

This is a fallback mode that we supported when we decided to use PEP 517 builds by default. I can't find a single reference to it on GitHub or in our issue tracker, so I want to drop support for it as part of v0.3.0.


---

_Label `breaking` added by @charliermarsh on 2024-06-11 23:45_

---

_Added to milestone `v0.3.0` by @charliermarsh on 2024-06-11 23:45_

---

_@zanieb approved on 2024-06-12 03:06_

Could we warn in the meantime or not worth it?

---

_Label `cli` added by @charliermarsh on 2024-06-12 14:26_

---

_Comment by @zanieb on 2024-08-19 20:23_

cc @charliermarsh needs rebase for #6218 

---

_Merged by @charliermarsh on 2024-08-19 20:57_

---

_Closed by @charliermarsh on 2024-08-19 20:57_

---

_Branch deleted on 2024-08-19 20:58_

---
