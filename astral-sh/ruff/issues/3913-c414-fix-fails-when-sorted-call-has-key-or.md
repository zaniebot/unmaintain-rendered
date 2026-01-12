```yaml
number: 3913
title: "C414 fix fails when `sorted()` call has `key` or `reverse` argument"
type: issue
state: closed
author: leos
labels:
  - bug
assignees: []
created_at: 2023-04-08T03:43:48Z
updated_at: 2023-04-09T22:40:38Z
url: https://github.com/astral-sh/ruff/issues/3913
synced_at: 2026-01-12T15:54:44Z
```

# C414 fix fails when `sorted()` call has `key` or `reverse` argument

---

_@leos_

ruff version: 0.0.261

`foo.py`:
```
sorted_by_size = sorted(list(sizes.keys()), key=lambda k: -sizes[k])
```

```
> ruff foo.py --fix
error: Failed to create fix: Expected one argument in outer function call
foo.py:2:18: C414 Unnecessary `list` call within `sorted()`
Found 1 error.
```


---

_Label `bug` added by @charliermarsh on 2023-04-08 04:00_

---

_Comment by @dhruvmanila on 2023-04-08 06:47_

There are couple more cases related to `sorted` function:

### Case 1: `sorted` with arguments inside a `set` call:
```python
set(sorted(iterable, key=lambda x: x))

# The above code gets fixed incorrectly to:
set(iterable, key=lambda x: x)
```

### Case 2: `sorted` with `sorted`

```python
sorted(sorted(iterable, key=lambda x: x))

# The arguments are copied as it is which is incorrect:
sorted(iterable, key=lambda x: x)
```

---

I think an easy fix for now would be to make this violation's `AUTOFIX` to `AutofixKind::Sometimes` and provide a fix only if there are no arguments in both the inner and outer call. Otherwise, if it's easy enough to fix all these cases, that would be preferable.


---

_Comment by @dhruvmanila on 2023-04-08 06:55_

I think everything can be fixed:

## Proposed solution

* **Author's case**: Preserve the outer call arguments
* **Case 1 & 2**: ~Do not flag this as error as the inner sorting is done using a key function. A generic way to put this would be to only remove the inner `sorted` call if no arguments are present.~ Solution for case 1 & 2 is to actually ignore the inner arguments and only copy the `iterable` to the outer call.

---

_Closed by @charliermarsh on 2023-04-09 22:33_

---

_Comment by @leos on 2023-04-09 22:40_

Thanks for the fix! The weekend isn't even over... now I see why `ruff` is as fast as it is! ⚡ 

---
