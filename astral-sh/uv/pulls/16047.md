```yaml
number: 16047
title: Use a global flags instance for wheel check
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/flags
created_at: 2025-09-28T00:49:51Z
updated_at: 2025-09-30T00:10:12Z
url: https://github.com/astral-sh/uv/pull/16047
synced_at: 2026-01-10T06:36:15Z
```

# Use a global flags instance for wheel check

---

_Pull request opened by @charliermarsh on 2025-09-28 00:49_

## Summary

This stands up the idea proposed in https://github.com/astral-sh/uv/pull/16046/files#r2384395797.

---

_Label `internal` added by @charliermarsh on 2025-09-28 00:49_

---

_Marked ready for review by @charliermarsh on 2025-09-28 00:49_

---

_Comment by @zanieb on 2025-09-29 14:17_

I'm on the fence. I'm wary because it introduces a second path for configuration which I think could be confusing. It also adds a footgun to our Rust APIs, but I think it's okay not to prioritize those.

---

_Comment by @charliermarsh on 2025-09-29 17:11_

I'm also on the fence, but it seems like a strict improvement over #16046 and I worry that the alternative (of threading the flag through all those places) will be a net negative for maintainability.

---

_@zanieb approved on 2025-09-29 22:27_

---

_Merged by @charliermarsh on 2025-09-30 00:10_

---

_Closed by @charliermarsh on 2025-09-30 00:10_

---

_Branch deleted on 2025-09-30 00:10_

---
