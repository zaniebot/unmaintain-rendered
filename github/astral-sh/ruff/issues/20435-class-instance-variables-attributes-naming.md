---
number: 20435
title: "Class' instance variables (attributes) naming"
type: issue
state: closed
author: Jtachan
labels: []
assignees: []
created_at: 2025-09-16T13:24:04Z
updated_at: 2025-09-16T16:06:06Z
url: https://github.com/astral-sh/ruff/issues/20435
synced_at: 2026-01-07T13:12:16-06:00
---

# Class' instance variables (attributes) naming

---

_Issue opened by @Jtachan on 2025-09-16 13:24_

### Summary

I can't find any rule about naming the attributes within a class. While there is no specific PEP rule about this within the index, this is documented at the PEP8 style guide: https://peps.python.org/pep-0008/#method-names-and-instance-variables

I've double checked, and right now the `N` rules do warn in the following scenario:

```python
class MyClass:
    def __init__(self):
        self.invalidPepNaming = ...
```

---

_Comment by @ntBre on 2025-09-16 16:06_

Thanks for the report! I believe this is a duplicate of #19893. Based on my comment there, I think this is consistent with the upstream pep8-naming linter, but we could still consider expanding the checks at some point.

---

_Closed by @ntBre on 2025-09-16 16:06_

---
