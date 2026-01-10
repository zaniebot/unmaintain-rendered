```yaml
number: 2104
title: All generics syntax should be allowed in pyi files no matter the Python version
type: issue
state: closed
author: flying-sheep
labels: []
assignees: []
created_at: 2025-12-19T08:54:37Z
updated_at: 2025-12-19T09:01:05Z
url: https://github.com/astral-sh/ty/issues/2104
synced_at: 2026-01-10T01:54:00Z
```

# All generics syntax should be allowed in pyi files no matter the Python version

---

_Issue opened by @flying-sheep on 2025-12-19 08:54_

### Summary

Just like you do with UnionType syntax: #1525

Similar usability issue related to generics as #2070

Currently, this code in a `.pyi` file has both Ruff and ty complain when the minimum Python version is `>= 3.12`. But it’s of course fine as Python never tries to parse `.pyi` files.

```python
class Array[E = object]: ...
```

Should I file a separate Ruff issue or do they use the same parser anyway and fixing it in one fixes it in the other?

### Version

0.0.3

---

_Comment by @AlexWaygood on 2025-12-19 09:01_

Thanks, there's actually already an issue on the ruff side so let's continue discussion there

---

_Closed by @AlexWaygood on 2025-12-19 09:01_

---
