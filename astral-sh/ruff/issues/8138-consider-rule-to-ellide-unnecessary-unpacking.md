---
number: 8138
title: Consider rule to ellide unnecessary unpacking
type: issue
state: closed
author: NeilGirdhar
labels: []
assignees: []
created_at: 2023-10-23T11:15:08Z
updated_at: 2023-10-23T11:15:46Z
url: https://github.com/astral-sh/ruff/issues/8138
synced_at: 2026-01-10T01:22:47Z
---

# Consider rule to ellide unnecessary unpacking

---

_Issue opened by @NeilGirdhar on 2023-10-23 11:15_

This pattern happens a few times in major libraries:
```python
def f(**kwargs): pass

x = {'a': 2}
y = {'b': 3}
f(**{**x, **y})  # Unnecessarily prolix
f(**x, **y)  # Simpler.
```
Real life examples:
```python
dm-haiku/haiku/_src/dot.py:    return Graph(**{**self._asdict(), **kwargs})
probability/tensorflow_probability/python/distributions/inflated.py:      dist = distribution_class(**{**kwargs, **more_kwargs})
matplotlib/lib/matplotlib/patches.py:        return _cls(**{**_args, **kwargs})
matplotlib/lib/matplotlib/ticker.py:        self.set_params(**{**self.default_params, **kwargs})
```

---

_Closed by @NeilGirdhar on 2023-10-23 11:15_

---

_Comment by @NeilGirdhar on 2023-10-23 11:15_

Whoops, there is a difference when there are duplicate parameters.

---
