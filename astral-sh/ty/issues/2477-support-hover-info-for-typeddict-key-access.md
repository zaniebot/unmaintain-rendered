```yaml
number: 2477
title: Support hover info for TypedDict key access
type: issue
state: open
author: max-kamps
labels:
  - server
assignees: []
created_at: 2026-01-13T08:52:47Z
updated_at: 2026-01-13T08:54:46Z
url: https://github.com/astral-sh/ty/issues/2477
synced_at: 2026-01-13T09:20:59Z
```

# Support hover info for TypedDict key access

---

_@max-kamps_

### Summary

Pyright supports hover info for TypedDict key accesses. For example:

```py
class Person(TypedDict):
    """A person in the database"""

    name: str
    """The person's full legal name"""

person = Person(name="Sarah Stevens")
print(person["name"])
```

Hovering over `"name"` on the last line shows the declared type and docstring for the name field:
<img width="305" height="175" alt="Image" src="https://github.com/user-attachments/assets/4e578909-49b2-4373-aaee-94d1024f88bd" />


This relates to #2189 (autocomplete for TypedDict keys)

(Thank you Alex and Micha)

### Version

0.0.11

---

_Label `server` added by @AlexWaygood on 2026-01-13 08:54_

---
