```yaml
number: 1022
title: no narrowing after sys.exit
type: issue
state: closed
author: andythomas
labels:
  - bug
  - narrowing
  - control flow
assignees: []
created_at: 2025-08-16T16:53:55Z
updated_at: 2025-08-18T08:27:34Z
url: https://github.com/astral-sh/ty/issues/1022
synced_at: 2026-01-10T02:06:24Z
```

# no narrowing after sys.exit

---

_Issue opened by @andythomas on 2025-08-16 16:53_

### Summary

If I look at the following minimal example

```
import sys

def test(m: int | None):

    if m is None:
        sys.exit(1)


    m
```

I would expect m to be `int` 

<img width="294" height="238" alt="Image" src="https://github.com/user-attachments/assets/eeb149f6-f2dc-4b47-821f-ab302b0d87fe" />

I found #454, which is labelled 'closed duplicate of #180, and #685, but this one looks different (to me).

### Version

ty 0.0.1-alpha.18 (d697cc092 2025-08-14)

---

_Label `narrowing` added by @AlexWaygood on 2025-08-16 17:05_

---

_Label `control flow` added by @AlexWaygood on 2025-08-16 17:05_

---

_Label `bug` added by @AlexWaygood on 2025-08-16 17:05_

---

_Comment by @sharkdp on 2025-08-18 08:27_

Thank you for reporting this.

#685 looks different, but really has the same underlying cause. We do not preserve narrowing constraints in (all) control flow joins. A fix for this is being discussed in #690, so I think we can close this for now.

---

_Closed by @sharkdp on 2025-08-18 08:27_

---
