```yaml
number: 12683
title: "Extend the reference documentation for `uv pip sync`"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/pip-sync-docs
created_at: 2025-04-04T23:17:37Z
updated_at: 2025-04-07T22:40:27Z
url: https://github.com/astral-sh/uv/pull/12683
synced_at: 2026-01-10T11:10:40Z
```

# Extend the reference documentation for `uv pip sync`

---

_Pull request opened by @zanieb on 2025-04-04 23:17_

See https://github.com/astral-sh/uv/issues/12680


---

_Label `documentation` added by @zanieb on 2025-04-04 23:17_

---

_Review requested from @charliermarsh by @zanieb on 2025-04-05 00:44_

---

_@charliermarsh reviewed on 2025-04-07 02:04_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:627 on 2025-04-07 02:04_

Can we keep the trailing period? :)

---

_@charliermarsh approved on 2025-04-07 02:05_

---

_@zanieb reviewed on 2025-04-07 16:37_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:627 on 2025-04-07 16:37_

Oh, I thought we intentionally omitted them? I didn't realize this changed.

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:627 on 2025-04-07 16:37_

```suggestion
    /// Sync an environment with a `requirements.txt` file.
```

---

_@zanieb reviewed on 2025-04-07 16:37_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:627 on 2025-04-07 16:37_

It's trimmed on display by Clap, I think.

---

_@zanieb reviewed on 2025-04-07 16:37_

---

_Merged by @zanieb on 2025-04-07 22:40_

---

_Closed by @zanieb on 2025-04-07 22:40_

---

_Branch deleted on 2025-04-07 22:40_

---
