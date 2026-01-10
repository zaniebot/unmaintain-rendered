---
number: 2362
title: Find all references in VS Code extension does not find call sites
type: issue
state: open
author: AdemFr
labels:
  - bug
  - server
assignees: []
created_at: 2026-01-06T10:46:07Z
updated_at: 2026-01-08T13:10:42Z
url: https://github.com/astral-sh/ty/issues/2362
synced_at: 2026-01-10T01:51:14Z
---

# Find all references in VS Code extension does not find call sites

---

_Issue opened by @AdemFr on 2026-01-06 10:46_

### Summary

Thanks for building Ty! Super excited about the project.
I set up ty using the cursor / vs code extension for the language server.

One of the critical use cases for type hints etc for me is finding / renaming all references of variables and attributes reliably.

This does not seem to work when searching for references based on the method argument across different files, but works when searching based on the call site or within the same file.

[Playground Link](https://play.ty.dev/9a3a5978-ecf8-4bd4-8a62-bd31a091bfa9)

example_rename.py
```py
from .example_rename_2 import ExampleClass

if __name__ == "__main__":
    instance = ExampleClass(old_name="test")  # Find all references for old_name works here
    result = instance.method("world")

```

example_rename_2.py
```py
class ExampleClass:
    def __init__(self, old_name: str) -> None:    # Find all references for old_name only finds usages within file
        self.old_name = old_name

    def method(self, old_name: str) -> str:
        return f"Hello {old_name}"
```


<img width="772" height="182" alt="Image" src="https://github.com/user-attachments/assets/e034292e-b13a-40c3-98e5-88bed5e02fe9" />

<img width="732" height="192" alt="Image" src="https://github.com/user-attachments/assets/e75a1cc1-e025-43d6-98cf-4c252eeb06ff" />

<img width="2554" height="366" alt="Image" src="https://github.com/user-attachments/assets/6b74021a-2b36-4e44-916c-d0b2d4df069a" />


### Expected
I would expect the same result, no matter where I search for references from. This also breaks renaming, when triggered from the class attribute, and will not rename call sites.

### Version

0.0.9

---

_Label `server` added by @AlexWaygood on 2026-01-06 11:29_

---

_Label `bug` added by @dhruvmanila on 2026-01-06 15:06_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-08 13:10_

---
