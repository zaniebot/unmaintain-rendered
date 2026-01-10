```yaml
number: 1627
title: "`invalid-syntax-in-forward-annotation` does not preserve error spans"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - diagnostics
assignees: []
created_at: 2025-11-24T23:59:53Z
updated_at: 2025-11-25T12:17:49Z
url: https://github.com/astral-sh/ty/issues/1627
synced_at: 2026-01-10T01:58:59Z
```

# `invalid-syntax-in-forward-annotation` does not preserve error spans

---

_Issue opened by @MeGaGiGaGon on 2025-11-24 23:59_

### Summary

As the title says, `invalid-syntax-in-forward-annotation` does not preserve error spans. For example:
https://play.ty.dev/f034109f-c63d-4871-9976-dbea46491a62
```py
a: type]
b: "type]"
```
The span of `a`'s error is `1:8-1:9` (well actually it has two errors, but both are on the `]`).
The span of `b`'s error is the entire string literal. It would be nice if `ty` preserved the `invalid-syntax` error location and propagated it to `invalid-syntax-in-forward-annotation`
```powershell
PS ~>Set-Content issue.py @'
a: type]
b: "type]"
'@
PS ~>uvx ty check issue.py
error[invalid-syntax]: Expected a statement
 --> issue.py:1:8
  |
1 | a: type]
  |        ^
2 | b: "type]"
  |

error[invalid-syntax]: Expected a statement
 --> issue.py:1:9
  |
1 | a: type]
  |         ^
2 | b: "type]"
  |

error[invalid-syntax-in-forward-annotation]: Syntax error in forward annotation: Unexpected token at the end of an expression
 --> issue.py:2:4
  |
1 | a: type]
2 | b: "type]"
  |    ^^^^^^^
  |
info: rule `invalid-syntax-in-forward-annotation` is enabled by default

Found 3 diagnostics
```

The error message is also not always helpful, since the error may not occur at the end, ie `type] | int`

### Version

ty 0.0.1-alpha.27 (26d7b6864 2025-11-18)

---

_Label `diagnostics` added by @carljm on 2025-11-25 01:14_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-25 07:39_

---

_Removed from milestone `Stable` by @MichaReiser on 2025-11-25 07:39_

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-25 07:39_

---

_Comment by @MichaReiser on 2025-11-25 07:39_

I think this should go into stable as it can lead to a panic when rendering the diagnostics if the offset falls out of range or between a char boundary

---

_Label `bug` added by @MichaReiser on 2025-11-25 07:40_

---

_Label `bug` removed by @MichaReiser on 2025-11-25 12:17_

---

_Comment by @MichaReiser on 2025-11-25 12:17_

Okay, I think I misunderstood this one but I suspect this should be easier with @Gankra's latest changes in https://github.dev/astral-sh/ruff/pull/21577

---
