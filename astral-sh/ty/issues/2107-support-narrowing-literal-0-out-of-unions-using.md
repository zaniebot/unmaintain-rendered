```yaml
number: 2107
title: "Support narrowing `Literal[0]` out of unions using `<`, `<=`, `>` and `>=` checks"
type: issue
state: open
author: The-Red-Dragons
labels:
  - wish
  - narrowing
assignees: []
created_at: 2025-12-19T11:12:25Z
updated_at: 2025-12-19T12:03:35Z
url: https://github.com/astral-sh/ty/issues/2107
synced_at: 2026-01-12T15:54:26Z
```

# Support narrowing `Literal[0]` out of unions using `<`, `<=`, `>` and `>=` checks

---

_@The-Red-Dragons_

### Summary

## Summary

ty reports `division-by-zero` errors even when the division is guarded by a conditional check like `if x > 0`.

## Environment

- ty version: 0.0.4
- Python version: 3.14
- OS: Ubuntu 24.04

## Reproduction

```python
class Example:
    def __init__(self) -> None:
        self.total_requests = 0
        self.rate_limited_requests = 0

    def get_statistics(self) -> dict[str, str]:
        return {
            "rate_limit_percentage": (
                f"{(self.rate_limited_requests / self.total_requests * 100):.2f}%"
                if self.total_requests > 0  # ← Guard is present!
                else "0%"
            ),
        }
```

## Expected Behavior

No error - the division is only executed when `self.total_requests > 0`, making division by zero impossible.

## Actual Behavior

```
error[division-by-zero]: Cannot divide object of type `Literal[0]` by zero
```

ty correctly infers that `self.total_requests` is initialized to `0`, but doesn't recognize that the `if self.total_requests > 0` guard narrows the type to exclude zero before the division occurs.

## Workaround

Currently using `division-by-zero = "warn"` in `pyproject.toml` to suppress these false positives.

## Related

This is similar to how type checkers handle None checks - after `if x is not None`, the type is narrowed. The same narrowing logic should apply to numeric comparisons before division.

### Version

0.0.4

---

_Comment by @AlexWaygood on 2025-12-19 11:24_

Hey -- yeah, our `division-by-zero` check has quite a few false positives like this. This is why it's currently disabled by default.

I'll leave this open, as it's _possible_ we might add support for narrowing `Literal[0]` out of a union by using `>`, `>=`, `<` or `<=` checks. I don't think it's a priority for us, though, as it would add some complexity to our model and I think this specific check might be the only rule that would benefit from this narrowing currently.

We should maybe also add a note to the documentation for this rule that it's known to have false positives, and that this is why it's disabled by default.

---

_Label `wish` added by @AlexWaygood on 2025-12-19 11:24_

---

_Label `narrowing` added by @AlexWaygood on 2025-12-19 11:24_

---

_Renamed from "False positive: `division-by-zero` doesn't understand conditional guards" to "Support narrowing `Literal[0]` out of unions using `<`, `<=`, `>` and `>=` checks" by @AlexWaygood on 2025-12-19 11:25_

---

_Comment by @AlexWaygood on 2025-12-19 12:03_

> We should maybe also add a note to the documentation for this rule that it's known to have false positives, and that this is why it's disabled by default.

I opened https://github.com/astral-sh/ty/issues/2112 to track this specifically

---
