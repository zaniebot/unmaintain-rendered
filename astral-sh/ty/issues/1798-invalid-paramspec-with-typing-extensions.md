```yaml
number: 1798
title: "`invalid-paramspec` with `typing_extensions.ParamSpec` on Python <3.13"
type: issue
state: closed
author: jorenham
labels:
  - bug
assignees: []
created_at: 2025-12-07T20:11:51Z
updated_at: 2025-12-08T12:34:32Z
url: https://github.com/astral-sh/ty/issues/1798
synced_at: 2026-01-10T01:56:41Z
```

# `invalid-paramspec` with `typing_extensions.ParamSpec` on Python <3.13

---

_Issue opened by @jorenham on 2025-12-07 20:11_

### Summary

When `python-version` is set to anything below 3.13, this happens:

```py
from typing_extensions import ParamSpec

P = ParamSpec("P", default=...)  # picture a squiggly here
```

```
The `default` parameter of `typing.ParamSpec` was added in Python 3.13 (invalid-paramspec) [Ln 3, Col 20]
```

https://play.ty.dev/9c8e4536-7ae2-4b17-b527-24860c8f7b0d

### Version

ty 0.0.1-alpha.32

---

_Comment by @jorenham on 2025-12-07 20:13_

It doesn't seem to be a typeshed issue: https://github.com/python/typeshed/blob/5d41fd6800407489925bc57d29dd5ffe6f71ce0b/stdlib/typing_extensions.pyi#L471-L567

---

_Label `bug` added by @AlexWaygood on 2025-12-07 21:58_

---

_Added to milestone `Beta` by @AlexWaygood on 2025-12-07 21:58_

---

_Comment by @AlexWaygood on 2025-12-07 22:19_

Cc. @dhruvmanila 

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-08 05:09_

---

_Closed by @dhruvmanila on 2025-12-08 12:34_

---
