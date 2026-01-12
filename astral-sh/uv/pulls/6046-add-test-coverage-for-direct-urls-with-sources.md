```yaml
number: 6046
title: Add test coverage for direct URLs with sources
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
  - preview
assignees: []
merged: true
base: main
head: charlie/url
created_at: 2024-08-12T21:45:57Z
updated_at: 2024-08-12T23:22:52Z
url: https://github.com/astral-sh/uv/pull/6046
synced_at: 2026-01-12T16:07:10Z
```

# Add test coverage for direct URLs with sources

---

_@charliermarsh_

## Summary

Ensures that we don't respect `tool.uv.sources` for (eg.) direct URL requirements, as intended.

Related to https://github.com/astral-sh/uv/issues/3943.


---

_Label `testing` added by @charliermarsh on 2024-08-12 21:46_

---

_Label `preview` added by @charliermarsh on 2024-08-12 21:46_

---

_@charliermarsh reviewed on 2024-08-12 23:06_

---

_Review comment by @charliermarsh on `crates/uv/tests/lock.rs`:6698 on 2024-08-12 23:06_

This is tracked in https://github.com/astral-sh/uv/issues/6048. I plan to fix it tonight.

---

_Merged by @charliermarsh on 2024-08-12 23:14_

---

_Closed by @charliermarsh on 2024-08-12 23:14_

---

_Branch deleted on 2024-08-12 23:14_

---
