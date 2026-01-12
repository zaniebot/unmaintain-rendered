```yaml
number: 2320
title: try/import/except vs. explicit None
type: issue
state: closed
author: smurfix
labels: []
assignees: []
created_at: 2026-01-04T11:59:28Z
updated_at: 2026-01-04T20:11:47Z
url: https://github.com/astral-sh/ty/issues/2320
synced_at: 2026-01-12T15:54:26Z
```

# try/import/except vs. explicit None

---

_@smurfix_

### Summary

The `type:ignore` annotation below is required. I would expect it not to be.

Apparently, the explicit `|None` in the first line has been obliterated by the import for no good reason -- it's in a try/except, after all, thus it should not narrow the variable's type.

```
nt: types.ModuleType | None
try:
    import nt
except ImportError:
    nt = None  # type: ignore[assignment]
```

### Version

_No response_

---

_Comment by @carljm on 2026-01-04 20:07_

Yes, we currently consider an import to be a "declaration", not just an assignment -- and I think we should not. Thanks for the report.

---

_Comment by @carljm on 2026-01-04 20:11_

This is https://github.com/astral-sh/ty/issues/1836

---

_Closed by @carljm on 2026-01-04 20:11_

---
