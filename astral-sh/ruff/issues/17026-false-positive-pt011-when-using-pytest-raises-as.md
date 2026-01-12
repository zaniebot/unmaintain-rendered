```yaml
number: 17026
title: False positive PT011 when using pytest.raises as a function
type: issue
state: closed
author: hauh
labels:
  - bug
  - good first issue
assignees: []
created_at: 2025-03-27T22:28:07Z
updated_at: 2025-04-08T07:24:48Z
url: https://github.com/astral-sh/ruff/issues/17026
synced_at: 2026-01-12T15:54:55Z
```

# False positive PT011 when using pytest.raises as a function

---

_@hauh_

### Summary

`pytest.raises` can be used not only as a context manager but also as a function that takes a function to call for specified exception. In that case `match` parameter cannot be passed to the `pytest.raises` but can be checked on the returned exception info.
```python
pytest.raises(Exception, func, *func_args, **func_kwargs).match("error message")
```
This line should not trigger PT011 error.

### Version

0.11.2

---

_Comment by @MichaReiser on 2025-03-31 10:19_

This is about the legacy form of `pytest.raises` where a user passes a matching function (see [*legacy form*](https://docs.pytest.org/en/stable/reference/reference.html#pytest-raises)). 

I think we could simply ignore the legacy form for this check (test if there's a second positional argument and then skip the check)

---

_Label `bug` added by @MichaReiser on 2025-03-31 10:19_

---

_Label `good first issue` added by @MichaReiser on 2025-03-31 10:19_

---

_Closed by @MichaReiser on 2025-04-08 07:24_

---

_Closed by @MichaReiser on 2025-04-08 07:24_

---
