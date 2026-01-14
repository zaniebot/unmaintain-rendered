```yaml
number: 2501
title: index-out-of-bounds error after numpy reshape
type: issue
state: open
author: ntjohnson1
labels: []
assignees: []
created_at: 2026-01-14T21:43:29Z
updated_at: 2026-01-14T21:43:29Z
url: https://github.com/astral-sh/ty/issues/2501
synced_at: 2026-01-14T22:42:18Z
```

# index-out-of-bounds error after numpy reshape

---

_@ntjohnson1_

### Summary

I didn't see discussion on numpy specific support or other index related edge cases.

```python
def example(data: int | Sequence[int | float]) -> None:
    if isinstance(data, Sequence):
        np_data = np.array(data).reshape((1, -1))
        if np_data.shape[1] not in (3, 4): # <---
            raise ValueError(f"expected sequence of length of 3 or 4, received {np_data.shape[1]}")
    return None
```
`error[index-out-of-bounds]: Index 0 is out of bounds for tuple `tuple[()]` with length 0`

A simpler version does seem to infer the shape correctly.
```python
  a = [1]
  b = np.array(a).reshape((1, -1))
  c = b.shape[1]
```

This triggers an index-out-of-bounds error (it thinks shape is an empty tuple so shape[0] also doesn't work). Mypy appears to infer the output shape from the reshape call.

Version `ty 0.0.11`

### Version

ty 0.0.11 (830cb9cc6 2026-01-09)

---
