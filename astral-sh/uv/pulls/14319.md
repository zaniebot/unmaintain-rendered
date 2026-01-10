```yaml
number: 14319
title: "Return `Cow` from `UrlString::with_` methods"
type: pull_request
state: merged
author: jtfmumm
labels:
  - internal
assignees: []
merged: true
base: main
head: jtfm/with_cows
created_at: 2025-06-27T16:01:54Z
updated_at: 2025-06-27T17:54:54Z
url: https://github.com/astral-sh/uv/pull/14319
synced_at: 2026-01-10T06:53:01Z
```

# Return `Cow` from `UrlString::with_` methods

---

_Pull request opened by @jtfmumm on 2025-06-27 16:01_

A minor performance improvement as a follow-up to #14245 (and an accompanying test).


---

_Label `internal` added by @jtfmumm on 2025-06-27 16:01_

---

_@jtfmumm reviewed on 2025-06-27 16:02_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:272 on 2025-06-27 16:02_

These `base_str()` checks were repeated from another test and shouldn't have been here

---

_Review requested from @charliermarsh by @jtfmumm on 2025-06-27 16:02_

---

_@oconnor663 reviewed on 2025-06-27 16:19_

---

_Review comment by @oconnor663 on `crates/uv-distribution-types/src/file.rs`:170 on 2025-06-27 16:19_

If these functions always return `Cow::Borrowed`, why not make the return type `&str`?

---

_@jtfmumm reviewed on 2025-06-27 16:44_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:170 on 2025-06-27 16:44_

That’s just the unwrap case. They don’t when a transformation is applied. 

---

_@charliermarsh reviewed on 2025-06-27 16:56_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/file.rs`:208 on 2025-06-27 16:56_

I would just inline this, personally, rather than creating a function. It also makes the owned case clearer (related to Jack's question).

---

_@jtfmumm reviewed on 2025-06-27 17:21_

---

_Review comment by @jtfmumm on `crates/uv-distribution-types/src/file.rs`:208 on 2025-06-27 17:21_

sure

---

_@charliermarsh approved on 2025-06-27 17:54_

---

_Merged by @charliermarsh on 2025-06-27 17:54_

---

_Closed by @charliermarsh on 2025-06-27 17:54_

---

_Branch deleted on 2025-06-27 17:54_

---
