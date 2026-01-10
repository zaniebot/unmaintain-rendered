---
number: 972
title: Support for dataclass_transform converters
type: issue
state: open
author: AdrianSosic
labels:
  - feature
  - library
assignees: []
created_at: 2025-08-12T09:20:09Z
updated_at: 2026-01-08T18:31:06Z
url: https://github.com/astral-sh/ty/issues/972
synced_at: 2026-01-10T01:51:14Z
---

# Support for dataclass_transform converters

---

_Issue opened by @AdrianSosic on 2025-08-12 09:20_

### Summary

The following code throws a false-positive:

```python
from attrs import define, field


@define
class C:
    x: float = field(converter=float)


C("1")
```

```bash
error[invalid-argument-type]: Argument is incorrect
 --> baybe/ty.py:9:3
  |
9 | C("1")
  |   ^^^ Expected `int | float`, found `Literal["1"]`
  |
info: rule `invalid-argument-type` is enabled by default
```

The issue is that attrs converters don't seem to be supported yet, which are handled via the attrs plugin in mypy. I'm aware that there is already a PR to enable dataclass support (#111) but since converters are not available via dataclasses, I think this is probably a separate issue.

### Version

_No response_

---

_Label `library` added by @carljm on 2025-08-12 14:17_

---

_Label `feature` added by @carljm on 2025-08-12 14:17_

---

_Comment by @Darkdragon84 on 2025-12-22 10:16_

there must be something in the typing standard about that, perhaps via [PEP681](https://peps.python.org/pep-0681/), because pyright supports attrs converters, and they only ever support features that are in the standard.

---

_Comment by @erictraut on 2025-12-22 16:15_

Yes, support for `converter` was added to the typing spec. See [here](https://discuss.python.org/t/discuss-adding-converter-to-dataclass-transform/55125) for the discussion.

---

_Added to milestone `Stable` by @carljm on 2025-12-22 16:28_

---

_Comment by @Darkdragon84 on 2025-12-23 14:29_

> Yes, support for `converter` was added to the typing spec. See [here](https://discuss.python.org/t/discuss-adding-converter-to-dataclass-transform/55125) for the discussion.

Ah I didn't know PEPs could be amended. TIL 👍 

---

_Renamed from "Support for attrs converters" to "Support for dataclass_transform converters" by @carljm on 2026-01-08 18:31_

---
