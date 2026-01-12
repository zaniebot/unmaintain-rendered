```yaml
number: 2455
title: Numpy deprecations
type: issue
state: closed
author: sbrugman
labels:
  - rule
assignees: []
created_at: 2023-02-01T19:52:31Z
updated_at: 2023-02-14T23:45:14Z
url: https://github.com/astral-sh/ruff/issues/2455
synced_at: 2026-01-12T15:54:42Z
```

# Numpy deprecations

---

_@sbrugman_

Numpy deprecated `np.bool` and other types in [1.20](https://numpy.org/doc/stable/release/1.20.0-notes.html#using-the-aliases-of-builtin-types-like-np-int-is-deprecated), which expired in the latest release [1.24](https://numpy.org/doc/stable/release/1.24.0-notes.html#expired-deprecations).



Deprecated name | Identical to | NumPy scalar type names
-- | -- | --
numpy.bool | bool | numpy.bool_
numpy.int | int | numpy.int_ (default), numpy.int64, or numpy.int32
numpy.float | float | numpy.float64, numpy.float_, numpy.double (equivalent)
numpy.complex | complex | numpy.complex128, numpy.complex_, numpy.cdouble(equivalent)
numpy.object | object | numpy.object_
numpy.str | str | numpy.str_
numpy.long | int | numpy.int_ (C long), numpy.longlong (largest integer type)
numpy.unicode | str | numpy.unicode_


Would be great to be able to automatically flag and fix this. Seen it pass by in multiple projects:
https://github.com/apache/spark/pull/37817
https://github.com/pandas-dev/pandas/pull/39019
https://github.com/dylan-profiler/visions/pull/184
https://github.com/plotly/plotly.py/issues/4010
https://stackoverflow.com/questions/67779374/deprecationwarning-np-bool

---

_Label `rule` added by @charliermarsh on 2023-02-01 20:06_

---

_Comment by @charliermarsh on 2023-02-01 20:07_

It might be reasonable to add these as an `UP` rule, since those codes are made up anyway given that `pyupgrade` isn't actually a Flake8 plugin.

---

_Comment by @charliermarsh on 2023-02-01 20:09_

Although it might be incorrect to "market" rules as part of `pyupgrade` when they're in fact not.

---

_Comment by @ngnpope on 2023-02-01 20:18_

Use `UPG` for "upgrade"? Or `UPN` for "upgrade numpy"?

---

_Comment by @charliermarsh on 2023-02-01 20:20_

For now, if we did something like this, it's probably safest to put it in RUF or add a new category like `RX` that requires users to turn them on one-by-one.

---

_Comment by @sbrugman on 2023-02-01 20:20_

For the future effort of reorganizing rules, we might group them per dependency (if any). Now we have `pytest` and `pandas`. For now I'd opt for `flake8-numpy` or just `numpy` with `NPY`. (This allows to add all future rules related to the package there too). `RUF` is also fine of course.

---

_Comment by @charliermarsh on 2023-02-01 20:21_

`NPY` is reasonable too, yeah.

---

_Comment by @ngnpope on 2023-02-01 20:24_

`NPY` makes so much sense. Good shout.

---

_Comment by @sbrugman on 2023-02-02 15:01_

Also relevant for https://github.com/mlflow/mlflow/issues/4048

---

_Closed by @charliermarsh on 2023-02-14 23:45_

---
