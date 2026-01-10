```yaml
number: 10651
title: Improve performance of our test drive in Windows CI
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/dev-filters
created_at: 2025-01-15T21:29:27Z
updated_at: 2025-01-16T18:07:13Z
url: https://github.com/astral-sh/uv/pull/10651
synced_at: 2026-01-10T11:45:02Z
```

# Improve performance of our test drive in Windows CI

---

_Pull request opened by @zanieb on 2025-01-15 21:29_

Previously, we couldn't use a DevDrive (https://github.com/astral-sh/uv/pull/3522#issuecomment-2111448930) because our Windows version was not sufficient.

Recently, I upgraded our larger runners to Windows 2025 preview (https://github.com/astral-sh/uv/pull/10298) which I presume has support for this.

I removed ReFS in https://github.com/astral-sh/uv/pull/10651/commits/953c3535c3754f580ecd8da55637756f58606567 which didn't seem to do anything to performance.

I also found some notes on "trusted" DevDrives and "disabling anti-virus filtering" which I simply have to try.

---

_Label `testing` added by @zanieb on 2025-01-15 21:29_

---

_Label `internal` added by @zanieb on 2025-01-15 21:29_

---

_Renamed from "Disable all file-system filtering on the CI Dev Drive" to "Improve performance of our test drive in Windows CI" by @zanieb on 2025-01-15 22:10_

---

_Comment by @zanieb on 2025-01-15 22:33_

Sadly I see no change, even though these are taking effect correctly (from the debug log).

---

_Comment by @zanieb on 2025-01-16 17:40_

Maybe this has a minor performance improvement. It's more visible in #10662

I think this might get us more consistency / seems fine to merge regardless of a lack of clear improvement.

---

_Marked ready for review by @zanieb on 2025-01-16 17:41_

---

_Review requested from @samypr100 by @zanieb on 2025-01-16 17:41_

---

_@samypr100 approved on 2025-01-16 18:01_

---

_Merged by @zanieb on 2025-01-16 18:07_

---

_Closed by @zanieb on 2025-01-16 18:07_

---

_Branch deleted on 2025-01-16 18:07_

---
