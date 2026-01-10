```yaml
number: 1576
title: "`Iterable[list[int]]` as type context is not handled well"
type: issue
state: closed
author: AlexWaygood
labels:
  - bidirectional inference
assignees: []
created_at: 2025-11-17T11:41:49Z
updated_at: 2026-01-05T21:30:52Z
url: https://github.com/astral-sh/ty/issues/1576
synced_at: 2026-01-10T01:56:40Z
```

# `Iterable[list[int]]` as type context is not handled well

---

_Issue opened by @AlexWaygood on 2025-11-17 11:41_

### Summary

In this snippet:

```py
from typing import reveal_type, Iterable

x: Iterable[list[int]] = [[42], [56]]
reveal_type(x)
```

we currently reveal `list[Unknown | list[Unknown | int]]`. For comparison, mypy and pyrefly reveal `Iterable[list[int]]` and pyright reveals `list[list[int]]`. Either seems better than our current answer (though I like pyright's best!).

- [ty](https://play.ty.dev/7d2d3484-8e47-4d9a-97b9-7ec367822d3d)
- [pyright](https://pyright-play.net/?pyrightVersion=1.1.405&strict=true&code=GYJw9gtgBALgngBwJYDsDmUkQWEMogCmAboQIYA2A%2BvAoQDRQCSMhIZARhYQFA8AeALmat2XQgG0KSAM4wJqGAF0lUALxQJEgCwAmJYwkBWAGwq%2BRUpRqJCACn4BKHkA)
- [mypy](https://mypy-play.net/?mypy=latest&python=3.12&gist=53444c9cc1db48bbf406917f0d536900)
- [pyrefly](https://pyrefly.org/sandbox/?project=N4IgZglgNgpgziAXKOBDAdgEwEYHsAeAdAA4CeS4ATrgLYAEALqcROgOZ0Q3G6UN2UYANxiooAfSbEYAGjoBJBjEqpssADrpN%2BRAqUq1MANpQIcBkdYMAutboBeOkaMAWAEzW5RgKwA2W5qCImKSzDAAFPgAlJogMiAArgzQcCTkiCAAxHQAqsmmTHRgCegAxsm46HCampgwYEW8NKgM4ugJNNjKkbpWUXQAtAB8dOaUiJp0UwIwDAmU6EXqIAByHV3jdMD4AL7LsfFkgmBQpIQMtFAU2QAKpMenoxg4BHSllZBs8y0QlYSa2QAyjAYHQABYMBjEOCIAD0sKO9VOhF4bFhMHQsMwuFKcFh73Qn2%2BFUxjUodFQQlQ0FUsDeHwgXxUJLouGIJNSmjIDDBlQGIkocF%2Bi0cywAzIQAIxufboEA7eKocoQEQAMWgMAoaCweCIZHlQA)

cc. @ibraheemdev 

### Version

_No response_

---

_Label `bidirectional inference` added by @AlexWaygood on 2025-11-17 11:41_

---

_Added to milestone `Stable` by @carljm on 2025-11-17 15:27_

---

_Comment by @ibraheemdev on 2025-11-17 16:03_

In general, we currently only consider type context if the types are equivalent. We probably need to extend this to a subclass check (and infer the correct type arguments).

---

_Comment by @carljm on 2025-12-17 23:15_

@ibraheemdev does this example fall into the category of "needs new constraint solver"?

https://play.ty.dev/68b3e41b-7eee-4658-ae20-3a2b0f59fb7d

```py
from collections.abc import Mapping
from typing import Literal

def process(data_types: Mapping[str, Literal["BOOL", "INT", "FLOAT"]], /) -> None:
    ...

process({"foo":"BOOL"})
```

(We handle it fine if it is `Mapping[str, str]`, but not with the union of literals)

---

_Comment by @ibraheemdev on 2025-12-17 23:52_

@carljm that one should also be fixed by https://github.com/astral-sh/ruff/pull/21930.

---

_Closed by @ibraheemdev on 2025-12-30 22:03_

---

_Comment by @zsol on 2026-01-05 14:57_

I expected [this code](https://play.ty.dev/a5cc514f-32e2-418e-b865-2df3d635c005) to no longer error after this issue was closed, but that doesn't seem to be the case. Do you want me to open a new issue?

---

_Comment by @ibraheemdev on 2026-01-05 21:30_

@zsol that example requires bidirectional inference for comprehensions, which is tracked by https://github.com/astral-sh/ty/issues/1602.

---
