```yaml
number: 663
title: "TypedDict unpacking (**data) incorrectly flagged as type error when parameters match TypedDict fields"
type: issue
state: closed
author: FabianVegaA
labels: []
assignees: []
created_at: 2025-06-16T18:56:22Z
updated_at: 2025-06-17T01:46:37Z
url: https://github.com/astral-sh/ty/issues/663
synced_at: 2026-01-10T02:08:20Z
```

# TypedDict unpacking (**data) incorrectly flagged as type error when parameters match TypedDict fields

---

_Issue opened by @FabianVegaA on 2025-06-16 18:56_

### Summary

When unpacking a TypedDict instance using the `**` operator into a function whose parameters exactly match the TypedDict's field types, the type checker incorrectly reports a type error. This happens despite the structural compatibility between the TypedDict definition and the function signature.

Example:
```python
from typing import TypedDict

class Foo(TypedDict):
    bar: int
    baz: str
    qux: float
    quux: tuple[int]

def do_with_foo(
    bar: int,
    baz: str,
    qux: float,
    quux: tuple[int],
) -> int:
    raise NotImplemented

data = Foo(bar=1, baz="1", qux=1.1, quux=(1,))
result = do_with_foo(**data) # Type checker incorrectly flags this line with error
```

The error marked is **"No arguments provided for required parameters `bar`, `baz`, `qux`, `quux` of function `do_with_foo` (ty missing-argument)"**


I'm using Zed integration. This is my configuration:
```json
{
"languages": {
    "Python": {
      "language_servers": ["ty"]
    }
  },
  "lsp": {
    "ty": {
      "binary": {
        "path": "/path/to/bin/ty",
        "arguments": ["server"]
      }
    }
}
```

### Version

ty 0.0.1-alpha.10 (15bae14c2 2025-06-13)

---

_Comment by @abhijeetbodas2001 on 2025-06-16 19:19_

Related: https://github.com/astral-sh/ty/issues/154

---

_Comment by @carljm on 2025-06-17 01:46_

Also related: https://github.com/astral-sh/ty/issues/247

Thanks for the report, but since this is a combination of two not-yet-supported features (TypedDict and splatted calls), I'm going to mark it as duplicate; if it still doesn't work after the above two issues are closed, then there may be a bug in one of the implementations of those features.

---

_Closed by @carljm on 2025-06-17 01:46_

---
