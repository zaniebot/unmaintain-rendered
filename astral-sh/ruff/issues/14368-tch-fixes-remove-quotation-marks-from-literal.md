```yaml
number: 14368
title: "TCH fixes remove quotation marks from `Literal` strings"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2024-11-15T19:36:33Z
updated_at: 2024-11-16T17:58:04Z
url: https://github.com/astral-sh/ruff/issues/14368
synced_at: 2026-01-10T11:09:56Z
```

# TCH fixes remove quotation marks from `Literal` strings

---

_Issue opened by @dscorbett on 2024-11-15 19:36_

The fixes for TCH001, TCH002, and TCH003 sometimes delete quotation marks from `Literal`s when `lint.flake8-type-checking.quote-annotations = true`. This can make a program fail to type-check. I am not sure exactly what makes it delete quotation marks but it seems to only happen to literals within nested brackets.

```console
$ ruff --version
ruff 0.7.4

$ cat tch003.py
from collections.abc import Sequence
from typing import Callable, Literal
def f(x: Sequence[Callable[[Literal["1"]], bool]]) -> bool:
    return False
def g(x: Literal["1"]) -> bool:
    return False
print(f([g]))

$ mypy tch003.py
Success: no issues found in 1 source file

$ ruff check --isolated --config 'lint.flake8-type-checking.quote-annotations = true' --select TCH003 tch003.py --unsafe-fixes --fix
Found 1 error (1 fixed, 0 remaining).

$ cat tch003.py
from typing import Callable, Literal, TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Sequence
def f(x: "Sequence[Callable[[Literal[1]], bool]]") -> bool:
    return False
def g(x: Literal["1"]) -> bool:
    return False
print(f([g]))

$ mypy tch003.py
tch003.py:9: error: List item 0 has incompatible type "Callable[[Literal['1']], bool]"; expected "Callable[[Literal[1]], bool]"  [list-item]
Found 1 error in 1 file (checked 1 source file)
```

With `"x"` instead of `"1"` in that example, `"x"` is rewritten to `x` and the final mypy error is:
```console
$ mypy tch003.py
tch003.py:5: error: Name "x" is not defined  [name-defined]
Found 1 error in 1 file (checked 1 source file)
```

---

_Label `bug` added by @dylwil3 on 2024-11-15 20:54_

---

_Comment by @dylwil3 on 2024-11-15 21:10_

Thanks so much!

Maybe this logic needs some inspection (or the `QuoteAnnotator`'s `Visitor` implementation)?

https://github.com/astral-sh/ruff/blob/c847cad389f202edae868765e690cb7042e88264/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L219-L281

Also appears that the nesting in the type annotation has some effect. For example, this fixes correctly:

```python
from collections.abc import Sequence
from typing import Callable, Literal


def f(x: Sequence[Literal["1"]]) -> bool:
    return False
```

becomes:

```python
from typing import Callable, Literal, TYPE_CHECKING

if TYPE_CHECKING:
    from collections.abc import Sequence


def f(x: "Sequence[Literal['1']]") -> bool:
    return False
```

[Playground link](https://play.ruff.rs/6580bc1b-a879-4d79-a814-0bd5ca2883f5)


---

_Assigned to @dylwil3 by @dylwil3 on 2024-11-15 21:53_

---

_Comment by @dylwil3 on 2024-11-15 22:14_

Looks like the issue was that _lists_ were not handled (i.e. the first argument to `Callable`, which is a list) as the quoted annotation builder was recursing through the expressions. Gonna check a few more edge cases to see if some other things weren't handled.

But if I miss any I'm counting on you to find a slick example 😂  you're so good at this!

---

_Closed by @charliermarsh on 2024-11-16 17:58_

---

_Closed by @charliermarsh on 2024-11-16 17:58_

---
