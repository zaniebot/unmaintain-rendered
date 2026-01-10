```yaml
number: 15815
title: Allow escaping spaces in --env-file handling
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/env
created_at: 2025-09-12T16:08:56Z
updated_at: 2025-09-12T22:11:53Z
url: https://github.com/astral-sh/uv/pull/15815
synced_at: 2026-01-10T06:36:15Z
```

# Allow escaping spaces in --env-file handling

---

_Pull request opened by @charliermarsh on 2025-09-12 16:08_

## Summary

We allow space-delimiting for `--env-file`, but Clap doesn't support any form of escaping, so as-is, there's no way to provide a `.env` file in a directory that contains a space. We now do the splitting ourselves and respect escapes.

Closes https://github.com/astral-sh/uv/issues/15806.


---

_Label `bug` added by @charliermarsh on 2025-09-12 16:09_

---

_Review requested from @konstin by @charliermarsh on 2025-09-12 16:09_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-12 16:09_

---

_Marked ready for review by @charliermarsh on 2025-09-12 16:09_

---

_@zanieb approved on 2025-09-12 21:17_

---

_Merged by @charliermarsh on 2025-09-12 22:11_

---

_Closed by @charliermarsh on 2025-09-12 22:11_

---

_Branch deleted on 2025-09-12 22:11_

---
