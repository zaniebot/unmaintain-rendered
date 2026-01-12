```yaml
number: 14349
title: Consistently normalize trailing slashes on URLs with no path segments
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/trailing-slash-pop
created_at: 2025-06-29T15:53:25Z
updated_at: 2025-06-29T17:25:12Z
url: https://github.com/astral-sh/uv/pull/14349
synced_at: 2026-01-12T16:11:10Z
```

# Consistently normalize trailing slashes on URLs with no path segments

---

_@zanieb_

Alternative to https://github.com/astral-sh/uv/pull/14348

---

_@charliermarsh reviewed on 2025-06-29 15:57_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/file.rs`:186 on 2025-06-29 15:57_

Can the `then_some` go on the outside? Like on the same level as `.and_then`? (Not sure)

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/file.rs`:185 on 2025-06-29 15:58_

If it doesn't contain `://`, should we return `None` rather than 0? (Again, didn't think about it super hard.)

---

_@charliermarsh reviewed on 2025-06-29 15:58_

---

_@charliermarsh approved on 2025-06-29 15:58_

---

_@zanieb reviewed on 2025-06-29 16:01_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/file.rs`:185 on 2025-06-29 16:01_

We probably shouldn't add 2, but it seems less surprising to still trim the slash here if there's no scheme

---

_@zanieb reviewed on 2025-06-29 16:05_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/file.rs`:186 on 2025-06-29 16:05_

If I use `filter` and `map`, yeah.

---

_@zanieb reviewed on 2025-06-29 16:11_

---

_Review comment by @zanieb on `crates/uv-distribution-types/src/file.rs`:185 on 2025-06-29 16:11_

I futzed with this a bit and decided to use `split_once` because it's clearer than the find/rfind

---

_Marked ready for review by @zanieb on 2025-06-29 16:13_

---

_Comment by @charliermarsh on 2025-06-29 17:13_

Nice, this looks good!

---

_Renamed from "Update `UrlString::without_trailing_slash` to match `Url::pop_if_empty` behavior" to "Consistently normalize trailing slashes on URLs with no path segments" by @zanieb on 2025-06-29 17:23_

---

_Merged by @zanieb on 2025-06-29 17:25_

---

_Closed by @zanieb on 2025-06-29 17:25_

---

_Branch deleted on 2025-06-29 17:25_

---
