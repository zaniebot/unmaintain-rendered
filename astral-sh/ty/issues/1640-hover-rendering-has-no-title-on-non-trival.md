```yaml
number: 1640
title: Hover rendering has no title on non-trival stringified annotations
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2025-11-26T04:04:22Z
updated_at: 2025-12-05T13:47:24Z
url: https://github.com/astral-sh/ty/issues/1640
synced_at: 2026-01-12T15:54:25Z
```

# Hover rendering has no title on non-trival stringified annotations

---

_@MeGaGiGaGon_

### Summary

With this code:
https://play.ty.dev/fdd1d134-c407-4bee-bbc6-55418b97988e
```py
a: "str"
b: "str | int"
```
Hovering the `str` in `a` gives a title saying `str`:

<img width="473" height="237" alt="Image" src="https://github.com/user-attachments/assets/5efc35da-2396-41f2-85f9-a8a3ec58c1c9" />

While hovering the `str` in `b` is missing that title:

<img width="510" height="232" alt="Image" src="https://github.com/user-attachments/assets/1740e63e-0821-4ebb-a788-1e96db269e37" />

This happens both in the playground and in VSC.

### Version

ty 0.0.1-alpha.28 (8c342496a 2025-11-25) playground (536425619)

---

_Comment by @Gankra on 2025-11-26 04:39_

Yep, expected limitation in the current implementation, unfortunately. We need to augment the string-annotation tracking in inference to store the type of the sub-ast exprs. 

Micha proposed just allowing (Expr, Expr) as a key for looking up the type of an expression (the second Expr would belong to the sub-AST).

---

_Label `server` added by @Gankra on 2025-11-26 04:39_

---

_Assigned to @Gankra by @Gankra on 2025-11-26 04:39_

---

_Comment by @MichaReiser on 2025-11-26 10:15_

Another possible approach is to introduce a new scope for string literal expressions, though I'm not sure whether that would break anything.



---

_Added to milestone `Stable` by @Gankra on 2025-12-05 13:47_

---
