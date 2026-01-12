```yaml
number: 901
title: "Typing an equivalent of rust's unwrap in ty (`def unwrap[T](x: T | None) -> T`)"
type: issue
state: closed
author: karlicoss
labels: []
assignees: []
created_at: 2025-07-27T01:15:35Z
updated_at: 2025-07-27T11:03:06Z
url: https://github.com/astral-sh/ty/issues/901
synced_at: 2026-01-12T15:54:24Z
```

# Typing an equivalent of rust's unwrap in ty (`def unwrap[T](x: T | None) -> T`)

---

_@karlicoss_

### Summary

Apologies if this is a duplicate/subissue of some bigger issue - I searched on issue tracker and couldn't find anything relevant.

Sometimes I find it quite useful to have a function similar to rust's `unwrap` in my projects.
In mypy, this snippet passes fine ([mypy playground link](
https://mypy-play.net/?mypy=latest&python=3.12&gist=e8bb5656911cc53e37a8a52bc9121580))

```python
from typing import cast, reveal_type

t = cast(int | None, 5)
reveal_type(t)  # reveals int | None


def unwrap[T](x: T | None) -> T:
    assert x is not None
    return x

reveal_type(unwrap(t))  # reveals int
unwrap(t) + 123   # type checks
```

However, ty ([playground link](https://play.ty.dev/2ce5ae60-a0bd-4330-abd9-42a0f854d0d6)) seems to be inferring the result as `T | None`, and complains about `unwrap(t) + 123` as a result.

Not sure if it's a bug (i.e. ty can't pattern match `int | None` against `T | None` yet), or it's some subtle difference in semantics (i.e. maybe mypy is doing something not fully correct here?).

Out of interest I looked at the `ty_extensions` stuff that I've seen mentioned in a couple of other issues and it seems to type check as I expect:

```python
from ty_extensions import Not, Intersection
def unwrap2[T](x: T | None) -> Intersection[T, Not[None]]:
    assert x is not None
    return x

reveal_type(unwrap2(t))  # reveals int
unwrap2(t) + 123  # type checks
```

Would be curious to know what's the right way!

### Version

latest master/playground

---

_Comment by @MatthewMckee4 on 2025-07-27 10:30_

Your first code snippet is correct and mypy is correct. We just do not support this yet.

This is similar to what is shown here

https://github.com/astral-sh/ruff/pull/18398#discussion_r2116936039

---

_Comment by @AlexWaygood on 2025-07-27 11:03_

Thanks for the report! This is a duplicate of https://github.com/astral-sh/ty/issues/623

---

_Closed by @AlexWaygood on 2025-07-27 11:03_

---
