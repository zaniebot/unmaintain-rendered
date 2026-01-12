```yaml
number: 5111
title: "`UP035` does not detect `typing.NoReturn`"
type: issue
state: closed
author: DetachHead
labels:
  - good first issue
  - rule
  - help wanted
assignees: []
created_at: 2023-06-15T04:07:52Z
updated_at: 2023-06-16T00:51:18Z
url: https://github.com/astral-sh/ruff/issues/5111
synced_at: 2026-01-12T15:54:45Z
```

# `UP035` does not detect `typing.NoReturn`

---

_@DetachHead_

in python 3.11, `typing.NoReturn` is deprecated in favor of `typing.Never` and `typing.Union` is deprecated in favor of the `|` operator.
```py
from typing import NoReturn, Union # no error
```

---

_Label `rule` added by @charliermarsh on 2023-06-15 04:14_

---

_Comment by @charliermarsh on 2023-06-15 04:14_

Makes sense, thanks.

---

_Label `help wanted` added by @charliermarsh on 2023-06-15 13:46_

---

_Label `good first issue` added by @charliermarsh on 2023-06-15 13:47_

---

_Comment by @bluetech on 2023-06-15 19:17_

`NoReturn` and `Union` are not deprecated in Python so I think the word "deprecated" should not be used for them.

The `NoReturn` lint should be qualified to when it appears in not-return position, there `Never` makes more sense.

`Union` is already covered by UP007 isn't it?

---

_Comment by @bluetech on 2023-06-15 19:18_

Actually the `NoReturn` case is handled by PYI050.

---

_Comment by @charliermarsh on 2023-06-15 20:49_

Ah, I think I agree -- unlike in PEP 585, PEP 604 didn't deprecate `Union`. And based on my reading of the docs, I think I agree that `Never` is not necessarily a deprecation of `NoReturn`.

---

_Closed by @charliermarsh on 2023-06-15 20:49_

---

_Comment by @KotlinIsland on 2023-06-15 23:51_

`Never`: 
> New in version 3.11: On older Python versions, [`NoReturn`](https://docs.python.org/3/library/typing.html#typing.NoReturn) may be used to express the same concept. `Never` was added to make the intended meaning more explicit.

`NoReturn`:
> Starting in Python 3.11, the [`Never`](https://docs.python.org/3/library/typing.html#typing.Never) type should be used for this concept instead.

While not explicitly deprecated (yet), the docs strongly state that `Never` should be used over `NoReturn`.

PYI050 doesn't apply to return types, and also doesn't apply to python files, so it's very limited in scope.

---

_Comment by @DetachHead on 2023-06-15 23:59_

> `Union` is already covered by UP007 isn't it?

oh right. i wasn't looking at usages only the imports. that's fine then

but yeah as @KotlinIsland said, `PYI050` isn't really sufficient

---

_Comment by @charliermarsh on 2023-06-16 00:37_

> Starting in Python 3.11, the [Never](https://docs.python.org/3/library/typing.html#typing.Never) type should be used for this concept instead.

I think the full paragraph reads differently:

> Special type indicating that a function never returns... NoReturn can also be used as a [bottom type](https://en.wikipedia.org/wiki/Bottom_type), a type that has no values. Starting in Python 3.11, the [Never](https://docs.python.org/3/library/typing.html#typing.Never) type should be used for this concept instead.

The suggestion is to use `Never` instead of `NoReturn` when you're indicating a "bottom type" (a type that has no values). My read of the wording here is that you are intended to use `NoReturn` as a return type for functions that don't return, but `Never` for (e.g.) arguments or other variables that have no value.

---

_Comment by @DetachHead on 2023-06-16 00:48_

imo having two separate types that do the exact same thing is just confusing and misleading. i've seen people misinterpret `NoReturn` to mean that the function returns `None`. having a separate `Never` type while not treating the old one as deprecated just makes that misinterpretation more likely. i feel like the name `NoReturn` was only picked because nobody considered the fact that there are other use cases for it, which is why i think it should be avoided altogether.

but i guess it comes down to personal preference, so perhaps a new rule could be created to ban all usages of `NoReturn`?

---

_Renamed from "`UP035` does not detect `typing.NoReturn` and `typing.Union`" to "`UP035` does not detect `typing.NoReturn`" by @DetachHead on 2023-06-16 00:51_

---
