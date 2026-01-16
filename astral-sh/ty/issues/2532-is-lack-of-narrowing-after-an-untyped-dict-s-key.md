```yaml
number: 2532
title: "Is lack of narrowing after an untyped dict's key is set expected?"
type: issue
state: closed
author: sitsofe
labels:
  - question
assignees: []
created_at: 2026-01-16T14:22:39Z
updated_at: 2026-01-16T16:14:47Z
url: https://github.com/astral-sh/ty/issues/2532
synced_at: 2026-01-16T16:59:27Z
```

# Is lack of narrowing after an untyped dict's key is set expected?

---

_@sitsofe_

### Question

Hi,

In the following some type checkers narrow the type of `d` to `dict[str, int]` but `ty` does not:
```python
import typing

d = {}
d["s"] = 1
typing.reveal_type(d)
```

ty:
```
> ty check main.py
info[revealed-type]: Revealed type
 --> main.py:5:20
  |
3 | d = {}
4 | d["s"] = 1
5 | typing.reveal_type(d)
  |                    ^ `dict[Unknown, Unknown]`
  |

Found 1 diagnostic
```

pyright:
```
> pyright main.py
/private/tmp/main.py
  /private/tmp/main.py:5:20 - information: Type of "d" is "dict[Unknown, Unknown]"
0 errors, 0 warnings, 1 information
```

mypy:
```
> mypy main.py
main.py:5: note: Revealed type is "builtins.dict[builtins.str, builtins.int]"
Success: no issues found in 1 source file
```

pyrefly:
```
> pyrefly check main.py
 INFO revealed type: dict[str, int] (_["s"]: Literal[1]) [reveal-type]
 --> main.py:5:19
  |
5 | typing.reveal_type(d)
  |                   ---
  |
 INFO 0 errors
```

As I lack the proper vocabulary to describe the desired behaviour (gradual type narrowing of a `dict`?) done by mypy and pyrefly I struggled to to find what I was looking for in the existing issues. Is this just another case of #1248 ?

(Curiously, asking for the type before a key is set changes the revealed type reported for the `d` by pyrefly after the key is set:
```python
import typing

d = {}
typing.reveal_type(d)
d["s"] = 1
typing.reveal_type(d)
```

```
 INFO revealed type: dict[@_, @_] [reveal-type]
 --> main.py:4:19
  |
4 | typing.reveal_type(d)
  |                   ---
  |
 INFO revealed type: dict[Unknown, Unknown] (_["s"]: Literal[1]) [reveal-type]
 --> main.py:6:19
  |
6 | typing.reveal_type(d)
  |                   ---
  |
 INFO 0 errors
```
but perhaps all bets are off when you don't explicitly type code...
)

### Version

ty 0.0.12 (4b74e4ded 2026-01-14)

---

_Label `question` added by @sitsofe on 2026-01-16 14:22_

---

_Renamed from "Is lack of narrowing after dict's key is set expected?" to "Is lack of narrowing after an untyped dict's key is set expected?" by @sitsofe on 2026-01-16 14:23_

---

_Comment by @carljm on 2026-01-16 16:14_

What pyrefly and mypy are doing here is not really "type narrowing" in the usual sense, as you observed with pyrefly. Rather they are looking forward to the first item inserted into an empty dictionary in order to infer a type for it right "from the start".

We have similar plans, see #1473 

---

_Closed by @carljm on 2026-01-16 16:14_

---
