```yaml
number: 1252
title: "ty infers literals and fails `assert_type`, opposed to other type checkers"
type: issue
state: closed
author: Bobronium
labels:
  - needs-info
assignees: []
created_at: 2025-09-25T12:23:23Z
updated_at: 2025-10-01T07:10:07Z
url: https://github.com/astral-sh/ty/issues/1252
synced_at: 2026-01-10T02:06:25Z
```

# ty infers literals and fails `assert_type`, opposed to other type checkers

---

_Issue opened by @Bobronium on 2025-09-25 12:23_

### Alternative title: 
## `ty` is superior at interpreting type variables and it's causing problems

```py
from typing_extensions import Literal, assert_type, reveal_type


def as_is[T](x: T) -> T:
    return x


reveal_type(as_is(1))  # `Literal[1]` with ty, `int` with other typecheckers
assert_type(as_is(1), Literal[1])  # ok with ty, error with other typecheckers
assert_type(as_is(1), int)  # error with ty: Inferred type of argument is `Literal[1]`

a: Literal[1] = 1
assert_type(a, Literal[1])  # here all typecheckers pass
assert_type(a, int)  # here all typecheckers fail
```

Other typecheckers:
- mypy
- pyright
- pyrefly
- zuban
- PyCharm

<details>
<summary>Output </summary>

```shell

❯ ty check expression.py  --output-format concise
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                      
expression.py:8:13: info[revealed-type] Revealed type: `Literal[1]`
expression.py:10:1: error[type-assertion-failure] Argument does not have asserted type `int`
expression.py:14:1: error[type-assertion-failure] Argument does not have asserted type `int`
Found 3 diagnostics


❯ mypy expression.py                       
expression.py:8: note: Revealed type is "builtins.int"
expression.py:9: error: Expression is of type "int", not "Literal[1]"  [assert-type]
expression.py:14: error: Expression is of type "Literal[1]", not "int"  [assert-type]
Found 2 errors in 1 file (checked 1 source file)

❯ pyright expression.py                     
expression.py
  expression.py:8:13 - information: Type of "as_is(1)" is "int"
  expression.py:9:13 - error: "assert_type" mismatch: expected "Literal[1]" but received "int" (reportAssertTypeFailure)
  expression.py:14:13 - error: "assert_type" mismatch: expected "int" but received "Literal[1]" (reportAssertTypeFailure)
2 errors, 0 warnings, 1 information


❯ pyrefly check expression.py  --output-format min-text
 INFO expression.py:8:12-22: revealed type: int [reveal-type]
ERROR expression.py:9:12-34: assert_type(int, Literal[1]) failed [assert-type]
ERROR expression.py:14:12-20: assert_type(Literal[1], int) failed [assert-type]
 INFO 2 errors


❯ zuban check expression.py --no-pretty     
expression.py:8: note: Revealed type is "builtins.int"
expression.py:9: error: Expression is of type "int", not "Literal[1]"  [misc]
expression.py:14: error: Expression is of type "Literal[1]", not "int"  [misc]
Found 2 errors in 1 file (checked 1 source file)
```

</details>

I'm not sure what the best course of action here would be. 

Arguably, `assert_type(Literal[1], int)` should pass, but that's not behavior of other typecheckers either.


### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Renamed from "ty expects literals in `assert_type`, opposed to other type checkers" to "ty infers literals and fails `assert_type`, opposed to other type checkers" by @Bobronium on 2025-09-25 12:24_

---

_Comment by @AlexWaygood on 2025-09-25 12:34_

Could you elaborate on exactly what the problems are that our behaviour here is causing you? Bug-for-bug compatibility with other type checkers is not a goal for this project. There are already many areas in which an `assert_type()` call will pass for one of mypy and pyright but not for the other, and there will be some areas in which similar things are true for ty as well

---

_Label `needs-info` added by @AlexWaygood on 2025-09-26 09:53_

---

_Comment by @AlexWaygood on 2025-10-01 07:10_

Closing for now. We'll probably get closer to the behaviour of other type checkers here once we've finished our new constraint solver, anyway 

---

_Closed by @AlexWaygood on 2025-10-01 07:10_

---
