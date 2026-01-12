```yaml
number: 20727
title: defaultdict() with no factory method provided
type: issue
state: open
author: Jeremiah-England
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-06T19:24:45Z
updated_at: 2025-10-07T00:16:59Z
url: https://github.com/astral-sh/ruff/issues/20727
synced_at: 2026-01-12T15:54:57Z
```

# defaultdict() with no factory method provided

---

_@Jeremiah-England_

### Summary

I just ran into a bug where someone accidentally left off passing the factory method to a default dictionary, which apparently is perfectly valid and just makes the `defaultdict` act like a regular `dict`.

```python
from collections import defaultdict

d = defaultdict()

d["key"]
# KeyError!
```

One use case for doing this is if you want to set the default factory later dynamically (`d.default_factory = list`).

But my guess is that _most_ of the time you have an empty `defaultdict()` is it not intentional.

I think that setting the factory method dynamically down the line is pretty weird and questionable, and needing to ignore a lint rule for that is OK. And if you don't want to ignore a lint rule, you could use `defaultdict(None)` and make it explicit.

---

_Label `rule` added by @amyreese on 2025-10-07 00:16_

---

_Label `needs-decision` added by @amyreese on 2025-10-07 00:16_

---
