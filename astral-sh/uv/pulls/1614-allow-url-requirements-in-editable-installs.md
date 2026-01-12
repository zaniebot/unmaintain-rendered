```yaml
number: 1614
title: Allow URL requirements in editable installs
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/editable
created_at: 2024-02-17T20:11:33Z
updated_at: 2024-02-17T20:20:24Z
url: https://github.com/astral-sh/uv/pull/1614
synced_at: 2026-01-12T16:04:40Z
```

# Allow URL requirements in editable installs

---

_@charliermarsh_

## Summary

If an editable package declares a direct URL requirement, we currently error since it's not considered an "allowed" requirement. We need to add those URLs to the allow-list.

Closes https://github.com/astral-sh/uv/issues/1603.


---

_Label `bug` added by @charliermarsh on 2024-02-17 20:11_

---

_Comment by @zanieb on 2024-02-17 20:14_

Oh thanks I was affected by this similarly in https://github.com/astral-sh/uv/pull/1050 (although slightly different since it's a git dependency — I do editable packse installs locally too though)

---

_Merged by @charliermarsh on 2024-02-17 20:20_

---

_Closed by @charliermarsh on 2024-02-17 20:20_

---

_Branch deleted on 2024-02-17 20:20_

---
