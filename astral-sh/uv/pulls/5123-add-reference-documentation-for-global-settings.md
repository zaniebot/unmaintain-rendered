```yaml
number: 5123
title: Add reference documentation for global settings
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/api-reference-ii
created_at: 2024-07-16T19:25:06Z
updated_at: 2024-07-16T20:50:06Z
url: https://github.com/astral-sh/uv/pull/5123
synced_at: 2026-01-10T13:42:52Z
```

# Add reference documentation for global settings

---

_Pull request opened by @charliermarsh on 2024-07-16 19:25_

## Summary

Second part of: https://github.com/astral-sh/uv/issues/5093.


---

_Review requested from @zanieb by @charliermarsh on 2024-07-16 19:25_

---

_Label `documentation` added by @charliermarsh on 2024-07-16 19:25_

---

_Marked ready for review by @charliermarsh on 2024-07-16 19:25_

---

_@zanieb reviewed on 2024-07-16 20:11_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:61 on 2024-07-16 20:11_

I thought we were explicitly not using backticks for "uv" anywhere.

---

_@zanieb reviewed on 2024-07-16 20:12_

---

_Review comment by @zanieb on `crates/uv-settings/src/settings.rs`:85 on 2024-07-16 20:12_

Should we note that we use a temporary cache for in-process purposes?

---

_@zanieb approved on 2024-07-16 20:13_

---

_@charliermarsh reviewed on 2024-07-16 20:16_

---

_Review comment by @charliermarsh on `crates/uv-settings/src/settings.rs`:61 on 2024-07-16 20:16_

Haha. This is copied from the CLI... I'll have to change it there too.

---

_Comment by @charliermarsh on 2024-07-16 20:41_

Changed existing backticked uv references, removing the backticks.

---

_Merged by @charliermarsh on 2024-07-16 20:50_

---

_Closed by @charliermarsh on 2024-07-16 20:50_

---

_Branch deleted on 2024-07-16 20:50_

---
