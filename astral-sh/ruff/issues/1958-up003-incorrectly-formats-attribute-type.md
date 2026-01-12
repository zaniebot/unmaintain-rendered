```yaml
number: 1958
title: "UP003 incorrectly formats attribute ``.type``"
type: issue
state: closed
author: nstarman
labels: []
assignees: []
created_at: 2023-01-18T15:09:37Z
updated_at: 2023-01-18T16:23:13Z
url: https://github.com/astral-sh/ruff/issues/1958
synced_at: 2026-01-12T15:54:41Z
```

# UP003 incorrectly formats attribute ``.type``

---

_@nstarman_

In Astropy, [this line](https://github.com/astropy/astropy/blob/66b4a198e92ce506c738e32faed6675b34e1c599/astropy/time/utils.py#L194)  (shown below) incorrectly trips UP003. I suspect that ruff is not checking whether ``type`` is an attribute/method.

```python
def longdouble_to_twoval(val1, val2=None):
    if val2 is None:
        val2 = val1.dtype.type(0.0)
    else:
        ...
```


---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-18 15:44_

---

_Comment by @charliermarsh on 2023-01-18 15:44_

Ah yeah, we need to check that it's a built-in. Fixing...

---

_Closed by @charliermarsh on 2023-01-18 15:53_

---

_Comment by @nstarman on 2023-01-18 16:23_

Thanks for the quick fix!

---
