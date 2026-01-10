```yaml
number: 15278
title: "Move `--config-settings` structs into `uv-distribution-types`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cs
created_at: 2025-08-14T13:25:02Z
updated_at: 2025-08-14T14:07:49Z
url: https://github.com/astral-sh/uv/pull/15278
synced_at: 2026-01-10T06:44:33Z
```

# Move `--config-settings` structs into `uv-distribution-types`

---

_Pull request opened by @charliermarsh on 2025-08-14 13:25_

## Summary

This breaks up a cycle I'm running into in incorporating the build configuration into our cache keys. This is actually a type that ends up in the frontend build system, etc., so I think it makes more sense here anyway (as opposed to `uv-configuration` which tend to be our own user-facing types).


---

_Label `internal` added by @charliermarsh on 2025-08-14 13:25_

---

_Marked ready for review by @charliermarsh on 2025-08-14 13:25_

---

_@zanieb approved on 2025-08-14 13:26_

---

_Merged by @charliermarsh on 2025-08-14 14:07_

---

_Closed by @charliermarsh on 2025-08-14 14:07_

---

_Branch deleted on 2025-08-14 14:07_

---
