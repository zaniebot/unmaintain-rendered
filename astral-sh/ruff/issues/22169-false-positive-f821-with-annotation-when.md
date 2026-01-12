```yaml
number: 22169
title: False positive F821 with Annotation when accessing dictionary
type: issue
state: open
author: brendan-morin
labels:
  - question
assignees: []
created_at: 2025-12-24T05:31:03Z
updated_at: 2025-12-24T21:01:44Z
url: https://github.com/astral-sh/ruff/issues/22169
synced_at: 2026-01-12T15:54:58Z
```

# False positive F821 with Annotation when accessing dictionary

---

_@brendan-morin_

### Summary

This bit
```python
from typing import Annotated

a = {
    "c": str,
}
Annotated[a["c"], str]
```
yields:
```
% uv run ruff check
F821 Undefined name `c`
 --> src/example.py:7:18
  |
4 |     "c": "cat",
5 | }
6 | Annotated[a["c"], str]
  |
```
However, as far as I can tell this should be valid.

### Version

_No response_

---

_Comment by @brendan-morin on 2025-12-24 05:34_

Some additional notes on `Annotated` for whenever this gets looked into, ruff also does not catch the following issues (from typing docs), but maybe that's more `ty`'s turf:
- `Annotated[a["c"]]` should fail: "It's an error to call `Annotated` with less than two arguments".
- `Annotated["a", str]` should fail: "The first argument to Annotated must be a valid type.

---

_Comment by @ntBre on 2025-12-24 15:24_

Thanks for the report! [ty](https://play.ty.dev/8344dcbb-1714-4b59-b867-51604b44cc43) is also reporting an invalid type form error on the `a["c"]` subscript, though, so I think this may be intentional?

I looked a bit at the [typing spec](https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions) and for any existing ty issues but someone like @AlexWaygood may know better whether this is correct.

I think https://github.com/astral-sh/ruff/issues/11378 may also be related on the Ruff side.

---

_Label `question` added by @ntBre on 2025-12-24 15:24_

---

_Comment by @AlexWaygood on 2025-12-24 16:27_

Yes, the first argument to `Annotated` is expected to be a valid "annotation expression" (which is a term of art described in https://typing.python.org/en/latest/spec/annotations.html#type-and-annotation-expressions). `a["c"]` is not a valid annotation expression, and that's what's causing the issue here. In annotation expressions, string-literal expressions are expected to be "forward references" to names not-yet defined, which is why Ruff is emitting the "undefined name" F821 error here: there's no `c` variable defined anywhere in this module. The only place where a string literal inside an annotation expression is _not_ expected to be a forward reference is if it's inside a `typing.Literal` slice: `typing.Literal["c"]`, etc.

---

_Comment by @AlexWaygood on 2025-12-24 16:28_

https://github.com/astral-sh/ruff/issues/11378 seems a bit different to me -- that issue is about arguably valid annotations that Ruff doesn't understand (because it doesn't have type inference). However, the annotation in this issue is invalid according to the typing spec, unfortunately.

---

_Comment by @brendan-morin on 2025-12-24 20:57_

I see what you are saying, but I'm not sure I agree that `a["c"]` actually represents a string literal , and as such I don't think it need be a forward referenced. As far as `Annotated[]` is concerned, it is only receiving the resolved output of the alias `a["c"]` and not the key internal to that alias itself (unless type inference skips interpretation, that I really don't know much about). 

If invalid, this has some pretty weird implications, such as `Annotated[a["c"], str]` being invalid but `Annotated[a.keys()[0], str]` being somehow more valid. Also in testing, I found that `Annotated[a["a"], str]` is considered by ruff to be valid, despite being somewhat silly and just random chance that a dictionary key matches a scoped variable name that is very clearly is not actually referencing.

FWIW there are no issues with python actually interpreting this, and in practice this type is "load bearing" in the way pydantic types tend to be so it's not just a pure hint. So we can confirm the reference method is considered valid enough to the interpreter. I'd suspect because it works we would see enough of this in the wild for it to be considered the defacto API contract that couldn't at this point be broken without a deprecation plan.

Anyhow I don't have strong opinions on what it _should_ be, if your interpretation is unambiguously the spec then it is what it is.

---
