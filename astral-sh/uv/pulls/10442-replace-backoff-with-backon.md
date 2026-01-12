```yaml
number: 10442
title: replace backoff with backon
type: pull_request
state: merged
author: Gankra
labels:
  - internal
assignees: []
merged: true
base: main
head: gankra/backon
created_at: 2025-01-09T20:09:25Z
updated_at: 2025-01-09T21:01:25Z
url: https://github.com/astral-sh/uv/pull/10442
synced_at: 2026-01-12T16:09:17Z
```

# replace backoff with backon

---

_@Gankra_

This should be essentially the exact same behaviour, but backon is a total API redesign, so things had to be expressed slightly differently. Overall I think the code is more readable, which is nice.

Fixes #10001 

---

_Label `internal` added by @Gankra on 2025-01-09 20:09_

---

_@Gankra reviewed on 2025-01-09 20:14_

---

_Review comment by @Gankra on `crates/uv-fs/src/lib.rs`:223 on 2025-01-09 20:14_

I think these sleep calls are technically defaulted but their examples use them and once they were there I felt they improved readability.

---

_@Gankra reviewed on 2025-01-09 20:15_

---

_Review comment by @Gankra on `crates/uv-fs/src/lib.rs`:342 on 2025-01-09 20:15_

This extra if-let from forgetting the analysis we did to decide to retry is the biggest "flaw" with backon's different API. small potatoes.

---

_@charliermarsh reviewed on 2025-01-09 20:23_

---

_Review comment by @charliermarsh on `crates/uv-fs/src/lib.rs`:192 on 2025-01-09 20:23_

I feel so dumb, can you explain this math? (I assume the intention is to be unchanged.)

---

_@charliermarsh approved on 2025-01-09 20:24_

---

_@Gankra reviewed on 2025-01-09 20:29_

---

_Review comment by @Gankra on `crates/uv-fs/src/lib.rs`:192 on 2025-01-09 20:29_

we start at 10 milliseconds and try 9 times. the longest try is therefore `10*pow(2, 9)` milliseconds, which is 5 seconds. This gives us about 10 seconds of attempting to retry, because the sum of powers of 2 is just the next power of 2 (-1).

Or more intuitively, we wait about 5 seconds in the worst case, and that dwarfs everything else combined.

---

_@Gankra reviewed on 2025-01-09 20:38_

---

_Review comment by @Gankra on `crates/uv-fs/src/lib.rs`:192 on 2025-01-09 20:38_

Updated the comment, thanks for calling it out!

---

_Merged by @Gankra on 2025-01-09 21:01_

---

_Closed by @Gankra on 2025-01-09 21:01_

---

_Branch deleted on 2025-01-09 21:01_

---
