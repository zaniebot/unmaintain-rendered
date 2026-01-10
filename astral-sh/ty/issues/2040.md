```yaml
number: 2040
title: iter and next builtins return Unknown types
type: issue
state: closed
author: gracepetryk
labels: []
assignees: []
created_at: 2025-12-17T21:23:20Z
updated_at: 2025-12-17T21:26:59Z
url: https://github.com/astral-sh/ty/issues/2040
synced_at: 2026-01-10T01:53:59Z
```

# iter and next builtins return Unknown types

---

_Issue opened by @gracepetryk on 2025-12-17 21:23_

### Summary

The return values of the builtins `iter` and `next` resolve to `Unknown`. When calling `__iter__` on iterables or `__next__` on iterators their return values are typed correctly.

<img width="474" height="160" alt="Image" src="https://github.com/user-attachments/assets/dcd46674-ec74-4cb9-a9b8-df3bc350ca7b" />

[playground](https://play.ty.dev/49cf9c2a-7a25-4f36-a210-8c3b1a47d21c)

### Version

ty 0.0.2

---

_Comment by @carljm on 2025-12-17 21:26_

Thanks for the report! This is #1714 and will hopefully be fixed soon.

---

_Closed by @carljm on 2025-12-17 21:26_

---
