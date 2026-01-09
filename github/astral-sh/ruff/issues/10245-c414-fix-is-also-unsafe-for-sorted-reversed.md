---
number: 10245
title: C414 --fix is also unsafe for sorted(reversed(iterable)), and other issues encountered when comparing with shed
type: issue
state: open
author: jakkdl
labels:
  - documentation
assignees: []
created_at: 2024-03-06T10:34:49Z
updated_at: 2024-03-13T15:12:34Z
url: https://github.com/astral-sh/ruff/issues/10245
synced_at: 2026-01-07T13:12:15-06:00
---

# C414 --fix is also unsafe for sorted(reversed(iterable)), and other issues encountered when comparing with shed

---

_Issue opened by @jakkdl on 2024-03-06 10:34_

C414 is marked as unsafe fix, but only because of dropping comments: https://docs.astral.sh/ruff/rules/unnecessary-double-cast-or-process/#fix-safety

EDIT: I noticed now that C413 has an extensive warning about safety with regards to stable sorting https://docs.astral.sh/ruff/rules/unnecessary-call-around-sorted/#fix-safety - so the fix is perhaps just that it should be copied over?

But it also breaks sorting stability in some cases, autofixing `sorted(reversed(iterable))` to `sorted(iterable)`.

full shell command:
```sh
$ ruff check --select=C414 --fix --unsafe-fixes --isolated - <<< 'sorted(reversed(iterable))'  
sorted(iterable)
Found 1 error (1 fixed, 0 remaining).
```
ruff version: 0.3.0
platform: linux

And a full minimal example where the problem is demonstrated. Assertions pass before autofixing with ruff, but fail afterwards.
```py
from __future__ import annotations
from dataclasses import dataclass
@dataclass
class MyClass:
    a: int
    b: int
    def __lt__(self, other: MyClass) -> bool:
        return self.a < other.a

first = MyClass(1, 1)
second = MyClass(1, 2)

# ruff will remove the reversed call in the line below, breaking the assertion
assert sorted(reversed((first, second))) == [second, first]
assert sorted((first, second)) == [first, second]
```

Found in the process of replacing some of sheds libcst code mods with ruff https://github.com/Zac-HD/shed/pull/105
CodeMod in question: https://github.com/Zac-HD/shed/blob/692a429bff19c3e1a798b97cb45396160dc138eb/src/shed/_codemods.py#L275-L315

Unrelated to this specific issue: That codemod also handles cases of combining `reverse`:
`sorted(sorted(iterable, reverse=True), reverse=False)` -> `sorted(iterable, reverse=False)`
`sorted(sorted(iterable, reverse=False), reverse=True)` -> `sorted(iterable, reverse=True)`

---

_Comment by @jakkdl on 2024-03-06 10:52_

EDIT: see https://github.com/Zac-HD/shed/blob/master/CODEMODS.md#replace_unnecessary_subscript_reversal

I'll continue mentioning some related things popping up, can open dedicated issues for them later if you want:
C415 does not have an autofix in ruff, but does in shed. So you might be interested in copying the implementation/logic: https://github.com/Zac-HD/shed/blob/692a429bff19c3e1a798b97cb45396160dc138eb/src/shed/_codemods.py#L317-L330

---

_Comment by @jakkdl on 2024-03-06 11:26_

