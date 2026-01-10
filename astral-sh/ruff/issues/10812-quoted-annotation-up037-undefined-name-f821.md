```yaml
number: 10812
title: "`quoted-annotation` (`UP037`), `undefined-name` (`F821`) & `quoted-type-alias` (`TC008`) - false positives on pyright inlined `TypedDict`s"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - linter
assignees: []
created_at: 2024-04-07T12:14:05Z
updated_at: 2024-12-30T09:34:57Z
url: https://github.com/astral-sh/ruff/issues/10812
synced_at: 2026-01-10T11:09:53Z
```

# `quoted-annotation` (`UP037`), `undefined-name` (`F821`) & `quoted-type-alias` (`TC008`) - false positives on pyright inlined `TypedDict`s

---

_Issue opened by @DetachHead on 2024-04-07 12:14_

pyright supports defining a `TypedDict` inline like so:

```py
from __future__ import annotations
from typing import TypedDict

foo: TypedDict[{"bar": str}] = {"bar": "bar"} # errors: F821, UP037
```

removing the quotes causes the following pyright error:

```py
foo: TypedDict[{bar: str}] = {"bar": "bar"} # error: Expected string literal for dictionary entry name
```
[playground](https://play.ruff.rs/ad657c55-d2c6-4be5-ade7-63212003dba0)

---

_Label `bug` added by @AlexWaygood on 2024-04-07 12:16_

---

_Label `linter` added by @AlexWaygood on 2024-04-07 12:16_

---

_Renamed from "`quoted-annotation` (`UP037`) - false positive on pyright inlined `TypedDict`s" to "`quoted-annotation` (`UP037`) & `undefined-name` (`F821`) - false positives on pyright inlined `TypedDict`s" by @DetachHead on 2024-04-07 12:17_

---

_Comment by @AlexWaygood on 2024-04-07 12:22_

Thanks, I agree that ruff's behaviour is incorrect here.

For other readers who may not be familiar — this is an experimental syntax for "inline TypedDicts" that has been floated on the (now-defunct) typing-sig mailing list. Pyright has added support for this experimental syntax so that people can experiment with it. Some more details here: https://github.com/python/typing/discussions/1391.

I don't believe a PEP has yet been written to formally propose this new way of writing TypedDicts. But without this new syntax, it wouldn't be a valid PEP-484 type annotation anyway, so regardless I think ruff probably shouldn't be telling people to remove the quotes in that type annotation.

---

_Comment by @DetachHead on 2024-12-03 06:30_

fyi there's now a draft PEP for this: https://github.com/python/peps/pull/4082

---

_Comment by @DetachHead on 2024-12-04 22:51_

also happens with the new `quoted-type-alias` (`TC008`) rule in 0.8.1:

```py
from typing import TypedDict

type Foo = TypedDict[{"Bar": str}] # error: Remove quotes from type alias
``` 

---

_Renamed from "`quoted-annotation` (`UP037`) & `undefined-name` (`F821`) - false positives on pyright inlined `TypedDict`s" to "`quoted-annotation` (`UP037`), `undefined-name` (`F821`) & quoted-type-alias` (`TC008`) - false positives on pyright inlined `TypedDict`s" by @DetachHead on 2024-12-04 22:51_

---

_Renamed from "`quoted-annotation` (`UP037`), `undefined-name` (`F821`) & quoted-type-alias` (`TC008`) - false positives on pyright inlined `TypedDict`s" to "`quoted-annotation` (`UP037`), `undefined-name` (`F821`) & `quoted-type-alias` (`TC008`) - false positives on pyright inlined `TypedDict`s" by @DetachHead on 2024-12-04 22:51_

---

_Closed by @dhruvmanila on 2024-12-30 09:34_

---

_Closed by @dhruvmanila on 2024-12-30 09:34_

---
