```yaml
number: 660
title: Too-many fix point iterations in growing union of literals
type: issue
state: closed
author: sharkdp
labels:
  - bug
  - fatal
  - type-inference
assignees: []
created_at: 2025-06-16T07:10:59Z
updated_at: 2025-08-28T14:46:38Z
url: https://github.com/astral-sh/ty/issues/660
synced_at: 2026-01-12T15:54:23Z
```

# Too-many fix point iterations in growing union of literals

---

_@sharkdp_

`ty` panics with "infer_expression_type(Id(1401)): execute: too many cycle iterations" for the following example:

```py
from typing import Self

class C:
  def __init__(self: Self):
    self.a = 0

  def b(self: Self):
    self.a = self.a + 1
```

When inferring the type of the `a` attribute, we take bindings in all methods into account, so we infer a growing union of literals `Unknown | Literal[0, 1, 2, 3, …]` here. This is not really related to attribute accesses though. The same thing would happen if we had full fixpoint iteration for loops (I believe):

```py
i = 0
while True:
    i += 1

    reveal_type(i)
```

This was orginally reported by @glyphack.

Comments by @carljm:

> The simplest approach here is to dramatically reduce the maximum size of literal unions we support 
> The infrastructure for this is all already in place, so it’s literally just changing a number
> The question is whether we want to be able to support larger explicit literal unions than we want to accumulate in fixpoint iteration
> If so, then that requires more additional infrastructure complication
> But I think we could set a literal-union size limit of anything under 200 (the salsa iteration limit) and make this too-many-iterations error go away
> (But we’d still iterate that many times, and hundreds of iterations might not be great for perf)
Another approach would be to add complication to the cycle handling function of some query in that cycle so that if it sees a growing union of literals, it falls back sooner
> And of course a third approach would just be to infer Unknown | int in the first place for undeclared = 1 instead of Unknown | Literal[1], and make the issue go away entirely


---

_Label `bug` added by @sharkdp on 2025-06-16 07:11_

---

_Label `type-inference` added by @sharkdp on 2025-06-16 07:11_

---

_Label `fatal` added by @AlexWaygood on 2025-06-16 07:18_

---

_Comment by @carljm on 2025-08-20 17:53_

I think #957 is the right way to address this.

---

_Assigned to @sharkdp by @sharkdp on 2025-08-22 13:59_

---

_Closed by @sharkdp on 2025-08-28 14:46_

---
