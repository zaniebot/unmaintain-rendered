```yaml
number: 15558
title: Windows keyring access can fail with error code 6
type: issue
state: closed
author: zanieb
labels:
  - ci-flake
assignees: []
created_at: 2025-08-27T18:19:10Z
updated_at: 2025-08-27T21:42:38Z
url: https://github.com/astral-sh/uv/issues/15558
synced_at: 2026-01-12T16:02:12Z
```

# Windows keyring access can fail with error code 6

---

_@zanieb_

```
    ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
    Snapshot: logout_native_keyring-3
    Source: V:\uv:539
    ───────────────────────────────────────────────────────────────────────────────
    Expression: snapshot
    ───────────────────────────────────────────────────────────────────────────────
    -old snapshot
    +new results
    ────────────┬──────────────────────────────────────────────────────────────────
        3     3 │ 
        4     4 │ ----- stderr -----
        5     5 │ warning: The native keyring provider is experimental and may change without warning. Pass `--preview-features native-keyring` to disable this warning.
        6     6 │ error: Unable to remove credentials for https://pypi-proxy.fly.dev/basic-auth/simple
        7       │-  Caused by: No matching entry found in secure storage
              7 │+  Caused by: Platform secure storage failure: Windows error code 6
              8 │+  Caused by: Windows error code 6
```

---

_Label `ci-flake` added by @zanieb on 2025-08-27 18:19_

---

_Assigned to @zanieb by @zanieb on 2025-08-27 18:19_

---

_Closed by @zanieb on 2025-08-27 21:42_

---
