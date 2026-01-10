```yaml
number: 8550
title: "Error when disallowed settings are defined in `uv.toml`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: tracking/050
head: charlie/clear
created_at: 2024-10-24T23:37:31Z
updated_at: 2024-10-25T16:22:41Z
url: https://github.com/astral-sh/uv/pull/8550
synced_at: 2026-01-10T12:54:12Z
```

# Error when disallowed settings are defined in `uv.toml`

---

_Pull request opened by @charliermarsh on 2024-10-24 23:37_

## Summary

These settings can only be defined in `pyproject.toml`, since they're project-centric, and not _configuration_.

Closes https://github.com/astral-sh/uv/issues/8539.


---

_Label `error messages` added by @charliermarsh on 2024-10-24 23:37_

---

_Review requested from @zanieb by @charliermarsh on 2024-10-25 00:31_

---

_Review requested from @konstin by @charliermarsh on 2024-10-25 00:31_

---

_Comment by @zanieb on 2024-10-25 04:06_

Should we do this in 0.5.0 since we're already prepping a breaking release? 

---

_Review comment by @konstin on `crates/uv-settings/src/lib.rs`:286 on 2024-10-25 07:14_

```suggestion
```

solved by boxing, i think, at least it doesn't fire anymore

---

_@konstin approved on 2024-10-25 07:15_

---

_Label `breaking` added by @charliermarsh on 2024-10-25 13:20_

---

_Added to milestone `v0.5.0` by @charliermarsh on 2024-10-25 13:20_

---

_Comment by @charliermarsh on 2024-10-25 13:20_

@zanieb -- Sounds good.

---

_@zanieb approved on 2024-10-25 16:02_

---

_Merged by @zanieb on 2024-10-25 16:22_

---

_Closed by @zanieb on 2024-10-25 16:22_

---

_Branch deleted on 2024-10-25 16:22_

---
