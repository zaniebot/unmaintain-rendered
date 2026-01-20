```yaml
number: 2572
title: "Result of `max` with `key=len` is always `Sized`"
type: issue
state: open
author: druzsan
labels: []
assignees: []
created_at: 2026-01-20T22:22:14Z
updated_at: 2026-01-20T22:22:14Z
url: https://github.com/astral-sh/ty/issues/2572
synced_at: 2026-01-20T22:52:34Z
```

# Result of `max` with `key=len` is always `Sized`

---

_@druzsan_

### Summary

In the following code I search the longest element in a collection (I tried values of a dict and list):
```python
xs: list[list[str]] = [["foo", "bar"], ["baz", "qux", "quux"]]

x = max(xs, key=len)
reveal_type(x)
```

I expect `x` to be revealed as `list[str]`, but it is revealed as `Sized`.

Am I missing something?

Command executed:
```sh
ty check --error-on-warning
```

No other settings are applied to `ty`.

### Version

0.0.12

---
