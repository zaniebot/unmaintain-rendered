---
number: 2216
title: False positive when appending a dict to a list of TypedDict unions
type: issue
state: closed
author: kkpattern
labels:
  - bidirectional inference
assignees: []
created_at: 2025-12-25T02:22:19Z
updated_at: 2025-12-29T22:08:30Z
url: https://github.com/astral-sh/ty/issues/2216
synced_at: 2026-01-10T01:51:14Z
---

# False positive when appending a dict to a list of TypedDict unions

---

_Issue opened by @kkpattern on 2025-12-25 02:22_

### Summary

The following code triggers a false positive invalid-argument-type error when appending a dictionary literal to a list of TypedDict unions:

```python
from typing import Literal, TypedDict


class Foo(TypedDict):
    role: Literal["assistant"]
    content: str

class Bar(TypedDict):
    role: Literal["user"]
    content: str


foo_list: list[Foo | Bar] = []
foo_list.append({"role": "assistant", "content": "hello world"})

# Argument to bound method `append` is incorrect: Expected `Foo | Bar`, found `dict[Unknown | str, Unknown | str]` (invalid-argument-type) [Ln 14, Col 17]
```

Both mypy and pyright pass without issues.

Also the share button in the ty playground seems broken, I got the following error:

```
index-CzSy9k3J.js:54 Failed to share playground TypeError: Failed to fetch
    at H0 (index-CzSy9k3J.js:64:35618)
    at L0 (index-CzSy9k3J.js:64:35791)
    at index-CzSy9k3J.js:98:7335
    at onClick (index-CzSy9k3J.js:54:4234)
    at Zh (index-CzSy9k3J.js:49:117237)
    at index-CzSy9k3J.js:49:122342
    at Pc (index-CzSy9k3J.js:49:9040)
    at zu (index-CzSy9k3J.js:49:118471)
    at Gu (index-CzSy9k3J.js:50:26674)
    at Xb (index-CzSy9k3J.js:50:26496)
```

- [pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAySMApiAIYA2ANFACqIkAmAIkgMYwCwAUL7%2B0rkAzsKgAxMGAAUDBMzacAlAC5eUDVHCUSKwsTJUA2gCIRwpMJjkUMEwF11m9mFslbeqyH49B5qABC5CCyjKwcMKpOGtq6%2BqQUlKYArsJkDtFQLm4eUF4%2BvMBSAPqUljB6ZVZGkmBQAD6BwfZQALxQRo48RWCl5QB05AjyKEzSAN4msSZ6ZqLlNna0JtmktjNQJgAWJJSUdQDuuJRMJgC%2BSkA)
- [mypy playground](https://mypy-play.net/?mypy=latest&python=3.12&gist=ada118463676e35110c6f35539168520)



### Version

ty 0.0.6

---

_Comment by @charliermarsh on 2025-12-25 03:02_

(Can you share the browser you're on? The share button works fine for me -- just tested it.)

---

_Comment by @kkpattern on 2025-12-25 03:12_

Windows 11
Chrome 143.0.7499.146.

I noticed there is another error log in the console:

```
Failed to load resource: net::ERR_CONNECTION_TIMED_OUT    api.astral-1ad.workers.dev/:1
```

It looks like a local network issue on my end, but the rest of the playground works fine.

BTW, Happy holidays!

---

_Comment by @charliermarsh on 2025-12-25 03:12_

> BTW, Happy holidays!

You too!

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-12-25 08:37_

---

_Added to milestone `Stable` by @carljm on 2025-12-27 00:38_

---

_Comment by @ibraheemdev on 2025-12-29 22:08_

It looks like the issue here is that the list element is a union of `TypedDict`s, which we currently don't support.

---

_Closed by @ibraheemdev on 2025-12-29 22:08_

---
