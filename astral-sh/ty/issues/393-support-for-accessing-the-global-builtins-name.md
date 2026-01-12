```yaml
number: 393
title: "support for accessing the global `__builtins__` name"
type: issue
state: closed
author: jorenham
labels:
  - help wanted
  - runtime semantics
assignees: []
created_at: 2025-05-14T18:18:55Z
updated_at: 2025-05-15T20:01:40Z
url: https://github.com/astral-sh/ty/issues/393
synced_at: 2026-01-12T15:54:23Z
```

# support for accessing the global `__builtins__` name

---

_@jorenham_

Attempting to do so will currently be reported as

```
error[unresolved-reference]: Name `__builtins__` used when not defined
```

https://play.ty.dev/758905b4-43dd-4307-84a4-47222adb086e


---

_Label `runtime semantics` added by @AlexWaygood on 2025-05-14 18:28_

---

_Label `runtime semantics` removed by @carljm on 2025-05-14 18:29_

---

_Label `help wanted` added by @carljm on 2025-05-14 18:29_

---

_Comment by @carljm on 2025-05-14 18:29_

Thanks for the report! This should be pretty easy to implement as a special case in builtin-names lookup. Pyright infers simply `Any` as the type of `__builtins__`, which is super easy to implement and at least won't cause any false positives. Mypy infers the actual builtins module, which is more precise, and I think should also be quite easy for us to do.

---

_Label `runtime semantics` added by @carljm on 2025-05-14 18:29_

---

_Comment by @felixscherz on 2025-05-15 10:55_

Hi, I had a shot at implementing this with https://github.com/astral-sh/ruff/pull/18118, let me know what you think:)

---

_Closed by @sharkdp on 2025-05-15 20:01_

---
