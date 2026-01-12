```yaml
number: 14853
title: Fix typos in uv_build reference documentation
type: pull_request
state: merged
author: Zij-IT
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc-typos
created_at: 2025-07-23T19:34:25Z
updated_at: 2025-07-24T09:55:15Z
url: https://github.com/astral-sh/uv/pull/14853
synced_at: 2026-01-12T16:11:27Z
```

# Fix typos in uv_build reference documentation

---

_@Zij-IT_

## Summary

Fixes both typos mentioned in #14845.

Closes #14845

## Test Plan

It wasn't :D


---

_@charliermarsh reviewed on 2025-07-23 19:38_

---

_Review comment by @charliermarsh on `docs/reference/settings.md`:438 on 2025-07-23 19:38_

In TOML, can't we omit the quotes around `headers` and `scripts`?

---

_Comment by @zanieb on 2025-07-23 20:08_

Note this file is generated. You need to change it in the source then run `cargo dev generate-all`

https://github.com/astral-sh/uv/blob/e798b09aa4bb52bf0c80d1e820a0c0f5fb82bb81/crates/uv-build-backend/src/settings.rs#L168

---

_Renamed from "doc-typos: fix typos in Settings documentation" to "Fix typos in uv_build reference documentation" by @konstin on 2025-07-24 09:40_

---

_Label `documentation` added by @konstin on 2025-07-24 09:41_

---

_Merged by @konstin on 2025-07-24 09:55_

---

_Closed by @konstin on 2025-07-24 09:55_

---
