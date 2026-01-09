---
number: 7760
title: "AST Improvement: Store dotted name parts"
type: issue
state: open
author: konstin
labels:
  - parser
assignees: []
created_at: 2023-10-02T09:50:01Z
updated_at: 2023-10-27T02:54:44Z
url: https://github.com/astral-sh/ruff/issues/7760
synced_at: 2026-01-07T13:12:15-06:00
---

# AST Improvement: Store dotted name parts

---

_Issue opened by @konstin on 2023-10-02 09:50_

Currently, the path of imports is not formatted, e.g.
```python
import a . b
```
remains as-is. This is due to a bug in our AST:

https://github.com/astral-sh/ruff/blob/6824b67f44d8d462f83a727ed8caf100d10c22a6/crates/ruff_python_ast/src/imports.rs#L6-L31

The entire path is represented as a single string, even though it should be dot-separated identifier (the parser calls it `DottedName`, but then emits an identifier), especially since identifier can not contain dots.

- [ ] Fix the AST so the import path is a `Vec<Identifier>` or something similar
- [x] Format import paths by removing whitespace (we can't insert parentheses like we do for dotted expressions)
- [ ] Check that `Identifier` is only used for strings matching the rules

---

_Label `formatter` added by @konstin on 2023-10-02 09:50_

---

_Referenced in [astral-sh/ruff#7744](../../astral-sh/ruff/pulls/7744.md) on 2023-10-02 09:51_

---

_Label `formatter` removed by @charliermarsh on 2023-10-02 13:51_

---

_Label `parser` added by @charliermarsh on 2023-10-02 13:51_

---

_Comment by @charliermarsh on 2023-10-02 13:52_

I think the counterargument is that CPython uses this representation:

```python
>>> print(ast.dump(ast.parse(s)))
Module(body=[Import(names=[alias(name='foo.bar')])], type_ignores=[])
```

And the ASDL uses the same `identifier` symbol as in other nodes: https://docs.python.org/3/library/ast.html.

But I care more about the ergonomics and performance than I do exact compatibility with CPython on these decisions. Would need to see what this makes easier or harder.

---

_Referenced in [astral-sh/ruff#7859](../../astral-sh/ruff/pulls/7859.md) on 2023-10-09 08:18_

---

_Comment by @MichaReiser on 2023-10-16 07:44_

@charliermarsh Is my understanding correct that CPython normalizes the identifier name (removes the whitespace)? 

I was a bit surprised when I saw this representation first because it's somewhat uncommon (at least in the languages that I have used thus far)—especially considering that it re-joins the identifier tokens that the lexer identified. 

Are there any upsides in the semantic model to have a single string? I would expect it to be easier to have the individual parts when e.g. resolving imports. The alternative is that we implement a `components` method similar to Rust's `Path::components` that returns the individual parts (splitting by string). 

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-16 07:44_

---

_Comment by @charliermarsh on 2023-10-16 13:35_

Yeah it's probably an improvement to store a list of dot-separated segments. There is likely no upside in the semantic model since we always decompose into segments.

---

_Renamed from "Fix the AST representation of import paths and format imports" to "Format import paths" by @MichaReiser on 2023-10-27 02:11_

---

_Label `parser` removed by @MichaReiser on 2023-10-27 02:11_

---

_Label `formatter` added by @MichaReiser on 2023-10-27 02:11_

---

_Comment by @MichaReiser on 2023-10-27 02:12_

Let us fix this, regardless of how it gets implemented. Splitting the names in the formatter would be silly, but something we could do. 

I think we're at least lucky that the following is not valid

```python
import (a # comment
   .b)
```

---

_Comment by @charliermarsh on 2023-10-27 02:17_

I think this actually was fixed, we format it correctly: https://play.ruff.rs/86d1a181-4d27-4bbe-ad13-0c425e1976c0. I think this issue was about changing the AST to better reflect the real structure.

---

_Renamed from "Format import paths" to "AST Improvement: Store dotted name parts" by @MichaReiser on 2023-10-27 02:28_

---

_Label `formatter` removed by @MichaReiser on 2023-10-27 02:28_

---

_Label `formatter` added by @MichaReiser on 2023-10-27 02:28_

---

_Label `parser` added by @MichaReiser on 2023-10-27 02:28_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-10-27 02:28_

---

_Label `formatter` removed by @MichaReiser on 2023-10-27 02:54_

---
