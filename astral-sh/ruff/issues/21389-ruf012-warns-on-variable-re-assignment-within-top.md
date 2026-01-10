---
number: 21389
title: RUF012 warns on variable re-assignment within top level of class body
type: issue
state: closed
author: anabelle2001
labels:
  - rule
assignees: []
created_at: 2025-11-11T18:40:21Z
updated_at: 2025-11-17T20:52:26Z
url: https://github.com/astral-sh/ruff/issues/21389
synced_at: 2026-01-10T01:23:02Z
---

# RUF012 warns on variable re-assignment within top level of class body

---

_Issue opened by @anabelle2001 on 2025-11-11 18:40_

### Summary

The second assignment reports that ROI_COLORS is not annotated with ClassVar. I think this is a false positive.

View in Playground: <https://play.ruff.rs/d208fa8c-4554-415c-86cd-9986c0a0d76f>
```py
from typing import ClassVar
import matplotlib

class SomeClass:
    ROI_COLORS: ClassVar[list] = matplotlib.color_sequences["tab20"]

    # Shuffle the order around
    ROI_COLORS = [*ROI_COLORS[0::2], *ROI_COLORS[1::2]]  # Mutable class attributes should be annotated with `typing.ClassVar` (RUF012) [Ln 11, Col 18]
```



### Version

ruff 0.14.4

---

_Comment by @ntBre on 2025-11-12 21:52_

Hmm, yeah this feels a bit like a false positive to me. In Rust I would do something like this pseudocode:

```rust
static ROI_COLORS: [...] = {
    let tmp = matplotlib.color_sequences["tab20"];
    [*tmp[0::2], *tmp[1::2]]
};
```

but obviously Python doesn't have similar blocks. 

@amyreese  is there a more idiomatic pattern here, or should we try to check if a variable has ever been declared a `ClassVar`?

---

_Label `question` added by @ntBre on 2025-11-12 21:52_

---

_Comment by @amyreese on 2025-11-12 22:35_

It feels like an anti-pattern to redefine a class value twice inline — I'd probably try to find a one-liner or define an intermediate variable elsewhere — but the lint rule itself is a false positive IMO because the variable was explicitly defined already with `ClassVar`. I'm guessing the lint rule is not expecting a class attribute to be redefined like this (back to the anti-pattern part).

---

_Comment by @anabelle2001 on 2025-11-12 23:47_

yeah. I would agree that redefinition of class variables is not common. It doesn't surprise me that people aren't doing this that often.

I think the best way to do it in one line would be to write `matplotlib.color_sequences["tab20"]` twice:

```py
class SomeClass:
    ROI_COLORS: ClassVar[list] = [
        *matplotlib.color_sequences["tab20"][0::2],
        *matplotlib.color_sequences["tab20"][1::2]
    ]
```

It's not too bad for a dictionary lookup, but for more expensive tasks, it probably makes sense to declare a variable before / inside the class, and delete it afterwards:

```py
tab20 = matplotlib.color_sequences["tab20"]
class SomeClass1:
    x: ClassVar[list] = [*tab20[0::2], *tab20[1::2]]
del tab20
```

```py
class SomeClass3:
    x: ClassVar[list] = [*(tab20:=matplotlib.color_sequences["tab20"])[0::2], *tab20[1::2]]
    del tab20
```

You could also modify the list in place without reassignment, but if you use a loop, i think you'd have to delete the variable afterwards?
```py
class SomeClass2:
    x: ClassVar[list] = matplotlib.color_sequences["tab20"]
    
    for item in x[1::2]:
        x.remove(item)
        x.append(item)
    del item
```

Here, I prefer using `ClassVar` reassignment over the alternatives, but if there's a more elegant way that i'm not seeing, please let me know! For now i've just added a `#type: ignore`.

---

_Comment by @ntBre on 2025-11-13 15:41_

It seems reasonable to me to look for previous declarations of the variable in the same scope. Let's give it a try in preview since the rule is stable.

---

_Label `question` removed by @ntBre on 2025-11-13 15:42_

---

_Label `rule` added by @ntBre on 2025-11-13 15:42_

---

_Referenced in [astral-sh/ruff#21478](../../astral-sh/ruff/pulls/21478.md) on 2025-11-16 15:28_

---

_Closed by @ntBre on 2025-11-17 20:52_

---
