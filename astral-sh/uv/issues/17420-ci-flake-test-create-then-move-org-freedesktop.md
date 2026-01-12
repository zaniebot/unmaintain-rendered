```yaml
number: 17420
title: "CI flake test_create_then_move: org.freedesktop.Secret.Error.IsLocked"
type: issue
state: open
author: konstin
labels:
  - ci-flake
assignees: []
created_at: 2026-01-12T18:39:08Z
updated_at: 2026-01-12T18:39:08Z
url: https://github.com/astral-sh/uv/issues/17420
synced_at: 2026-01-12T19:14:14Z
```

# CI flake test_create_then_move: org.freedesktop.Secret.Error.IsLocked

---

_@konstin_

https://github.com/astral-sh/uv/actions/runs/20905972019/job/60059487575?pr=17336

```
thread 'test_create_then_move' (133015) panicked at crates/uv-keyring/tests/threading.rs:52:18:
Task failed: JoinError::Panic(Id(1), "Can't set initial ascii password: PlatformFailure(Zbus(MethodError(OwnedErrorName(\"org.freedesktop.Secret.Error.IsLocked\"), Some(\"Cannot create an item in a locked collection\"), Msg { type: Error, serial: 427, sender: UniqueName(\":1.20\"), reply-serial: 18, body: Str, fds: [] })))", ...)
```

---

_Label `ci-flake` added by @konstin on 2026-01-12 18:39_

---
