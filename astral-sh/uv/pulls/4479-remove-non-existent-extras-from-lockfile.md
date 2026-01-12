```yaml
number: 4479
title: Remove non-existent extras from lockfile
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - preview
assignees: []
merged: true
base: main
head: charlie/ex
created_at: 2024-06-24T17:31:11Z
updated_at: 2024-06-24T18:56:57Z
url: https://github.com/astral-sh/uv/pull/4479
synced_at: 2026-01-12T16:06:15Z
```

# Remove non-existent extras from lockfile

---

_@charliermarsh_

## Summary

Ultimately decided to view this as part of `LockWire` normalization: removing references to extras that don't exist. I think it would be nice if the resolver avoided omitting these, but I don't know if it's fully possible.

Closes https://github.com/astral-sh/uv/issues/4405.


---

_Label `bug` added by @charliermarsh on 2024-06-24 17:31_

---

_Label `preview` added by @charliermarsh on 2024-06-24 17:31_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-24 17:31_

---

_Comment by @charliermarsh on 2024-06-24 17:32_

Another option is to leave these in the lockfile and not validate them at all.

---

_@BurntSushi approved on 2024-06-24 18:28_

LGTM. I like filtering them out instead of including them I think.

---

_Merged by @charliermarsh on 2024-06-24 18:56_

---

_Closed by @charliermarsh on 2024-06-24 18:56_

---

_Branch deleted on 2024-06-24 18:56_

---
