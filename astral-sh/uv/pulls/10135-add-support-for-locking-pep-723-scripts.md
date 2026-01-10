```yaml
number: 10135
title: Add support for locking PEP 723 scripts
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/script-target
created_at: 2024-12-24T02:32:35Z
updated_at: 2025-01-08T18:36:55Z
url: https://github.com/astral-sh/uv/pull/10135
synced_at: 2026-01-10T11:44:36Z
```

# Add support for locking PEP 723 scripts

---

_Pull request opened by @charliermarsh on 2024-12-24 02:32_

## Summary

You can now run `uv lock --script main.py` to lock a given script (though as of this PR, the script itself isn't used anywhere).

Closes https://github.com/astral-sh/uv/issues/6318.


---

_Converted to draft by @charliermarsh on 2024-12-24 02:45_

---

_Label `no-build` added by @charliermarsh on 2024-12-24 02:45_

---

_Label `no-test` added by @charliermarsh on 2024-12-24 02:45_

---

_Label `enhancement` added by @charliermarsh on 2024-12-24 02:45_

---

_Label `no-build` removed by @charliermarsh on 2024-12-25 20:07_

---

_Label `no-test` removed by @charliermarsh on 2024-12-25 20:07_

---

_Marked ready for review by @charliermarsh on 2024-12-25 20:17_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-26 20:02_

---

_@zanieb reviewed on 2025-01-08 18:02_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3095 on 2025-01-08 18:02_

Let's mention where the lockfile will be written too.

---

_@zanieb approved on 2025-01-08 18:02_

---

_Merged by @charliermarsh on 2025-01-08 18:36_

---

_Closed by @charliermarsh on 2025-01-08 18:36_

---

_Branch deleted on 2025-01-08 18:36_

---
