```yaml
number: 5869
title: "Improve `--python` CLI documentation"
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - cli
assignees: []
merged: true
base: main
head: zb/docs-cli-python
created_at: 2024-08-07T15:21:42Z
updated_at: 2024-08-07T16:37:12Z
url: https://github.com/astral-sh/uv/pull/5869
synced_at: 2026-01-10T13:31:54Z
```

# Improve `--python` CLI documentation

---

_Pull request opened by @zanieb on 2024-08-07 15:21_

Closes #4400 

---

_Label `documentation` added by @zanieb on 2024-08-07 15:21_

---

_Label `cli` added by @zanieb on 2024-08-07 15:21_

---

_@zanieb reviewed on 2024-08-07 15:50_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2438 on 2024-08-07 15:50_

This is not correct, moved this fix into https://github.com/astral-sh/uv/pull/5870

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:248 on 2024-08-07 16:02_

Intentional that period was removed here?

---

_@charliermarsh reviewed on 2024-08-07 16:02_

---

_@charliermarsh approved on 2024-08-07 16:03_

---

_@zanieb reviewed on 2024-08-07 16:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:248 on 2024-08-07 16:07_

Yes, I switched it to verbatim mode which I think would retain the period but clap strips them everywhere else. We need to drop the period for consistency. Kind of awkward, https://github.com/clap-rs/clap/issues/3367

---

_Merged by @zanieb on 2024-08-07 16:37_

---

_Closed by @zanieb on 2024-08-07 16:37_

---

_Branch deleted on 2024-08-07 16:37_

---
