```yaml
number: 16544
title: "[syntax-errors] Star expression in index before Python 3.11"
type: pull_request
state: merged
author: ntBre
labels:
  - parser
  - preview
assignees: []
merged: true
base: main
head: brent/syn-star-index
created_at: 2025-03-06T21:36:28Z
updated_at: 2025-03-14T14:57:11Z
url: https://github.com/astral-sh/ruff/pull/16544
synced_at: 2026-01-10T19:49:01Z
```

# [syntax-errors] Star expression in index before Python 3.11

---

_Pull request opened by @ntBre on 2025-03-06 21:36_

Summary
--

This PR detects tuple unpacking expressions in index/subscript expressions before Python 3.11.

Test Plan
--

New inline tests

---

_Label `parser` added by @ntBre on 2025-03-06 21:36_

---

_Label `preview` added by @ntBre on 2025-03-06 21:36_

---

_Review requested from @MichaReiser by @ntBre on 2025-03-06 21:36_

---

_Review requested from @dhruvmanila by @ntBre on 2025-03-06 21:36_

---

_Comment by @github-actions[bot] on 2025-03-06 21:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/error.rs`:700 on 2025-03-07 09:08_

Nit: Maybe *in index operations* to align with the terminology used in the PEP

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@star_index_py310.py.snap`:361 on 2025-03-07 09:11_

I'm not sure this is correct. I haven't checked if it is a parse or semantic error but Python 3.12 gives me:

```
>>> [][*list]
<python-input-1>:1: SyntaxWarning: list indices must be integers or slices, not tuple; perhaps you missed a comma?
  [][*list]
Traceback (most recent call last):
  File "<python-input-1>", line 1, in <module>
    [][*list]
    ~~^^^^^^^
TypeError: list indices must be integers or slices, not tuple
```

Which is also what I read from the grammar change: It only allows star expressions for slices except the first

```
slices:
    | slice !','
    | ','.(slice | starred_expression)+ [',']
```

---

_@MichaReiser reviewed on 2025-03-07 09:11_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:884 on 2025-03-07 10:20_

As mentioned by Micha, I don't think this is the correct place for this check.

As per the grammar, the parser converts a slice elements that's a starred expression into a single element tuple. This happens in the `else if` branch above. You can see this in the output AST as well.

I think this check would need to go in the `if` branch which is a pure tuple case and, it's only in that position where the grammar was changed.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@star_index_py310.py.snap`:361 on 2025-03-07 10:21_

Yeah, I think the check needs to be moved up in the `if` branch which is a pure tuple case.

This is a `TypeError` and is not relevant to the parser:
```console
$ bat _.py                         
         File: _.py
     1 ~ [][*data]

$ uv run --python=3.12 --no-project python -m ast _.py
Module(
   body=[
      Expr(
         value=Subscript(
            value=List(elts=[], ctx=Load()),
            slice=Tuple(
               elts=[
                  Starred(
                     value=Name(id='data', ctx=Load()),
                     ctx=Load())],
               ctx=Load()),
            ctx=Load()))],
   type_ignores=[])
```

---

_@dhruvmanila requested changes on 2025-03-07 10:21_

---

_@ntBre reviewed on 2025-03-07 12:48_

---

_Review comment by @ntBre on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@star_index_py310.py.snap`:361 on 2025-03-07 12:48_

I may have made this test case a bit misleading by naming the container `lst`, but as @dhruvmanila points out with the valid AST, this is *syntactically* valid, and it's *not* a type error for a container like a numpy array or another container that can be validly indexed by a tuple:

```pycon
>>> import numpy as np
>>> a = np.array([[1, 2, 3], [4, 5, 6]])
>>> t = [1, 2]
>>> a[*t]
np.int64(6)
```

It's not a parser or compiler error, it's only a runtime `TypeError` if the container doesn't support tuple indexing.

I believe the grammar in the [reference](https://docs.python.org/3/reference/expressions.html#slicings) also supports this, albeit with different terminology from the PEP:

```
slicing      ::= primary "[" slice_list "]"
slice_list   ::= slice_item ("," slice_item)* [","]
slice_item   ::= expression | proper_slice
proper_slice ::= [lower_bound] ":" [upper_bound] [ ":" [stride] ]
lower_bound  ::= expression
upper_bound  ::= expression
stride       ::= expression
```

`slice_item` is either an `expression` *or* a `proper_slice` with multiple parts.


---

_@dhruvmanila reviewed on 2025-03-10 16:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@star_index_py310.py.snap`:361 on 2025-03-10 16:06_

Sorry, I think I mis-understood the change here. I think what Brent has here is correct. The example and I and Micha have pointed out is a runtime error and should throw a syntax error for Python version older than 3.11.

---

_@dhruvmanila reviewed on 2025-03-10 16:10_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/expression.rs`:884 on 2025-03-10 16:10_

The existing implementation is correct. I think I misunderstood the change as to be only limited to indexing operations i.e., `data[0]` but a tuple expression used is _also_ an indexing operation (`data[0, 1]`).

---

_@dhruvmanila approved on 2025-03-10 16:11_

Looks good!

---

_Merged by @ntBre on 2025-03-14 14:51_

---

_Closed by @ntBre on 2025-03-14 14:51_

---

_Branch deleted on 2025-03-14 14:51_

---
