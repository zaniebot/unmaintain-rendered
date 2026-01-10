```yaml
number: 16607
title: "Remove fast path from `uv-git` fetch"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/fast
created_at: 2025-11-06T02:26:14Z
updated_at: 2025-11-07T04:18:07Z
url: https://github.com/astral-sh/uv/pull/16607
synced_at: 2026-01-10T06:28:12Z
```

# Remove fast path from `uv-git` fetch

---

_Pull request opened by @charliermarsh on 2025-11-06 02:26_

## Summary

Now that we perform this fast-path in `crates/uv-distribution/src/source/mod.rs`, I _think_ the fast-path here is no longer used? In my testing, we only actually took this path when the fast-path _already_ failed (and thus it would fail again, wasting time).


---

_Renamed from "Remove fast path from uv-git fetch" to "Remove fast path from `uv-git` fetch" by @charliermarsh on 2025-11-06 02:26_

---

_Review requested from @zanieb by @charliermarsh on 2025-11-06 02:26_

---

_Review requested from @konstin by @charliermarsh on 2025-11-06 02:26_

---

_Label `internal` added by @charliermarsh on 2025-11-06 02:26_

---

_Marked ready for review by @charliermarsh on 2025-11-06 02:26_

---

_Comment by @zanieb on 2025-11-06 03:00_

cc @samypr100 (I figure you may have context — no pressure)

---

_Comment by @samypr100 on 2025-11-06 04:29_

I believe I noticed this too during testing when I had set UV_NO_GITHUB_FAST_PATH and it ended up in the now-removed fastpath regardless. Functionally they seemed somewhat the same (besides the "If-None-Match" header), not sure if it was intentional for it to happen twice.

---

_@zanieb approved on 2025-11-07 04:17_

---

_Merged by @zanieb on 2025-11-07 04:18_

---

_Closed by @zanieb on 2025-11-07 04:18_

---

_Branch deleted on 2025-11-07 04:18_

---
