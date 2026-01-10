---
number: 2348
title: How to annotate Iterators of Literals?
type: issue
state: closed
author: Pijukatel
labels:
  - bidirectional inference
assignees: []
created_at: 2026-01-05T15:59:26Z
updated_at: 2026-01-09T05:30:06Z
url: https://github.com/astral-sh/ty/issues/2348
synced_at: 2026-01-10T01:48:23Z
---

# How to annotate Iterators of Literals?

---

_Issue opened by @Pijukatel on 2026-01-05 15:59_

### Question

How to properly annotate an Iterator that is supposed to return just specific literals

This works in **mypy**, but thows error in **ty**  `Incompatible value of type cycle[Unknown | str]`

```py
from itertools import cycle
from typing import Literal, Iterator

a_or_b = Literal["a", "b"]
a_or_b_iterator: Iterator[a_or_b] = cycle(['a', 'b'])
```

### Version

_No response_

---

_Label `question` added by @Pijukatel on 2026-01-05 15:59_

---

_Comment by @MeGaGiGaGon on 2026-01-05 19:24_

You can make it work if you explicitly specify the `cycle` generic type (since `cycle` is actually a class, not a function):
https://play.ty.dev/16ec49da-506b-4c8a-a909-c366b08ad333
```py
from itertools import cycle
from typing import Literal, Iterator

a_or_b = Literal["a", "b"]
a_or_b_iterator: Iterator[a_or_b] = cycle[a_or_b](['a', 'b'])
```
The issue is probably something under bidirectional inference, though unsure.

---

_Label `question` removed by @carljm on 2026-01-05 20:01_

---

_Label `bidirectional inference` added by @carljm on 2026-01-05 20:01_

---

_Comment by @carljm on 2026-01-05 20:04_

Yeah, this looks like a case where we'd need to push down the type context into the list literal.

This seems similar to #2280, but higher priority since other type checkers do handle this.

---

_Added to milestone `Stable` by @carljm on 2026-01-05 20:04_

---

_Comment by @ibraheemdev on 2026-01-06 16:10_

We are propagating the context here, I believe this is the same underlying issue as https://github.com/astral-sh/ty/issues/1815.

---

_Closed by @ibraheemdev on 2026-01-09 05:30_

---
