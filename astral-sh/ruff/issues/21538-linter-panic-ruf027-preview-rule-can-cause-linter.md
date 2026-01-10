```yaml
number: 21538
title: "[Linter panic] RUF027 preview rule can cause linter panic on specific string"
type: issue
state: open
author: skraeven
labels:
  - bug
  - parser
assignees: []
created_at: 2025-11-20T14:01:13Z
updated_at: 2025-12-10T14:20:30Z
url: https://github.com/astral-sh/ruff/issues/21538
synced_at: 2026-01-10T11:10:00Z
```

# [Linter panic] RUF027 preview rule can cause linter panic on specific string

---

_Issue opened by @skraeven on 2025-11-20 14:01_

Ruff has crashed while running on our code base, due to a specific string/rule combination.

Given this (minimal repro version) string in a python file:
```
foo = "{'bar': {\t'"
```

Running `ruff check . --preview --select=RUF027 --isolated` will cause linter panic.

Playground: https://play.ruff.rs/486e305d-99f7-4dcb-88fb-60d8fe45f002

Here's `RUST_BACKTRACE=1` output:

``` 
info: Backtrace:
   0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
  27: start_thread
             at ./nptl/pthread_create.c:447:8
  28: clone3
             at ./misc/../sysdeps/unix/sysv/linux/x86_64/clone3.S:78:0

```

---

_Label `bug` added by @ntBre on 2025-11-20 14:07_

---

_Comment by @MichaReiser on 2025-11-20 14:08_

Thanks. The playground has a more useful stacktrace and it points to:  `crates/ruff_python_parser/src/parser/expression.rs:1619:25` with the message `internal error: entered unreachable code: f-string: unexpected token `TStringMiddle` at 14..15` (Sorry @dylwil3) 

---

_Label `parser` added by @MichaReiser on 2025-11-20 14:08_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-11-21 03:10_

---

_Comment by @MichaReiser on 2025-12-10 13:55_

This is actually valid Python syntax :) 

---

_Comment by @dylwil3 on 2025-12-10 14:10_

Yes but the panic is caused not from parsing the expression in the OP, but from calling `parse_expression` on 

```python
f"{'bar': {\t'"
```

Recall that RUF027 checks to see whether turning a string into an f-string would be correct syntax before suggesting it. Unfortunately this has lead to errors in the past, mainly because our error recovery is not nearly as battle-tested for `parse_expression` as it is for parsing a module.

---

_Comment by @MichaReiser on 2025-12-10 14:20_

Ah right. Too bad, my other change also doesn't fix that

---
