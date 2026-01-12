```yaml
number: 13335
title: Avoid enumerating sources in errors for path Python requests
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/source-file
created_at: 2025-05-07T18:10:04Z
updated_at: 2025-05-07T18:53:10Z
url: https://github.com/astral-sh/uv/pull/13335
synced_at: 2026-01-12T16:10:39Z
```

# Avoid enumerating sources in errors for path Python requests

---

_@zanieb_

e.g., these are misleading cruft in the error message at https://github.com/astral-sh/uv/pull/12168#discussion_r2078204601

```
❯ uv python find /foo/bar
error: No interpreter found for path `/foo/bar` in virtual environments, managed installations, or search path
❯ cargo run -q -- python find /foo/bar
error: No interpreter found at path `/foo/bar`
```

---

_Label `error messages` added by @zanieb on 2025-05-07 18:10_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_find.rs`:995 on 2025-05-07 18:19_

We should probably show absolute paths here? Can be separate though.

---

_@zanieb reviewed on 2025-05-07 18:19_

---

_@konstin approved on 2025-05-07 18:21_

Thanks!

---

_Comment by @zanieb on 2025-05-07 18:22_

I think this test snapshot is guaranteed to fail on Windows

---

_Marked ready for review by @zanieb on 2025-05-07 18:45_

---

_Merged by @zanieb on 2025-05-07 18:53_

---

_Closed by @zanieb on 2025-05-07 18:53_

---

_Branch deleted on 2025-05-07 18:53_

---
