```yaml
number: 14789
title: "Add `--all-groups` to `uv pip install` and `uv pip sync`"
type: pull_request
state: open
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
base: main
head: charlie/all-groups
created_at: 2025-07-21T14:32:35Z
updated_at: 2025-07-22T12:33:49Z
url: https://github.com/astral-sh/uv/pull/14789
synced_at: 2026-01-10T06:53:02Z
```

# Add `--all-groups` to `uv pip install` and `uv pip sync`

---

_Pull request opened by @charliermarsh on 2025-07-21 14:32_

## Summary

`--all-groups` doesn't accept a path; instead, it assumes the `pyproject.toml` in the root.

Closes #14483.

---

_Marked ready for review by @charliermarsh on 2025-07-21 14:57_

---

_Review requested from @Gankra by @charliermarsh on 2025-07-21 14:57_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-21 14:57_

---

_Label `enhancement` added by @charliermarsh on 2025-07-21 14:58_

---

_Label `cli` added by @charliermarsh on 2025-07-21 14:58_

---

_@zanieb reviewed on 2025-07-22 12:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:1863 on 2025-07-22 12:16_

Interesting, we don't have this anywhere else right now.

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:9761 on 2025-07-22 12:32_

This seems misleading, the `-r` shouldn't be needed there, right?

---

_@zanieb reviewed on 2025-07-22 12:32_

---

_@zanieb reviewed on 2025-07-22 12:33_

---

_Review comment by @zanieb on `crates/uv/tests/it/pip_install.rs`:11611 on 2025-07-22 12:33_

Gosh, I was not aware we added `-r pylock.toml --group` support. That's sort of concerning, since it doesn't match the existing `--group` semantics (i.e., not applying to other input files)

---
