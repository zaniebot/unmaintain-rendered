```yaml
number: 17162
title: "`meta-class-abc-meta` (`FURB180`) - fix does not check whether the class already extends `ABC`"
type: issue
state: open
author: DetachHead
labels:
  - fixes
  - help wanted
assignees: []
created_at: 2025-04-03T02:51:59Z
updated_at: 2025-12-28T12:18:28Z
url: https://github.com/astral-sh/ruff/issues/17162
synced_at: 2026-01-10T11:09:58Z
```

# `meta-class-abc-meta` (`FURB180`) - fix does not check whether the class already extends `ABC`

---

_Issue opened by @DetachHead on 2025-04-03 02:51_

### Summary

before:
```py
from abc import ABC, ABCMeta

class Foo(ABC, metaclass=ABCMeta): # error: FURB180
    ...
```
after applying the fix:
```py
from abc import ABC, ABCMeta

class Foo(ABC, ABC):
    ...
```
[playground](https://play.ruff.rs/328c2b15-91ea-425b-afce-39079485cbee)

### Version

0.11.2

---

_Label `bug` added by @MichaReiser on 2025-04-03 06:56_

---

_Label `help wanted` added by @MichaReiser on 2025-04-03 06:56_

---

_Comment by @MichaReiser on 2025-04-03 06:57_

We won't be able to detect if the class indirectly inherits ABC but we can mark the fix as unsafe if there are other base classes (it might already be unsafe). We should be able to explicitly test if one of the direct base classes is ABC

---

_Label `bug` removed by @MichaReiser on 2025-04-03 06:57_

---

_Label `fixes` added by @MichaReiser on 2025-04-03 06:57_

---

_Comment by @akawd on 2025-12-28 12:18_

Suggested a fix; would appreciate a review.

---
