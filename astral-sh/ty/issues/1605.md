```yaml
number: 1605
title: Figure out why the inlay_hint tests needed to escape backslash
type: issue
state: closed
author: Gankra
labels:
  - internal
assignees: []
created_at: 2025-11-21T03:18:16Z
updated_at: 2025-11-21T17:48:03Z
url: https://github.com/astral-sh/ty/issues/1605
synced_at: 2026-01-10T01:58:59Z
```

# Figure out why the inlay_hint tests needed to escape backslash

---

_Issue opened by @Gankra on 2025-11-21 03:18_

The goto-definition tests don't have this, what gives.

https://github.com/astral-sh/ruff/blob/6b7adb0537d1a57f26cf462a637af483e0ba2c75/crates/ty_ide/src/inlay_hints.rs#L576

---

_Label `internal` added by @Gankra on 2025-11-21 03:18_

---

_Comment by @MichaReiser on 2025-11-21 06:52_

Go to def tests also do some backslash normalization in https://github.com/astral-sh/ruff/blob/6b7adb0537d1a57f26cf462a637af483e0ba2c75/crates/ty_ide/src/lib.rs#L481

---

_Closed by @Gankra on 2025-11-21 17:48_

---
