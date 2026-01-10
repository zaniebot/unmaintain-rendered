```yaml
number: 6706
title: "Avoid writing invalid PEP 723 scripts on `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unc
created_at: 2024-08-27T17:24:26Z
updated_at: 2024-08-27T17:49:10Z
url: https://github.com/astral-sh/uv/pull/6706
synced_at: 2026-01-10T13:09:51Z
```

# Avoid writing invalid PEP 723 scripts on `tool.uv.sources`

---

_Pull request opened by @charliermarsh on 2024-08-27 17:24_

## Summary

We were writing empty lines between the dependencies and the `tool.uv.sources` table, which led to the `/// script` tag being unclosed and thus not recognized.

Closes https://github.com/astral-sh/uv/issues/6700.


---

_Review requested from @zanieb by @charliermarsh on 2024-08-27 17:24_

---

_Review requested from @konstin by @charliermarsh on 2024-08-27 17:24_

---

_Label `bug` added by @charliermarsh on 2024-08-27 17:24_

---

_@zanieb reviewed on 2024-08-27 17:29_

---

_Review comment by @zanieb on `crates/uv/tests/edit.rs`:3957 on 2024-08-27 17:29_

We had test coverage!? Can we add test coverage here to make sure the script _works_ with `uv run`?

---

_@zanieb approved on 2024-08-27 17:29_

---

_Merged by @charliermarsh on 2024-08-27 17:49_

---

_Closed by @charliermarsh on 2024-08-27 17:49_

---

_Branch deleted on 2024-08-27 17:49_

---
