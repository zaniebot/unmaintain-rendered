```yaml
number: 7633
title: Invalid python files, that ruff parse
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-24T18:30:48Z
updated_at: 2025-04-22T13:43:48Z
url: https://github.com/astral-sh/ruff/issues/7633
synced_at: 2026-01-10T11:09:49Z
```

# Invalid python files, that ruff parse

---

_Issue opened by @qarmin on 2023-09-24 18:30_

Ruff 0.0.291
Cpython 3.11.4

Tested with 
```
ruff format . --check
```
and cpython part
```
python -m compileall .
```
https://github.com/qarmin/Automated-Fuzzer/releases/download/test/Cpython.no.parsable.parsable.by.ruff.zip

This makes difficult to report fuzzer problems, because I cannot with only ruff recognize if this is valid python file or not


Example of files
```
*f
```
```
break
```
```
yield #
```
```
#coding:ut
```
```
from pydanic import BaseModel


class HostedResult(BaseModel):
    id: str
  url: str
```

Example of cpython errors
```
    *Ï
    ^^^
SyntaxError: can't use starred expression here
```
```
*** Sorry: IndentationError: unindent does not match any outer indentation level (98_IDX_0_RAND_10049945704112701812.py, line 45)
```
```
    grouping() = "Right"
    ^^^^^^^^^^
SyntaxError: cannot assign to function call here. Maybe you meant '==' instead of '='?
```
```
***   File "./990_IDX_0_RAND_14720601008565103604.py", line 107
    self.u<= a.rSDP = None
    ^^^^^^^^^^^^^^^
SyntaxError: cannot assign to comparison
```
```
SyntaxError: unknown encoding: u
```
```
    'id': (str,await  none_type,),  # noqa: E501
               ^^^^^^^^^^^^^^^^
SyntaxError: 'await' outside async function
```
```
    from __future__ import absolute_im
    ^
SyntaxError: future feature absolute_im is not defined
```

---

_Comment by @dhruvmanila on 2023-09-25 04:41_

Related https://github.com/astral-sh/ruff/issues/6895

---

_Label `bug` added by @charliermarsh on 2023-09-25 14:08_

---

_Comment by @qarmin on 2023-09-29 07:22_

List of all cpython errors visible in files in first post 
```
SyntaxError: 'ascii' codec can't decode byte 0xc2 in position 16: ordinal not in range(128)
SyntaxError: 'async for' outside async function
SyntaxError: 'async with' outside async function
SyntaxError: 'await expression' can not be used within an annotation
SyntaxError: 'await' outside async function
SyntaxError: 'await' outside function
SyntaxError: 'break' outside loop
SyntaxError: 'comparison' is an illegal expression for augmented assignment
SyntaxError: 'continue' not properly in loop
SyntaxError: 'expression' is an illegal expression for augmented assignment
SyntaxError: 'list' is an illegal expression for augmented assignment
SyntaxError: 'literal' is an illegal expression for augmented assignment
SyntaxError: 'return' outside function
SyntaxError: 'rot13' is not a text encoding; use codecs.decode() to handle arbitrary codecs
SyntaxError: 'tuple' is an illegal expression for augmented assignment
SyntaxError: 'utf7' codec can't decode byte 0xc2 in position 22: unexpected special character
SyntaxError: 'yield from' inside async function
SyntaxError: 'yield' inside generator expression
SyntaxError: 'yield' outside function
SyntaxError: Generator expression must be parenthesized
SyntaxError: asynchronous comprehension outside of an asynchronous function
SyntaxError: can't use starred expression here
SyntaxError: cannot assign to False
SyntaxError: cannot assign to None
SyntaxError: cannot assign to True
SyntaxError: cannot assign to attribute here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to await expression
SyntaxError: cannot assign to await expression here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to comparison
SyntaxError: cannot assign to expression
SyntaxError: cannot assign to expression here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to function call
SyntaxError: cannot assign to function call here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to lambda
SyntaxError: cannot assign to literal
SyntaxError: cannot assign to literal here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to set display here. Maybe you meant '==' instead of '='?
SyntaxError: cannot assign to subscript here. Maybe you meant '==' instead of '='?
SyntaxError: cannot delete function call
SyntaxError: cannot delete literal
SyntaxError: default 'except:' must be last
SyntaxError: encoding problem: utI-8 with BOM
SyntaxError: encoding problem: utf-int with BOM
SyntaxError: expected 'else' after 'if' expression
SyntaxError: f-string: cannot use starred expression here
SyntaxError: from __future__ imports must occur at the beginning of the file
SyntaxError: future feature Faimportlseannotations is not defined
SyntaxError: illegal target for annotation
SyntaxError: import * only allowed at module level
SyntaxError: invalid character '¦' (U+00A6)
SyntaxError: invalid non-printable character U+0084
SyntaxError: invalid syntax
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
SyntaxError: iterable unpacking cannot be used in comprehension
SyntaxError: multiple starred expressions in assignment
SyntaxError: name 'LOG' is used prior to global declaration
SyntaxError: no binding for nonlocal 'card_st' found
SyntaxError: nonlocal declaration not allowed at module level
SyntaxError: not a chance
SyntaxError: starred assignment target must be in a list or tuple
SyntaxError: too many statically nested blocks
SyntaxError: unexpected EOF while parsing
SyntaxError: unknown encoding: -t1-8
SyntaxError: unterminated string literal (detected at line 13)
SyntaxError: wildcard makes remaining patterns unreachable
```

---

_Comment by @MichaReiser on 2024-04-04 09:09_

@dhruvmanila this could be an interesting source for obscure parser tests ;)

---

_Comment by @dhruvmanila on 2024-04-04 09:36_

Interesting! I think a lot of [this messages](https://github.com/astral-sh/ruff/issues/7633#issuecomment-1740424031) shouldn't really be handled by the parser. Basically, a lot of them are soft syntax error which means they're not raised at the parsing step. For example, `'async with' outside async function`, `nonlocal declaration not allowed at module level`, `'break' outside loop`, etc. But, this would be useful in the next phase to emit these errors as diagnostics.

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-11 11:14_

---

_Comment by @dhruvmanila on 2025-04-22 13:43_

This is superseded by #17412 which captures all the remaining syntax errors from the ones mentioned here. Thank you for running the fuzzer and collecting these errors!

---

_Closed by @dhruvmanila on 2025-04-22 13:43_

---