[E711 docs](https://docs.astral.sh/ruff/rules/none-comparison/) states 'fix is always available' but it's marked as unsafe
```
$ ruff check --select=E711 --isolated --fix-only - <<< 'z == None' 
z == None
No errors fixed (1 fix available with `--unsafe-fixes`).
```

The "Use instead" in the example also only contains one of the two input examples in the Example: https://docs.astral.sh/ruff/rules/none-comparison/#example

---

_Comment by @jakkdl on 2024-03-06 12:20_

shed has a custom codemod for handling empty collections passed to builtins, which isn't fully covered by ruff - likely because flake8-comprehensions doesn't cover them either. (I have not dug into if they've been rejected by flake8-comprehensions, or just not considered)
```python
# handled by ruff
list([]) # C410
list(()) # C410

dict({}) # C418
dict([]) # C406
dict(()) # C406

tuple([]) # C409
tuple(()) # C409

# ruff does not raise any errors
list({}) 
list("")
dict("")
tuple({})
tuple("")
```

---

_Renamed from "C414 --fix is also unsafe for sorted(reversed(iterable))" to "C414 --fix is also unsafe for sorted(reversed(iterable)), and other issues encountered when comparing with shed" by @jakkdl on 2024-03-06 12:20_

---

_Comment by @jakkdl on 2024-03-06 12:42_

EDIT: see https://github.com/Zac-HD/shed/blob/master/CODEMODS.md#leave_assert

there's two checks for `assert False` being disallowed (PT015 and B011), but none for `assert True` (though F631 is related).

---

_Comment by @jakkdl on 2024-03-06 14:04_

[PT018 docs](https://docs.astral.sh/ruff/rules/pytest-composite-assertion/) also mention nothing about being unsafe
```sh
$ ruff check --select=PT018 --fix - <<< 'assert a and b'          
assert a and b
-:1:1: PT018 Assertion should be broken down into multiple parts
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

and it very much is, doesn't even handle the trivial case of a single trailing comment:
```sh
$ ruff check --select=PT018 --fix --unsafe-fixes - <<< 'assert a and b # comment'
assert a
assert b
Found 1 error (1 fixed, 0 remaining).
```
shed has a very chonky, afaik fully comment-safe, autofix for it. Since you don't use libcst, and it being very much tailored to it wrt comments, it maybe isn't of that much use to you though https://github.com/Zac-HD/shed/blob/692a429bff19c3e1a798b97cb45396160dc138eb/src/shed/_codemods.py#L476-625

---

_Comment by @charliermarsh on 2024-03-06 14:10_

Thanks! Heads up that it's not defined that removing comments merits an unsafe fix, there's discussion here that needs a decision: https://github.com/astral-sh/ruff/issues/9790.

---

_Comment by @jakkdl on 2024-03-06 14:46_

Good to know! Regardless of the decision though, it seems you need a pass (and/or automated test) to make sure that checks that require `--unsafe-fixes` are documented as such.

# 

SIM117 joins the ranks of checks not being documented as requiring `--unsafe-fixes`, and it appears to give up on collapsing once it hits the line-length limit (regardless of python version). `ruff format` handles multi-line `with`s perfectly.

before
```python
with make_context_manager() as cm1:
    with make_context_manager() as cm2:
        with make_context_manager() as cm3:
            with make_context_manager() as cm4:
                pass
```
after running `ruff --select=SIM117 --unsafe-fixes --fix --target-version=py312 --isolated foo.py`
```python
with make_context_manager() as cm1, make_context_manager() as cm2:
    with make_context_manager() as cm3:
        with make_context_manager() as cm4:
            pass
```

Shed codemod that handles cases like the above: https://github.com/Zac-HD/shed/blob/692a429bff19c3e1a798b97cb45396160dc138eb/src/shed/_codemods.py#L688-L749
Inspired by pybetter: https://github.com/lensvol/pybetter/blob/master/pybetter/transformers/nested_withs.py

---

_Comment by @jakkdl on 2024-03-06 15:06_

SIM101 and PLR1701 seems to be the exact same check, except the former requires `--unsafe-fixes` while the latter doesn't...
which is entirely backward because PLR1701 does more than just remove comments:
```
$ ruff check --select=PLR1701 --fix - <<< 'isinstance(x, y) or CRITICAL_CALL() or isinstance(x, z)'
isinstance(x, (y, z))
Found 1 error (1 fixed, 0 remaining).
```

SIM101 does not trigger an error on the above code, which is the correct behaviour.

They funnily enough also differ on whether they sort(?) the types:
```
$ source='isinstance(x, int) or isinstance(x, float)' 

$ ruff check --select=SIM101 --fix --unsafe-fixes - <<< $source                               
isinstance(x, (int, float))
Found 1 error (1 fixed, 0 remaining).

$ ruff check --select=PLR1701 --fix - <<< $source                                     
isinstance(x, (float, int))
Found 1 error (1 fixed, 0 remaining).
```

---

_Comment by @charliermarsh on 2024-03-06 15:08_

`PLR1701` should probably be removed.

---

_Comment by @jakkdl on 2024-03-06 15:12_

I'll also try to document the remaining autofixes in shed, several of which has no checks in other flake8 plugins, so ruff can consider adding them as checks and potentially fixes without having to dig straight into the source code.

---

_Comment by @charliermarsh on 2024-03-06 15:12_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-03-06 19:27_

---

_Comment by @jakkdl on 2024-03-08 15:42_

Quick draft: https://github.com/jakkdl/shed/blob/use_ruff/CODEMODS.md

---

_Referenced in [astral-sh/ruff#11616](../../astral-sh/ruff/issues/11616.md) on 2024-05-30 09:56_

---
