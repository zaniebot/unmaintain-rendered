```yaml
number: 894
title: "Add hashes to `pip-compile` output"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/compile
created_at: 2024-01-12T04:26:12Z
updated_at: 2024-01-12T17:44:20Z
url: https://github.com/astral-sh/uv/pull/894
synced_at: 2026-01-12T16:04:16Z
```

# Add hashes to `pip-compile` output

---

_@charliermarsh_

## Summary

Adds hashes to `pip-compile` output, though we don't actually check those hashes in `pip-sync` yet.

Closes https://github.com/astral-sh/puffin/issues/131.

---

_Label `enhancement` added by @charliermarsh on 2024-01-12 04:26_

---

_Marked ready for review by @charliermarsh on 2024-01-12 04:34_

---

_Review requested from @zanieb by @charliermarsh on 2024-01-12 04:35_

---

_Review requested from @konstin by @charliermarsh on 2024-01-12 04:36_

---

_@konstin approved on 2024-01-12 09:07_

:100:

Important feature, and less intrusive than i expected

---

_@charliermarsh reviewed on 2024-01-12 13:48_

---

_Review comment by @charliermarsh on `crates/puffin-cli/tests/pip_compile.rs`:2963 on 2024-01-12 13:48_

Need backslash at the end.

---

_Merged by @charliermarsh on 2024-01-12 17:44_

---

_Closed by @charliermarsh on 2024-01-12 17:44_

---

_Branch deleted on 2024-01-12 17:44_

---
