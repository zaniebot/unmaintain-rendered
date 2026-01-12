```yaml
number: 9718
title: "Allow users to specify URLs in `project.dependencies` and `tool.uv.sources`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/urls
created_at: 2024-12-08T13:45:17Z
updated_at: 2024-12-09T17:16:10Z
url: https://github.com/astral-sh/uv/pull/9718
synced_at: 2026-01-12T16:08:57Z
```

# Allow users to specify URLs in `project.dependencies` and `tool.uv.sources`

---

_@charliermarsh_

## Summary

This PR allows users to specify a source both in `project.dependencies` ("production") and `tool.uv.sources` ("development"). It's not intended as a holistic fix for "production" vs. "development" dependencies, but in some cases this is good enough with `--no-sources`, and I don't see a great reason for enforcing it right now.

Closes: https://github.com/astral-sh/uv/issues/9682
Ref: https://github.com/astral-sh/uv/issues/7945 (but I'll leave this open?)


---

_Review requested from @zanieb by @charliermarsh on 2024-12-08 13:45_

---

_Review requested from @konstin by @charliermarsh on 2024-12-08 13:45_

---

_Label `enhancement` added by @charliermarsh on 2024-12-08 13:45_

---

_Review comment by @T-256 on `crates/uv/tests/it/lock.rs`:9743 on 2024-12-08 16:25_

perhaps needs rename?

---

_@T-256 reviewed on 2024-12-08 16:25_

Nice DX feature!

---

_@konstin approved on 2024-12-09 08:25_

---

_Merged by @charliermarsh on 2024-12-09 17:16_

---

_Closed by @charliermarsh on 2024-12-09 17:16_

---

_Branch deleted on 2024-12-09 17:16_

---
