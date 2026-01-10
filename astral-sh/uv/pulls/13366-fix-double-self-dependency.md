```yaml
number: 13366
title: Fix double self-dependency
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/fix-self-dependency-twice
created_at: 2025-05-09T16:32:08Z
updated_at: 2025-05-13T03:03:45Z
url: https://github.com/astral-sh/uv/pull/13366
synced_at: 2026-01-10T11:10:41Z
```

# Fix double self-dependency

---

_Pull request opened by @konstin on 2025-05-09 16:32_

The fix itself and its documentation live in pubgrub: https://github.com/astral-sh/pubgrub/pull/44

Fixes #13344

---

_Label `bug` added by @konstin on 2025-05-09 16:32_

---

_@charliermarsh approved on 2025-05-10 14:48_

---

_Closed by @charliermarsh on 2025-05-10 14:49_

---

_Reopened by @charliermarsh on 2025-05-10 14:49_

---

_@cthoyt reviewed on 2025-05-10 16:26_

---

_Review comment by @cthoyt on `crates/uv/tests/it/pip_compile.rs`:17420 on 2025-05-10 16:26_

just for completeness sake, can you imagine a scenario where you have `foo` and a double self dependency `foo[extra-bar]` that also has an extra with it?

---

_Merged by @charliermarsh on 2025-05-13 03:03_

---

_Closed by @charliermarsh on 2025-05-13 03:03_

---

_Branch deleted on 2025-05-13 03:03_

---
