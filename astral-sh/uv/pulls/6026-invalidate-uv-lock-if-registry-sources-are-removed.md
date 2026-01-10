```yaml
number: 6026
title: "Invalidate `uv.lock` if registry sources are removed"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/invalidate-index
created_at: 2024-08-12T02:26:15Z
updated_at: 2024-08-13T23:42:05Z
url: https://github.com/astral-sh/uv/pull/6026
synced_at: 2026-01-10T13:09:50Z
```

# Invalidate `uv.lock` if registry sources are removed

---

_Pull request opened by @charliermarsh on 2024-08-12 02:26_

## Summary

Now, if you resolve against a registry, then swap it out for another, we won't reuse the lockfile. (If you don't provide any registry configuration, then we won't enforce this, so that `uv lock --index-url foo` and `uv lock` is stable.)

Closes https://github.com/astral-sh/uv/issues/5920.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-12 02:26_

---

_Label `bug` added by @charliermarsh on 2024-08-12 02:26_

---

_Label `preview` added by @charliermarsh on 2024-08-12 02:26_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-08-12 19:56_

---

_@BurntSushi approved on 2024-08-13 13:30_

Seems sensible!

---

_Merged by @charliermarsh on 2024-08-13 23:42_

---

_Closed by @charliermarsh on 2024-08-13 23:42_

---

_Branch deleted on 2024-08-13 23:42_

---
