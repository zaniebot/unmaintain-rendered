```yaml
number: 893
title: "Type inference doesn't work for `self` in class instance methods"
type: issue
state: closed
author: scosman
labels: []
assignees: []
created_at: 2025-07-25T16:45:34Z
updated_at: 2025-07-25T17:26:37Z
url: https://github.com/astral-sh/ty/issues/893
synced_at: 2026-01-12T15:54:24Z
```

# Type inference doesn't work for `self` in class instance methods

---

_@scosman_

### Summary

I can't find a dupe of this, but I can't imagine you haven't already seen it. Apologies if I missed the dupe.

Type inference isn't working for `self` in class methods. Minimal example:

```
from typing_extensions import assert_type


class MyClass2:
    def class_method(self) -> bool:
        me = self
        # Fails - Inferred type of argument is `Unknown`
        assert_type(me, MyClass2)
        return True
```

Screenshot showing that language server knows the type of self is MyClass2:

<img width="442" height="188" alt="Image" src="https://github.com/user-attachments/assets/2e7d6791-4905-4d35-8f3b-b0b2327fee45" />


```
uvx ty --version
ty 0.0.1-alpha.16 (c452e53a0 2025-07-25)
```


### Version

_No response_

---

_Comment by @carljm on 2025-07-25 17:26_

Thanks for the report! This is tracked in https://github.com/astral-sh/ty/issues/159. There's a PR in progress, but it's currently blocked on the fact that it surfaces a bunch of new cycles in our inference of implicit instance attributes of classes. We're prioritizing fixing those issues in the next few weeks so we can land that PR.

---

_Closed by @carljm on 2025-07-25 17:26_

---
