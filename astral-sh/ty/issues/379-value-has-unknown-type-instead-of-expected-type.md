```yaml
number: 379
title: "Value has 'Unknown' Type instead of expected type"
type: issue
state: closed
author: tonka3000
labels: []
assignees: []
created_at: 2025-05-14T09:45:53Z
updated_at: 2025-05-14T10:44:52Z
url: https://github.com/astral-sh/ty/issues/379
synced_at: 2026-01-12T15:54:23Z
```

# Value has 'Unknown' Type instead of expected type

---

_@tonka3000_

### Summary

Hi,

The following example show `Unknown` type but it should be `Doc`. The VSCode Inlay hints show the `Unknown` as well.

```py
class Doc:
    def foo(self):
        print("hello")


class StreetGenerator:
    def __init__(self, doc: Doc):
        self.doc = doc

    def generate(self):
        doc = self.doc # ty say it is "Unknown", but it should be "Doc"
        doc.foo()
```

![Image](https://github.com/user-attachments/assets/352fe823-daa0-437c-a6c8-1db9d5b4d869)

### Version

ty 0.0.1-alpha.1 (12f466e46 2025-05-13)

---

_Closed by @AlexWaygood on 2025-05-14 10:44_

---
