```yaml
number: 10039
title: "`blank-line-between-methods` (`E301`) & `blank-lines-top-level` (`E302`) - conflicts with formatter on `.pyi` files"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-02-19T07:16:08Z
updated_at: 2024-03-05T11:48:51Z
url: https://github.com/astral-sh/ruff/issues/10039
synced_at: 2026-01-12T15:54:49Z
```

# `blank-line-between-methods` (`E301`) & `blank-lines-top-level` (`E302`) - conflicts with formatter on `.pyi` files

---

_@DetachHead_

```py
# foo.pyi
class Foo: ...
class Bar: ... # error: Expected 2 blank lines, found 0
```

when running `ruff format` on this file, it removes the additional blank lines that `E302` expects

---

_Label `bug` added by @MichaReiser on 2024-02-19 07:16_

---

_Label `preview` added by @MichaReiser on 2024-02-19 07:16_

---

_Comment by @MichaReiser on 2024-02-19 07:17_

Nice find. We should review all the E3* rules and verify that they enforce at most one blank lines for pyi files rather than 2

---

_Renamed from "`blank-lines-top-level` (`E302`) - conflicts with formatter on `.pyi` files" to "`blank-line-between-methods` (`E301`) & `blank-lines-top-level` (`E302`) - conflicts with formatter on `.pyi` files" by @DetachHead on 2024-02-19 07:18_

---

_Comment by @KotlinIsland on 2024-02-20 05:55_

I've also detected E305 blank-lines-after-function-or-class also conflicts

---

_Label `help wanted` added by @MichaReiser on 2024-02-20 06:01_

---

_Closed by @MichaReiser on 2024-03-05 11:48_

---
