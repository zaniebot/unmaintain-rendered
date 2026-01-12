```yaml
number: 22528
title: "invalid syntax error with something that looks like a `match` statement"
type: issue
state: open
author: KotlinIsland
labels:
  - bug
  - parser
assignees: []
created_at: 2026-01-12T08:42:37Z
updated_at: 2026-01-12T08:54:48Z
url: https://github.com/astral-sh/ruff/issues/22528
synced_at: 2026-01-12T15:54:58Z
```

# invalid syntax error with something that looks like a `match` statement

---

_@KotlinIsland_

### Summary

```py
from re import *

def main(x, y, z):
    match [x, y, z]:                        {   #<--- load bearing brace
        case [0, 1, 2]: print("a"),
        case [3, 4, 5]: print("b"),
        case [_, _, _]: exit(1)
                                            }   #<--- load bearing brace
    print("all good")

main(4, 2, 0)
```
[playground](https://play.ruff.rs/516ccd06-b6b5-436a-8dd7-a86420a746c1)

`Expected newline, found `{` (invalid-syntax) [Ln 4, Col 45]`

finders credit to @decorator-factory 

<details><summary>Solution</summary>
<p>

```py
from re import match

match[0]: int  # Expected newline, found name(invalid-syntax)
```

</p>
</details> 

---

_Comment by @MichaReiser on 2026-01-12 08:50_

wow... 

In case anyone else wonders as what this is supposed to parse. It's an annotated assignment (the target is a subscript expression)

```
Module(
   body=[
      ImportFrom(
         module='re',
         names=[
            alias(name='*')],
         level=0),
      FunctionDef(
         name='main',
         args=arguments(
            args=[
               arg(arg='x'),
               arg(arg='y'),
               arg(arg='z')]),
         body=[
            AnnAssign(
               target=Subscript(
                  value=Name(id='match', ctx=Load()),
                  slice=Tuple(
                     elts=[
                        Name(id='x', ctx=Load()),
                        Name(id='y', ctx=Load()),
                        Name(id='z', ctx=Load())],
                     ctx=Load()),
                  ctx=Store()),
               annotation=Dict(
                  keys=[
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Constant(value=0),
                              Constant(value=1),
                              Constant(value=2)],
                           ctx=Load()),
                        ctx=Load()),
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Constant(value=3),
                              Constant(value=4),
                              Constant(value=5)],
                           ctx=Load()),
                        ctx=Load()),
                     Subscript(
                        value=Name(id='case', ctx=Load()),
                        slice=Tuple(
                           elts=[
                              Name(id='_', ctx=Load()),
                              Name(id='_', ctx=Load()),
                              Name(id='_', ctx=Load())],
                           ctx=Load()),
                        ctx=Load())],
                  values=[
                     Call(
                        func=Name(id='print', ctx=Load()),
                        args=[
                           Constant(value='a')]),
                     Call(
                        func=Name(id='print', ctx=Load()),
                        args=[
                           Constant(value='b')]),
                     Call(
                        func=Name(id='exit', ctx=Load()),
                        args=[
                           Constant(value=1)])]),
               simple=0),
            Expr(
               value=Call(
                  func=Name(id='print', ctx=Load()),
                  args=[
                     Constant(value='all good')]))]),
      Expr(
         value=Call(
            func=Name(id='main', ctx=Load()),
            args=[
               Constant(value=4),
               Constant(value=2),
               Constant(value=0)]))])

```

---

_Label `bug` added by @MichaReiser on 2026-01-12 08:51_

---

_Label `parser` added by @MichaReiser on 2026-01-12 08:51_

---
