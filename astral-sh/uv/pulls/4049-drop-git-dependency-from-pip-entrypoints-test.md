```yaml
number: 4049
title: "Drop `git` dependency from `pip_entrypoints` test"
type: pull_request
state: merged
author: zanieb
labels:
  - testing
assignees: []
merged: true
base: main
head: zb/git-pip-entry
created_at: 2024-06-05T14:37:17Z
updated_at: 2024-06-05T15:22:33Z
url: https://github.com/astral-sh/uv/pull/4049
synced_at: 2026-01-12T16:06:00Z
```

# Drop `git` dependency from `pip_entrypoints` test

---

_@zanieb_

There's a release with the changes we want test coverage for now

---

_Label `testing` added by @zanieb on 2024-06-05 14:37_

---

_@zanieb reviewed on 2024-06-05 14:39_

---

_Review comment by @zanieb on `crates/uv/tests/pip_sync.rs`:3041 on 2024-06-05 14:39_

@konstin  Is the intent here to have the second requirement be `pip>0.24.0` to test the latest version or what?

---

_Review comment by @konstin on `crates/uv/tests/pip_sync.rs`:3041 on 2024-06-05 14:46_

The entrypoint behavior changed in https://github.com/pypa/pip/pull/12536 and we need to test both behaviors. Any pip release that includes https://github.com/pypa/pip/commit/a643cc8970bd42e5559661703ea0a5b0bca68c5e will do.

---

_@konstin approved on 2024-06-05 14:46_

---

_Comment by @zanieb on 2024-06-05 14:53_

@konstin I tweaked the comments a bit. Still a bit confused lmk if it makes sense to you.

---

_Comment by @konstin on 2024-06-05 15:22_

Yep, thanks

---

_Merged by @konstin on 2024-06-05 15:22_

---

_Closed by @konstin on 2024-06-05 15:22_

---

_Branch deleted on 2024-06-05 15:22_

---
