```yaml
number: 1492
title: Optional Typed dict keys are marked as required.
type: issue
state: closed
author: lypwig
labels:
  - question
assignees: []
created_at: 2025-11-06T07:36:45Z
updated_at: 2025-11-06T13:51:20Z
url: https://github.com/astral-sh/ty/issues/1492
synced_at: 2026-01-10T02:06:25Z
```

# Optional Typed dict keys are marked as required.

---

_Issue opened by @lypwig on 2025-11-06 07:36_

### Summary

Optional keys in a typed dictionary are marked as required.

```python
from typing import TypedDict, Optional

class MyTypedDict1(TypedDict):
    aaa: int
    bbb: Optional[list[str]]


class MyTypedDict2(TypedDict):
    aaa: int
    bbb: list[str] | None

d1: MyTypedDict1 = {
    "aaa": 42
}

d2: MyTypedDict2 = {
    "aaa": 42
}
```

> Missing required key 'bbb' in TypedDict `MyTypedDict1` constructor (missing-typed-dict-key) [Ln 12, Col 19]
> Missing required key 'bbb' in TypedDict `MyTypedDict2` constructor (missing-typed-dict-key) [Ln 16, Col 19]

https://play.ty.dev/a7117e4d-66e0-4c31-bac7-fafcc1e1001e

### Version

0.0.1a25

---

_Label `question` added by @sharkdp on 2025-11-06 13:49_

---

_Comment by @sharkdp on 2025-11-06 13:51_

`Optional` does not mean that the corresponding key (`bbb`) can be omitted. It means that the type of the `bbb` field can be `list[str]` or `None`. I think you are looking for [`NotRequired`](https://docs.python.org/3/library/typing.html#typing.NotRequired).

---

_Closed by @sharkdp on 2025-11-06 13:51_

---
