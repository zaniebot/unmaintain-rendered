---
number: 8045
title: "false positive for Pandas rule `PD008` by using JAX `.at` syntax"
type: issue
state: closed
author: eringrant
labels:
  - bug
assignees: []
created_at: 2023-10-18T14:09:46Z
updated_at: 2025-07-09T18:15:31Z
url: https://github.com/astral-sh/ruff/issues/8045
synced_at: 2026-01-10T01:22:47Z
---

# false positive for Pandas rule `PD008` by using JAX `.at` syntax

---

_Issue opened by @eringrant on 2023-10-18 14:09_

The following `jax_not_pandas.py` script:
```python
from jax import numpy as jnp

x = jnp.empty((100))
x.at[:10].set(1)
```
gives
```sh
$ python -m ruff check --select PD008 --isolated jax_not_pandas.py
jax_not_pandas.py:4:1: PD008 Use `.loc` instead of `.at`. If speed is important, use NumPy.
Found 1 error.
```
even though Pandas is not in use.

---

_Label `bug` added by @charliermarsh on 2023-10-18 22:53_

---

_Comment by @MeGaGiGaGon on 2025-07-09 18:12_

This was fixed in #14671, all `PD` rules now make sure pandas is in use before triggering. https://play.ruff.rs/8a4ef5c9-5e26-4995-855f-2bb75f3ec717

---

_Closed by @charliermarsh on 2025-07-09 18:15_

---
