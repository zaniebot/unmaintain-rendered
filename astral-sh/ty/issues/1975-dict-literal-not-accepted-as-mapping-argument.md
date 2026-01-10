```yaml
number: 1975
title: "`dict` literal not accepted as `Mapping` argument"
type: issue
state: closed
author: tibdex
labels: []
assignees: []
created_at: 2025-12-17T05:02:41Z
updated_at: 2025-12-17T13:39:53Z
url: https://github.com/astral-sh/ty/issues/1975
synced_at: 2026-01-10T01:55:00Z
```

# `dict` literal not accepted as `Mapping` argument

---

_Issue opened by @tibdex on 2025-12-17 05:02_

### Summary

In:

```python
from collections.abc import Mapping
from typing import Literal

type DataType = Literal["BOOL", "INT", "FLOAT"]

def process(data_types: Mapping[str, DataType], /) -> None:
    ...

process({"foo":"BOOL"})
```

I would expect the type checker to see that `{"foo":"BOOL"}` conforms to `Mapping[str, DataType]` when it is declared, and since it is not assigned anywhere, it cannot be mutated in a way that would modify its type before being passed as an argument.

However, I get:

```
Argument to function `process` is incorrect: Expected `Mapping[str, DataType]`, found `dict[Unknown | str, Unknown | str]` (invalid-argument-type) [Ln 9, Col 9]
```

[TypeScript 5.9.3 behaves like I expect](https://www.typescriptlang.org/play/?#code/C4TwDgpgBAIghsOAVc0C8UBEAhA8rgGUygB8sBJAOSWLMwDEDcBBGgKDYGMB7AOwGdgUMACdunCP35QMACgAmCZKn4AuKACUIceXwA2IADwBvANq84AWwirBIgJa8A5gF118RCkgBfAHwBKGV8oY28Abg5RcUl+WWNuAGt1HHwib382KIkpON5uYFwkrBhWAFFMdI4eASF+bmtcACMAKwhOIQx4opTCCoismNk6hpa24H8woA):

```typescript
type DataType = "BOOL" | "INT" | "FLOAT"

const process = (dataTypes: Readonly<{[name:string]: DataType}>) => {};

// This is accepted, but ty rejects it :(
process({ok: "BOOL"})

// Type '"DATE"' is not assignable to type 'DataType'
process({notOk: "DATE"})

const someObject = {ok: "BOOL"};
// Argument of type '{ ok: string; }' is not assignable to parameter of type // 'Readonly<{ [name: string]: DataType; }>'.
//   Property 'ok' is incompatible with index signature.
//    Type 'string' is not assignable to type 'DataType'.
process(someObject);
```

### Version

0.0.2.

---

_Comment by @AlexWaygood on 2025-12-17 07:02_

Thanks! Please see #1576. We're working on solutions to this :-)

---

_Closed by @AlexWaygood on 2025-12-17 07:02_

---

_Comment by @tibdex on 2025-12-17 13:39_

Thanks

---
