---
number: 2312
title: unsupported-bool-conversion should detect __bool__ methods that return NoReturn
type: issue
state: open
author: eric-distyl-ai
labels: []
assignees: []
created_at: 2026-01-02T23:52:41Z
updated_at: 2026-01-04T21:33:42Z
url: https://github.com/astral-sh/ty/issues/2312
synced_at: 2026-01-10T01:51:14Z
---

# unsupported-bool-conversion should detect __bool__ methods that return NoReturn

---

_Issue opened by @eric-distyl-ai on 2026-01-02 23:52_

## Summary

The `unsupported-bool-conversion` rule currently only detects `__bool__ = None` (non-callable), but does not flag objects whose `__bool__` method returns `NoReturn` (unconditionally raises).

Pyright catches this under `reportGeneralTypeIssues`, and it represents a real runtime issue - calling `bool()` on such objects will raise `TypeError` (or similar).

## Example

```python
class MyClass:
    def __bool__(self):
        raise TypeError("not allowed")

x = MyClass()
if x:  # ← Should be flagged: __bool__ raises TypeError at runtime
    pass
```

**ty 0.0.8 result:** `All checks passed!`

**pyright result:**
```
error: Invalid conditional operand of type "MyClass"
  Method __bool__ for type "MyClass" returns type "NoReturn" rather than "bool" (reportGeneralTypeIssues)
```

## Real-World Impact

This affects common libraries:

### SQLAlchemy (Column.__bool__)
```python
from sqlalchemy import Column, DateTime
from sqlalchemy.orm import DeclarativeBase

class Base(DeclarativeBase):
    pass

class MyModel(Base):
    __tablename__ = "my_table"
    created_at = Column(DateTime, nullable=True)

    def to_api(self):
        # This pattern is flagged by pyright but not by ty
        return {
            "createdAt": self.created_at.isoformat() if self.created_at else None
        }
```

Note: The SQLAlchemy case is actually a false positive because the descriptor protocol returns the actual datetime at runtime, not the Column. But the check is still valuable for cases where `__bool__` truly raises.

### Pandas (Series.__bool__)
```python
import pandas as pd

series = pd.Series([1, 2, 3])
if series:  # Raises ValueError at runtime: "The truth value of a Series is ambiguous"
    pass
```

## Suggested Behavior

Extend `unsupported-bool-conversion` to also flag when:
1. `__bool__` is inferred to return `NoReturn`
2. `__bool__` unconditionally raises an exception

This would catch real bugs where objects are used in boolean context but cannot be safely converted to bool.

## Related

- [Type system feature overview #1889](https://github.com/astral-sh/ty/issues/1889) - NoReturn/Never function propagation is complete, but this specific interaction isn't covered
- The [unsupported-bool-conversion docs](https://docs.astral.sh/ty/reference/rules/#unsupported-bool-conversion) only mention `__bool__ = None`

## Environment

- ty version: 0.0.8
- Comparison: pyright 1.1.403

---

_Comment by @carljm on 2026-01-04 21:02_

Thanks for the report! It looks like pyright is the only type checker that flags this.

I think it's a bit arguable. Explicitly calling a function that returns Never/NoReturn is not a type error, so it's not clear why implicitly calling such a function as a dunder method should be.

It would make sense to at least treat an implicit dunder call that never returns in the same way as an explicit call that never returns? (That is, treat it as terminating control flow.) I don't think we do that today.

I don't think any change here is high priority, but we can keep the issue open to consider.

---

_Comment by @AlexWaygood on 2026-01-04 21:33_

I agree with @carljm here that this should not be reported by `unsupported-bool-conversion`. All `bool(x)` does at runtime is implicitly call `__bool__`, so if calling `x.__bool__()` directly would not result in a diagnostic then neither should `bool(x)`.

Annotating a function as returning `NoReturn` is not the correct way to indicate to a type checker that calling that function should always lead to a diagnostic being emitted. We don't really have a good mechanism for that right now, but there have been various proposals for this such as https://discuss.python.org/t/wrong-special-form/103832 and https://github.com/python/typing/issues/1043

---
