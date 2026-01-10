```yaml
number: 12356
title: "Move lowered requirement source type out of `uv-pypi-types`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - no-build
assignees: []
merged: true
base: main
head: charlie/dist-types
created_at: 2025-03-21T00:50:48Z
updated_at: 2025-03-21T01:16:14Z
url: https://github.com/astral-sh/uv/pull/12356
synced_at: 2026-01-10T11:10:39Z
```

# Move lowered requirement source type out of `uv-pypi-types`

---

_Pull request opened by @charliermarsh on 2025-03-21 00:50_

## Summary

This crate is for standards-compliant types, but this is explicitly a type that's custom to uv. It's also strange because we kind of want to reference `IndexUrl` on the registry type, but that's in a crate that _depends_ on `uv-pypi-types`, which to me is a sign that this is off.


---

_Label `no-build` added by @charliermarsh on 2025-03-21 00:55_

---

_Merged by @charliermarsh on 2025-03-21 01:16_

---

_Closed by @charliermarsh on 2025-03-21 01:16_

---

_Branch deleted on 2025-03-21 01:16_

---
