```yaml
number: 10998
title: "Update `compile_enumerate_no_versions ` snapshot"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2025-01-27T18:53:43Z
updated_at: 2025-01-27T19:18:17Z
url: https://github.com/astral-sh/uv/pull/10998
synced_at: 2026-01-10T11:45:22Z
```

# Update `compile_enumerate_no_versions ` snapshot

---

_Pull request opened by @charliermarsh on 2025-01-27 18:53_

## Summary

I think the "available versions" may not filter on `--exclude-newer`, since it's marked as an incompatibility? In which case, this error message can change as versions are published.

---

_Label `testing` added by @charliermarsh on 2025-01-27 18:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-01-27 18:54_

---

_Comment by @zanieb on 2025-01-27 18:58_

That doesn't sound right — we explicitly treat unavailable versions due to exclude newer as entirely missing during error messaging for reproducibility.

---

_Comment by @zanieb on 2025-01-27 18:58_

https://github.com/astral-sh/uv/blob/8b6383ebe85eddaf888b70a9146fa75548d7eb82/crates/uv-resolver/src/candidate_selector.rs#L495-L510

---

_Comment by @zanieb on 2025-01-27 18:59_

but I guess it's possible it manifests still somehow?

---

_Comment by @charliermarsh on 2025-01-27 18:59_

Yeah, but this is based on `available_versions`? And `available_versions` is populated by `versions_response`.

---

_Comment by @zanieb on 2025-01-27 19:00_

I see, that makes sense I think.

---

_Comment by @charliermarsh on 2025-01-27 19:00_

I can fix it at some point (even later today), I just want to get this passing to unblock.

---

_@zanieb approved on 2025-01-27 19:17_

---

_Merged by @charliermarsh on 2025-01-27 19:18_

---

_Closed by @charliermarsh on 2025-01-27 19:18_

---

_Branch deleted on 2025-01-27 19:18_

---
