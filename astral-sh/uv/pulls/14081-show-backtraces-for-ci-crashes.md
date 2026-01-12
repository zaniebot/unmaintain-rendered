```yaml
number: 14081
title: Show backtraces for CI crashes
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/show-backtraces-for-crashes
created_at: 2025-06-16T16:34:18Z
updated_at: 2025-06-17T08:44:03Z
url: https://github.com/astral-sh/uv/pull/14081
synced_at: 2026-01-12T16:11:01Z
```

# Show backtraces for CI crashes

---

_@konstin_

I only just realized that we can get backtraces for crashes with `RUST_BACKTRACE: 1`, even non-panics (https://github.com/astral-sh/uv/pull/14079). This is much better than trying to analyze crash dumps.

---

_Review requested from @zanieb by @konstin on 2025-06-16 16:34_

---

_Label `internal` added by @konstin on 2025-06-16 16:34_

---

_@zanieb approved on 2025-06-16 16:44_

---

_Merged by @konstin on 2025-06-16 17:10_

---

_Closed by @konstin on 2025-06-16 17:10_

---

_Branch deleted on 2025-06-16 17:10_

---

_Comment by @zanieb on 2025-06-17 01:21_

It turns out this shows a backtrace for every snapshot failure, which is pretty noisy. I'm guessing there's nothing to be done about that?

---

_Comment by @konstin on 2025-06-17 08:44_

I'm not aware of any, insta uses panics to signal failure.

---
