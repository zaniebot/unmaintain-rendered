```yaml
number: 11336
title: "Fix credential caching for index roots when URL ends in `simple/`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/fix-index
created_at: 2025-02-08T01:57:35Z
updated_at: 2025-02-08T15:23:33Z
url: https://github.com/astral-sh/uv/pull/11336
synced_at: 2026-01-12T16:09:48Z
```

# Fix credential caching for index roots when URL ends in `simple/`

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/11244

See test failure at https://github.com/astral-sh/uv/pull/11336/commits/e12f98a3e40ddb61edd2fd32be4bfcf01c160102

---

_Label `bug` added by @zanieb on 2025-02-08 01:59_

---

_@zanieb reviewed on 2025-02-08 02:05_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:171 on 2025-02-08 02:05_

I started implementing this to skip all empty trailing segments, but I don't actually know if that's correct? `https:://example.com/simple////` would be awfully weird anyway.

---

_Review requested from @charliermarsh by @zanieb on 2025-02-08 02:10_

---

_@charliermarsh reviewed on 2025-02-08 02:16_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:164 on 2025-02-08 02:16_

I think there's a `pop_if_empty` that you might be able to use here?

---

_@charliermarsh reviewed on 2025-02-08 02:18_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/index.rs`:164 on 2025-02-08 02:18_

(Honestly, I didn't read the diff closely enough to know if it helps for sure, but it kind of looked like it from a glance.)

---

_@zanieb reviewed on 2025-02-08 02:19_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:164 on 2025-02-08 02:19_

Not in the immutable version, but yes I can use it later. One sec...

---

_@zanieb reviewed on 2025-02-08 02:25_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/index.rs`:164 on 2025-02-08 02:25_

It's a bit cleaner, thanks.

---

_@charliermarsh approved on 2025-02-08 13:43_

---

_Merged by @zanieb on 2025-02-08 15:23_

---

_Closed by @zanieb on 2025-02-08 15:23_

---

_Branch deleted on 2025-02-08 15:23_

---
