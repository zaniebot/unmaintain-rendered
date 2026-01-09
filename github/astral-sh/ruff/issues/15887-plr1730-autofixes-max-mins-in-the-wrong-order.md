---
number: 15887
title: PLR1730 autofixes max/mins in the wrong order
type: issue
state: closed
author: TylerYep
labels:
  - bug
  - fixes
  - help wanted
assignees: []
created_at: 2025-02-02T22:26:12Z
updated_at: 2025-02-05T09:29:12Z
url: https://github.com/astral-sh/ruff/issues/15887
synced_at: 2026-01-07T13:12:16-06:00
---

# PLR1730 autofixes max/mins in the wrong order

---

_Issue opened by @TylerYep on 2025-02-02 22:26_

### Description

Ruff v0.9.4

Output:
```
PLR1730 [*] Replace `if` statement with `self.top = min(self.top, curr)`
    |
352 | /             if curr <= self.top:
353 | |                 self.top = curr
    | |_______________________________^ PLR1730
```

Ruff autofixes this to `self.top = min(self.top, curr)`, but this is incorrect.

The correct autofix is `self.top = min(curr, self.top)`, because if curr and self.top are equivalent, self.top should not be updated.

This autofix works when equivalent values are interchangeable, but when we use custom overrides for the `__eq__` function, values may be equal but not interchangeable.

---

_Comment by @AlexWaygood on 2025-02-03 12:33_

Yes, agreed that this is buggy. Here's a minimal repro for how this can change the order of elements that compare equal but are not equivalent:

```pycon
>>> import itertools
>>> COUNT = itertools.count()
>>> from functools import total_ordering
>>> @total_ordering
... class Foo:
...     def __init__(self, value):
...         self.value = value
...         self.count = next(COUNT)
...     def __eq__(self, other):
...         isinstance(other, Foo) and other.value == self.value
...     def __lt__(self, other):
...         if not isinstance(other, Foo):
...             return NotImplemented
...         self.value < other.value
...         
>>> current = Foo(1)
>>> top = Foo(1)
>>> current.count
0
>>> top.count
1
>>> current = min(top, current)
>>> current.count
1
```

---

_Label `bug` added by @AlexWaygood on 2025-02-03 12:33_

---

_Label `fixes` added by @AlexWaygood on 2025-02-03 12:33_

---

_Label `help wanted` added by @AlexWaygood on 2025-02-03 12:33_

---

_Comment by @VascoSch92 on 2025-02-03 12:40_

Can I give a try? :-)


---

_Comment by @AlexWaygood on 2025-02-03 12:41_

> Can I give a try? :-)

Go for it! Let us know if you need any help or clarification :-)

---

_Assigned to @VascoSch92 by @AlexWaygood on 2025-02-03 12:41_

---

_Comment by @VascoSch92 on 2025-02-03 16:32_

Just a clarification. I was looking into the code and I saw this snippet

```rust
/// R1730, R1731
pub(crate) fn if_stmt_min_max(checker: &mut Checker, stmt_if: &ast::StmtIf)
```
Two small questions:

- So I suppose there are two rules merged in one, i.e., `PLR1730` and `PLR1731` (one for min and one for max), correct?
But in the docs we have just `1730`: https://docs.astral.sh/ruff/rules/if-stmt-min-max/

- The example above was just for `min` but I suppose the same bug persist also for `max`, correct?

---

_Comment by @MichaReiser on 2025-02-03 17:29_

Yes, Ruff has a single rule that corresponds to two pylint rules. I'd expect that the same bug is present for `max`. The best way to verify it is to add a test for both `min` and `max` before fixing the bug.

---

_Comment by @VascoSch92 on 2025-02-03 20:12_

Looking at the python documentation:

> If multiple items are maximal, the function returns the first one encountered. [max doc](https://docs.python.org/3/library/functions.html#max)

> If multiple items are minimal, the function returns the first one encountered. [min doc](https://docs.python.org/3/library/functions.html#min)

As a general rule, **the variable that we want to update must always be in the first position in the `min`/`max` method**. (To be consistent with python implementation). 

Therefore, the autofix discussed at the beginning of this issue was indeed correct. But the problem mentioned is real and present.  In fact, we have at line 452 of [if_stmt_min_max.py.snap](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLR1730_if_stmt_min_max.py.snap)

```text
ℹ Safe fix
155 155 |         if value >= self._max:
156 156 |             self._max = value
157 157 | 
158     |-        if self._min <= value:
159     |-            self._min = value
    158 |+        self._min = max(value, self._min)
160 159 |         if self._max >= value:
161 160 |             self._max = value
```

which is wrong as it should be `self._min = max(self._min, value)`.

I will check if we can enforce the above mentioned rule in the suggestion and in the auto fix ;-) 

Or am I understanding something wrong?

---

_Comment by @MichaReiser on 2025-02-04 09:37_

That all sounds correct to me. Thanks for the detailed write up!

---

_Referenced in [astral-sh/ruff#15930](../../astral-sh/ruff/pulls/15930.md) on 2025-02-04 11:49_

---

_Renamed from "PLR1730 autofixes mins in the wrong order" to "PLR1730 autofixes max/mins in the wrong order" by @TylerYep on 2025-02-04 23:19_

---

_Closed by @MichaReiser on 2025-02-05 09:29_

---
