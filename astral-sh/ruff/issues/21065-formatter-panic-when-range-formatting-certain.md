```yaml
number: 21065
title: Formatter panic when range formatting certain clause headers with semicolons
type: issue
state: closed
author: dylwil3
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-10-24T16:25:22Z
updated_at: 2025-10-27T14:52:19Z
url: https://github.com/astral-sh/ruff/issues/21065
synced_at: 2026-01-10T11:10:00Z
```

# Formatter panic when range formatting certain clause headers with semicolons

---

_Issue opened by @dylwil3 on 2025-10-24 16:25_

### Summary

Given `ex.py` with content:

```python
x =7
try:
    a;
finally:
    b
```

We get:

```console
❯ ruff format --range=2 --diff ex.py
error: Failed to format tarmin.py: syntax error: Expected the keyword token but found another token instead.
```

This only happens with range formatting, since it comes from this call of `self.range`:

https://github.com/astral-sh/ruff/blob/eb8c0ad87c4e10e2d66ada5e4bb7f436214ca9a8/crates/ruff_python_formatter/src/statement/clause.rs#L368-L373

The `SimpleTokenizer` finding the keyword range is expecting to find `Finally` but instead it finds the (unnecessary) semi-colon.

### Version

_No response_

---

_Label `bug` added by @dylwil3 on 2025-10-24 16:25_

---

_Label `formatter` added by @dylwil3 on 2025-10-24 16:25_

---

_Assigned to @dylwil3 by @dylwil3 on 2025-10-24 16:42_

---

_Closed by @dylwil3 on 2025-10-27 14:52_

---
