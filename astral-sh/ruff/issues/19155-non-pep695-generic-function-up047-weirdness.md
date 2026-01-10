```yaml
number: 19155
title: non-pep695-generic-function (UP047) weirdness
type: issue
state: open
author: actionless
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-07-05T17:30:30Z
updated_at: 2025-09-03T13:31:18Z
url: https://github.com/astral-sh/ruff/issues/19155
synced_at: 2026-01-10T11:09:59Z
```

# non-pep695-generic-function (UP047) weirdness

---

_Issue opened by @actionless on 2025-07-05 17:30_

### Summary

Proposed fix seems to break the typing, as it would remoev the existing type boundaries of a TypeVar:


```python
import sys
from typing import TypeVar


class Class1:
    param = 1


class Class2:
    param = 2


type AnyofTwo = Class1 | Class2
SameClassT = TypeVar("SameClassT", Class1, Class2)


def print_foo(arg: AnyofTwo) -> None:
    sys.stderr.write(f"{arg.param}\n")


def universal_func_1(arg: SameClassT) -> SameClassT: # RUFF /* W: Generic function `universal_func_1` should use type parameters
    print_foo(arg)
    return arg


def universal_func_2[T](arg: T) -> T:
    """
    This is how `ruff check --fix` would modify `universal_func_1()`.
    """
    print_foo(arg) # MYPY /* E: Argument 1 to "print_foo" has incompatible type "T"; expected "Class1 | Class2"  [arg-type]
    return arg

```


```console
$ ruff check test_pyrefly_01.py
test_pyrefly_01.py:21:5: UP047 [*] Generic function `universal_func_1` should use type parameters
   |
21 | def universal_func_1(arg: SameClassT) -> SameClassT:
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP047
22 |     print_foo(arg)
23 |     return arg
   |
   = help: Use type parameters

Found 1 error.
[*] 1 fixable with the --fix option.
```  

### Version

ruff 0.12.1

---

_Comment by @actionless on 2025-07-05 17:32_

and in general, Ruff's documentation already mentions that such 'upgrade' would remove co/contra-variance information, and as we see in the example above - also the type boundaries.

So I'm very challenged to understand why even to propose such a fix/rule.

---

_Comment by @actionless on 2025-07-05 17:48_

mb the rule should fire only if TypeVar doesn't have any arguments except for the name, e.g.:

```python
SameClassT = TypeVar("SameClassT")
```

or if it have only one additional arg `infer_covariance` only equal to `True`, but no other args/kwargs

---

_Comment by @ntBre on 2025-07-06 02:17_

I might be missing something here, but `def universal_func_2[T](arg: T) -> T:` is not the suggested fix for UP047 in this case. Here's your example in the playground: https://play.ruff.rs/a94f56e1-90e9-42b7-bf23-e66fc87fc7c0.

Applying the autofix for UP047 yields this:

```python
def universal_func_1[SameClassT: (Class1, Class2)](arg: SameClassT) -> SameClassT: # RUFF /* W: Generic function `universal_func_1` should use type parameters
    print_foo(arg)
    return arg
```

which I believe preserves the original bounds from the `TypveVar`.

---

_Label `question` added by @ntBre on 2025-07-06 02:17_

---

_Comment by @actionless on 2025-07-07 10:37_

i double-checked, indeed ruff 0.12.1 autofix produces the correct result, probably i was running in an outdated virtualenv, because for some reason previously autofix was returning just `[T]` without constraints

---

_Closed by @actionless on 2025-07-07 10:37_

---

_Comment by @s-banach on 2025-09-03 13:21_

How do I see the suggested fix without running `unsafe-fixes` on my entire codebase?
I was looking at [the documentation](https://docs.astral.sh/ruff/rules/non-pep695-generic-function/) and it doesn't mention how to handle constraints on `T`.
I had to google this issue to figure out how to fix it.


---

_Comment by @ntBre on 2025-09-03 13:30_

I think the best option on a released version of Ruff is the `--diff` flag. However, we [just](https://github.com/astral-sh/ruff/pull/19919) added the ability to show fix diffs in the default output format in preview. That should go out in the release tomorrow. So those commands would be something like:

```shell
ruff check --select UP047 --unsafe-fixes --diff /path/to/file  # current
ruff check --select UP047 --unsafe-fixes /path/to/file         # after the next release
```

---

_Comment by @ntBre on 2025-09-03 13:30_

We could also add a couple of additional examples to the docs, that seems like it would be helpful.

---

_Reopened by @ntBre on 2025-09-03 13:31_

---

_Label `documentation` added by @ntBre on 2025-09-03 13:31_

---

_Label `help wanted` added by @ntBre on 2025-09-03 13:31_

---

_Label `question` removed by @ntBre on 2025-09-03 13:31_

---
