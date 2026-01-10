```yaml
number: 3143
title: For pre-commits, should ruff run before or after black?
type: issue
state: closed
author: bellini666
labels: []
assignees: []
created_at: 2023-02-22T20:43:12Z
updated_at: 2023-02-23T18:34:34Z
url: https://github.com/astral-sh/ruff/issues/3143
synced_at: 2026-01-10T11:09:46Z
```

# For pre-commits, should ruff run before or after black?

---

_Issue opened by @bellini666 on 2023-02-22 20:43_

The documentation doesn't specify if ruff should run before or after black for best results.

---

_Renamed from "For pre-commits, should ruff run before or after black" to "For pre-commits, should ruff run before or after black?" by @bellini666 on 2023-02-22 20:43_

---

_Comment by @charliermarsh on 2023-02-22 20:51_

Before! I can add a note.

---

_Comment by @charliermarsh on 2023-02-22 21:53_

(We test in CI that running Ruff, then Black, then Ruff again is a stable operation.)

---

_Closed by @charliermarsh on 2023-02-22 22:22_

---

_Comment by @onerandomusername on 2023-02-23 01:36_

Out of curiosity, why does the order matter? I run black before I run ruff because black might fix some things that a linter would otherwise complain about, so it keeps actual errors to a minimum.

---

_Comment by @charliermarsh on 2023-02-23 01:48_

Ruff's autofixes aren't guaranteed to output Black-compatible code. They often do, but it's not guaranteed. So running Black, then Ruff, then Black could result in changes on each invocation (whereas running Ruff, then Black should always be stable). (By "Black-compatible", I mean code that Black would _not_ reformat.)

As a silly example, imagine we're converting a lambda to a function definition. When we do that, we don't take line lengths into account, and we certainly don't take all of Black's possible line length wrapping behaviors into account. So if you had a lambda with a ton of arguments, we might yield a function definition on one very long line, which Black would then wrap.


---

_Comment by @onerandomusername on 2023-02-23 18:33_

Ah, so it only needs to run first *when autofix is enabled*. That makes more sense.

---

_Comment by @charliermarsh on 2023-02-23 18:34_

Correct!

---
