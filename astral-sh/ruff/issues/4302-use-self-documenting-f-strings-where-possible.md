```yaml
number: 4302
title: Use self-documenting f-strings where possible
type: issue
state: open
author: janosh
labels:
  - rule
assignees: []
created_at: 2023-05-09T04:04:11Z
updated_at: 2023-05-10T16:02:30Z
url: https://github.com/astral-sh/ruff/issues/4302
synced_at: 2026-01-12T15:54:44Z
```

# Use self-documenting f-strings where possible

---

_@janosh_

Could maybe be added to `flake8-simplify`.

```py
my_var = 42_000

# bad (if target=py3.8+)
f"my_var={my_var}"
f"my_var = {my_var}"
f"my_var = {my_var:,}"

# good
f"{my_var=}"
f"{my_var = }"
f"{my_var = :,}"
```

---

_Comment by @ngnpope on 2023-05-09 08:14_

This would be nice, and would help make those who don't know this feature aware of it 🙂 

The only snag is that f-strings are not part of the formal grammar until Python 3.12 - see [PEP 701](https://peps.python.org/pep-0701/) - so they're tricky to do much with right now.

Other test cases to include for unbalanced spacing:
```python
# bad (if target=py3.8+)
f"my_var= {my_var}"
f"my_var ={my_var}"

# good
f"{my_var= }"
f"{my_var =}"
```

---

_Comment by @akx on 2023-05-09 10:07_

I think this is a `RUF` rule or `FLY` (recently introduced in #4196, see https://github.com/ikamensh/flynt) rule, but sounds like a good idea :) 

---

_Comment by @akx on 2023-05-09 10:24_

Ah, this is made difficult by the fact that ruffpython-ast apparently already parses `f"{my_var=}"` as 

```
[
  Located { range: 119..131, custom: (), node: Constant { value: Str("my_var="), kind: None } },
  Located { range: 119..131, custom: (), node: FormattedValue {
    value: Located { range: 122..128, custom: (), node: Name { id: "my_var", ctx: Load } },
    conversion: 114,
    format_spec: None
  }}
]
```
so it's not easy to distinguish between already correctly formed expressions and those that need changing 🤔 

---

_Label `rule` added by @charliermarsh on 2023-05-09 12:48_

---

_Comment by @janosh on 2023-05-09 13:36_

Tbh I'm out of my depth on AST and grammar. At the risk of going against orthodoxy and sounding low-status, if the grammar doesn't formalize f-strings below 3.12, maybe a regex can help distinguish true positives? 😄 

---

_Comment by @MichaReiser on 2023-05-10 14:45_

> Ah, this is made difficult by the fact that ruffpython-ast apparently already parses `f"{my_var=}"` as
> 
> ```
> [
>   Located { range: 119..131, custom: (), node: Constant { value: Str("my_var="), kind: None } },
>   Located { range: 119..131, custom: (), node: FormattedValue {
>     value: Located { range: 122..128, custom: (), node: Name { id: "my_var", ctx: Load } },
>     conversion: 114,
>     format_spec: None
>   }}
> ]
> ```
> 
> so it's not easy to distinguish between already correctly formed expressions and those that need changing thinking

The two nodes differ in that the second has `conversion`  set to `b'r'`. I don't know why `conversion` is an `usize` instead the `ConversionFlags` that RustPython uses internally or just the char (@youknowone do you know why?). But I think it could allow us to tell the two formats apart. 

https://github.com/charliermarsh/RustPython/blob/50b8570bc262098b84dac86092133d3ee26caf23/compiler/core/src/bytecode.rs#L328-L342
 



---

_Comment by @youknowone on 2023-05-10 16:00_

[Python.asdl](https://github.com/python/cpython/blob/v3.11.2/Parser/Python.asdl#L78) refers it as `int` and RustPython naively convert it to actual int. We didn't care about it that much because we were the sole ast user, but that need to be enhanced now.
There are 7 int values and 2 refers bool and another one is conversion. 
I recently changes bool values to actual bool and going to work on conversion too.
It will be `ConversionFlag` or  `Option<ConversionFlag>`. I didn't investigate enough which one is more correct.

---

_Comment by @youknowone on 2023-05-10 16:02_

About this or not, please feel free to suggest anything I can make ast better.

---
