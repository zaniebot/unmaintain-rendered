```yaml
number: 7772
title: "Avoid reusing cached downloaded binaries with `--no-binary`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/bin
created_at: 2024-09-29T01:41:37Z
updated_at: 2024-09-29T17:34:53Z
url: https://github.com/astral-sh/uv/pull/7772
synced_at: 2026-01-10T12:53:55Z
```

# Avoid reusing cached downloaded binaries with `--no-binary`

---

_Pull request opened by @charliermarsh on 2024-09-29 01:41_

## Summary

Historically, we've allowed the use of wheels that were downloaded from PyPI even when the user passes `--no-binary`, if the wheel exists in the cache. This PR modifies the cache lookup code such that we respect `--no-build` and `--no-binary` in those paths.

Closes https://github.com/astral-sh/uv/issues/2154.


---

_Marked ready for review by @charliermarsh on 2024-09-29 01:41_

---

_Label `bug` added by @charliermarsh on 2024-09-29 01:41_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-29 01:42_

---

_@zanieb approved on 2024-09-29 15:33_

---

_Merged by @charliermarsh on 2024-09-29 17:34_

---

_Closed by @charliermarsh on 2024-09-29 17:34_

---

_Branch deleted on 2024-09-29 17:34_

---
