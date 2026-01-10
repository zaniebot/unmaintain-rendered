```yaml
number: 20700
title: "[RUF051] fix silently drops `else` branch"
type: issue
state: closed
author: dvolodin7
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-05T08:41:02Z
updated_at: 2025-10-06T13:02:29Z
url: https://github.com/astral-sh/ruff/issues/20700
synced_at: 2026-01-10T11:09:59Z
```

# [RUF051] fix silently drops `else` branch

---

_Issue opened by @dvolodin7 on 2025-10-05 08:41_

### Summary

Having code snippet:

``` python
    not_found: Set[Dict[str, Any]] = set()
    for ctx in e.query(query, **args):
        ctx_hash = dict_hash_int(ctx)
        if ctx_hash in left:
            del left[ctx_hash]
        else:
            not_found.add(ctx)
```

Ruff detects `if-key-in-dict-del` problem and offers fix:

``` diff
     not_found: Set[Dict[str, Any]] = set()
     for ctx in e.query(query, **args):
         ctx_hash = dict_hash_int(ctx)
-        if ctx_hash in left:
-            del left[ctx_hash]
-        else:
-            not_found.add(ctx)
+        left.pop(ctx_hash, None)
```

Obviously, it will broke the code, as the `else` chain will be lost. So in the presence of `else` clause either:
1. there is no problem in code
2. fix must be rewritten like this

``` python
if left.pop(ctx_hash, GUARD) is GUARD:
    ... # former else clause
```

### Version

0.13.3

---

_Label `bug` added by @dylwil3 on 2025-10-05 11:50_

---

_Label `rule` added by @dylwil3 on 2025-10-05 11:50_

---

_Label `fixes` added by @dylwil3 on 2025-10-05 11:50_

---

_Label `rule` removed by @MichaReiser on 2025-10-06 06:54_

---

_Closed by @dylwil3 on 2025-10-06 13:02_

---
