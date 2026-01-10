```yaml
number: 14182
title: "Refactor `find_uv_bin` and add a better error message"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/ref-find
created_at: 2025-06-21T11:58:55Z
updated_at: 2025-08-08T13:28:03Z
url: https://github.com/astral-sh/uv/pull/14182
synced_at: 2026-01-10T06:44:33Z
```

# Refactor `find_uv_bin` and add a better error message

---

_Pull request opened by @zanieb on 2025-06-21 11:58_

Follows https://github.com/astral-sh/uv/pull/14181

Two goals here

- Remove duplicated logic and make the search order clear
- Resolve user confusion around the searched directories; we previously only displayed the last attempt, which we rarely expect to be relevant

---

_Label `enhancement` added by @zanieb on 2025-06-21 11:58_

---

_Review requested from @konstin by @zanieb on 2025-06-27 17:46_

---

_Assigned to @konstin by @zanieb on 2025-06-27 17:46_

---

_Marked ready for review by @zanieb on 2025-06-27 17:46_

---

_@konstin reviewed on 2025-06-30 13:16_

I'm reviewing the combined changes in #14184

---

_@zanieb reviewed on 2025-08-07 19:59_

---

_Review comment by @zanieb on `crates/uv/tests/it/common/mod.rs`:624 on 2025-08-07 19:59_

This is new, the rest is just moved up to ensure it comes first in the filter order.

---

_@woodruffw approved on 2025-08-07 20:07_

---

_Merged by @zanieb on 2025-08-07 20:10_

---

_Closed by @zanieb on 2025-08-07 20:10_

---

_Branch deleted on 2025-08-07 20:10_

---

_Review comment by @konstin on `python/uv/_find_uv.py`:38 on 2025-08-08 12:48_

Why are we using both `os.linesep` and `\n` in the same message?

---

_@konstin reviewed on 2025-08-08 12:48_

---

_@zanieb reviewed on 2025-08-08 12:52_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:38 on 2025-08-08 12:52_

We shouldn't be, thanks!

---

_@zanieb reviewed on 2025-08-08 12:52_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:38 on 2025-08-08 12:52_

Confused this isn't rendered in the snapshots?

---

_@zanieb reviewed on 2025-08-08 12:53_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:38 on 2025-08-08 12:53_

Oh, that's just a trailing newline. This was just easier than writing a literal `\n` inside the f-string? Do you want me to use linesep everywhere?

---

_@konstin reviewed on 2025-08-08 13:28_

---

_Review comment by @konstin on `python/uv/_find_uv.py`:38 on 2025-08-08 13:28_

I would just use `\n` everywhere.

---
