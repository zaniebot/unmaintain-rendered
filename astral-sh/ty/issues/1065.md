```yaml
number: 1065
title: "[ty] Divergent fixpoint iteration in instance attribute inference"
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
assignees: []
created_at: 2025-08-20T16:28:12Z
updated_at: 2025-08-20T18:01:10Z
url: https://github.com/astral-sh/ty/issues/1065
synced_at: 2026-01-10T02:06:24Z
```

# [ty] Divergent fixpoint iteration in instance attribute inference

---

_Issue opened by @sharkdp on 2025-08-20 16:28_

Another example where divergent fixpoint iteration leads to a panic:
```py
class Counter:
    def __init__(self: "Counter"):
        self.x = 0

    def inc(self: "Counter"):
        self.x = self.x + 1
```

I wonder why the union of literals -> `int` mechanism doesn't trigger here?

Note: all of these panics appear with implicit instance attributes, but it's not at all related to attribute access. If we had cyclic control flow, this could certainly be demonstrated with something like:
```py
x = 0
while True:
    x += 1
```

---

_Label `bug` added by @sharkdp on 2025-08-20 16:28_

---

_Label `fatal` added by @sharkdp on 2025-08-20 16:28_

---

_Renamed from "[ty] Too many cycle iterations for cyclically dependent instance attributes" to "[ty] Divergent fixpoint iteration in instance attribute inference" by @sharkdp on 2025-08-20 16:32_

---

_Comment by @carljm on 2025-08-20 17:53_

I think this is a duplicate of #660, and there's some discussion there of possible solutions. Although I think the best solution is #957, which isn't mentioned there yet.

---

_Closed by @carljm on 2025-08-20 17:53_

---

_Comment by @sharkdp on 2025-08-20 18:01_

🙈

I started with something else here and eventually minified it to something I had already reported. Totally forgot that this was still open. Thanks.

---
