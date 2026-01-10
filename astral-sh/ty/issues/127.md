```yaml
number: 127
title: Silence all diagnostics in unreachable code?
type: issue
state: closed
author: sharkdp
labels:
  - diagnostics
  - unreachable-code
assignees: []
created_at: 2025-04-14T11:02:19Z
updated_at: 2025-06-17T08:04:30Z
url: https://github.com/astral-sh/ty/issues/127
synced_at: 2026-01-10T02:08:20Z
```

# Silence all diagnostics in unreachable code?

---

_Issue opened by @sharkdp on 2025-04-14 11:02_

Our current approach to silencing only a subset of diagnostics in unreachable sections of code has some issues. They are [documented here](https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md#limitations-of-the-current-approach). In short, we still emit false positives in some cases:
```py
if False:
    x: int = 1
else:
    x: str = "a"

if False:
    # TODO We currently emit a false positive here:
    # error: [invalid-assignment] "Object of type `Literal["a"]` is not assignable to `int`"
    other: int = x
else:
    other: str = x
```

We should re-evaluate if it makes more sense to silence *all* diagnostics (possibly with a very limited set of exceptions, like `reveal_type`-messages) in unreachable sections.



---

_Renamed from "[red-knot] Silence *all* diagnostics in unreachable code?" to "[red-knot] Silence all diagnostics in unreachable code?" by @sharkdp on 2025-04-14 11:03_

---

_Renamed from "[red-knot] Silence all diagnostics in unreachable code?" to "Silence all diagnostics in unreachable code?" by @MichaReiser on 2025-05-07 15:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-11 07:39_

---

_Comment by @sharkdp on 2025-05-26 11:42_

Example from #511:

```py
import sys
from typing import Union

if sys.version_info >= (3, 10):
    T = str | None
else:
    T = Union[str, None]
```

---

_Label `unreachable-code` added by @sharkdp on 2025-05-26 11:42_

---

_Comment by @sharkdp on 2025-05-26 12:10_

https://github.com/astral-sh/ty/issues/443 includes another example which we solved (temporarily) by ignoring `division-by-zero` diagnostics by default.

---

_Comment by @karlicoss on 2025-05-27 00:19_

(btw the link in the issue description is broken, here's the correct one https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/resources/mdtest/unreachable.md )

----

Another case where silencing all diagnostic might be useful is tests guarded by `pytest.skip`. pytest.skip is a bit broken atm [due to a different issue](https://github.com/astral-sh/ruff/pull/17832#issuecomment-2855425767), so I just simulated it with `sys.exit`

```
from pathlib import Path
from typing import Never
import sys

def pytest_skip() -> Never:  # ty: ignore[invalid-return-type]
    sys.exit(0)

def test_using_path_walk() -> None:
    if sys.version_info < (3, 12):
        pytest_skip()

    # Path.walk only available from 3.12
    Path('.').walk()
```

Arguably, this test could have been `@pytest.mark.skipIf(sys.version_info < (3, 12))`, but that would be even harder to support -- not sure if it's possible to propagate this information through `pytest.skipIf` and back to the type checker? E.g. `mypy --python-version 3.11` passes the example with `pytest.skip()`, but not with `skipIf`

-----

FWIW, in my experience, silencing everything in unreachable branches in mypy hasn't been an issue -- but I can't really remember any case where it wasn't due to `sys.version_info` guards. And in these cases I have a CI matrix testing all python versions I care about anyway so ultimately they all end up covered.

---

_Comment by @sharkdp on 2025-05-27 06:19_

The unresolved-attribute error on `Path('.').walk()` should actually disappear in this specific example once we implement #180. You can test this by replacing the `pytest_skip()` call with `return` in that example.

---

_Comment by @sharkdp on 2025-06-17 08:04_

With https://github.com/astral-sh/ruff/pull/18621 merged, we now silence a whole new class of diagnostics in unreachable code (when there is conflicting type information, like in the issue description example). I am currently not aware of any limitations of our approach, except for #667 which I reported separately. It is certainly possible that we are still missing something (issues similar to #667 where we mistakenly report diagnostics when symbols in type annotations are `Never` or `Unknown`), but I'm closing this for now.

---

_Closed by @sharkdp on 2025-06-17 08:04_

---
