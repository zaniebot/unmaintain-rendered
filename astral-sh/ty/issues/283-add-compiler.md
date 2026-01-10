```yaml
number: 283
title: Add compiler
type: issue
state: open
author: nickdrozd
labels:
  - wish
assignees: []
created_at: 2025-05-08T19:05:42Z
updated_at: 2025-11-18T16:10:28Z
url: https://github.com/astral-sh/ty/issues/283
synced_at: 2026-01-10T01:58:59Z
```

# Add compiler

---

_Issue opened by @nickdrozd on 2025-05-08 19:05_

An extremely useful and underappreciated feature of Mypy is that it is able to use type information to generate C extensions. Or IOW, it acts as a Python compiler (Mypyc). The result is substantially faster than plain interpreted Python. It is a good middle ground between doing nothing and doing a full RIIR.

It would be very cool if Ty could do this. Obviously this is a long-term feature request. Good to keep in mind at this early stage.

Could try out different codegen backends, etc.

---

_Label `wish` added by @carljm on 2025-05-08 19:07_

---

_Added to milestone `Z post-stable` by @carljm on 2025-11-15 01:58_

---

_Removed from milestone `Z post-stable` by @carljm on 2025-11-18 16:10_

---
