```yaml
number: 14281
title: Return to GitHub runners for Windows tests
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: main
head: zb/depot-rev
created_at: 2025-06-26T16:14:23Z
updated_at: 2025-06-27T22:05:37Z
url: https://github.com/astral-sh/uv/pull/14281
synced_at: 2026-01-12T16:11:07Z
```

# Return to GitHub runners for Windows tests

---

_@zanieb_

While the average performance is better, we're seeing spurious slowdowns leading to a large increase in failure rates

```
Job                     Failure rate    Avg run time    Avg queue time    Runner type      Runner labels                    Job runs
cargo test | windows    23.69%          9m 5s           11s               hosted-larger    github-windows-2025-x86_64-16    743
cargo test | windows    42.37%          6m 51s          1m 3s             self-hosted      depot-windows-2022-16            262
```

---

_Label `internal` added by @zanieb on 2025-06-26 16:14_

---

_Marked ready for review by @zanieb on 2025-06-26 16:15_

---

_Comment by @zanieb on 2025-06-26 20:35_

Hoping #14285 worked

---

_Closed by @zanieb on 2025-06-27 22:05_

---
