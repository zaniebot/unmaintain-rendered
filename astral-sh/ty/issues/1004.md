```yaml
number: 1004
title: "goto-definition on a stub definition doesn't work"
type: issue
state: closed
author: Gankra
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-15T13:50:01Z
updated_at: 2025-08-19T01:42:55Z
url: https://github.com/astral-sh/ty/issues/1004
synced_at: 2026-01-10T02:06:24Z
```

# goto-definition on a stub definition doesn't work

---

_Issue opened by @Gankra on 2025-08-15 13:50_

example (in project that depends on colorama and types-colorama): 

```
from colorama.ansi import AnsiCursor
                           ^^^^^^^^
```

goto-declaration will jump to the definition in ansi.pyi and, while goto-definition will jump to the definition in ansi.py, good!

However if you goto-definition on the definition in ansi.pyi, it won't find the definition in ansi.py!

I believe this is part of the same issue as described in this comment, so the same fix may fix both: https://github.com/astral-sh/ty/issues/1001#issuecomment-3191481291



---

_Added to milestone `Beta` by @Gankra on 2025-08-15 13:50_

---

_Label `bug` added by @Gankra on 2025-08-15 13:50_

---

_Label `server` added by @Gankra on 2025-08-15 13:50_

---

_Comment by @AlexWaygood on 2025-08-15 13:56_

Interesting -- what do pyright/pylance do here? It would be cool if we could get this working, but I also don't find our current behaviour _very_ surprising. From the "perspective of the stub file", the `AnsiCursor` definition in the stub file _is_ the canonical definition as well as the canonical declaration, right? Though this is obviously not true from the perspective of `.py` files that _use_ the stub files.

---

_Comment by @Gankra on 2025-08-15 14:06_

Pylance does indeed implement the behaviour described.

---

_Assigned to @Gankra by @Gankra on 2025-08-15 15:34_

---

_Closed by @Gankra on 2025-08-19 01:42_

---
