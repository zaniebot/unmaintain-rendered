```yaml
number: 2548
title: "Mdtest framework panic on a malformed `# error` assertion"
type: issue
state: open
author: AlexWaygood
labels:
  - bug
  - help wanted
  - testing
assignees: []
created_at: 2026-01-17T15:05:53Z
updated_at: 2026-01-17T15:06:44Z
url: https://github.com/astral-sh/ty/issues/2548
synced_at: 2026-01-17T16:15:01Z
```

# Mdtest framework panic on a malformed `# error` assertion

---

_@AlexWaygood_

### Summary

If you put this assertion in an mdtest, it causes a panic in the `ty_test` crate:

```py
C = NewType("C", list[T])  # error: [invalid-newtype]"
```

```
thread '<unnamed>' (10880726) panicked at crates/ty_test/src/assertion.rs:424:29:
begin <= end (18 <= 17) when slicing `[invalid-newtype]"`
```

---

_Label `bug` added by @AlexWaygood on 2026-01-17 15:05_

---

_Label `help wanted` added by @AlexWaygood on 2026-01-17 15:05_

---

_Label `testing` added by @AlexWaygood on 2026-01-17 15:05_

---
