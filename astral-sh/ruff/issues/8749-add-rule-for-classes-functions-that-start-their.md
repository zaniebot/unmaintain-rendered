```yaml
number: 8749
title: Add rule for classes/functions that start their body with a blank line
type: issue
state: closed
author: ThiefMaster
labels: []
assignees: []
created_at: 2023-11-17T23:50:30Z
updated_at: 2023-11-17T23:51:01Z
url: https://github.com/astral-sh/ruff/issues/8749
synced_at: 2026-01-12T15:54:48Z
```

# Add rule for classes/functions that start their body with a blank line

---

_@ThiefMaster_

```py
class Foo:

    def bar(self):
        pass


def foo():

    pass
```

It looks like there's no rule for this at all (tested w/ `--select ALL`), even though it looks pretty weird and would even be easy to auto-fix.

There are rules for similar issues when docstrings are involved (e.g. `D201`), but nothing that does the same in undocumented code.

---

_Comment by @ThiefMaster on 2023-11-17 23:50_

If you have a suggestion where to best place such a rule I wouldn't mind looking into implementing this.

---

_Closed by @ThiefMaster on 2023-11-17 23:50_

---
