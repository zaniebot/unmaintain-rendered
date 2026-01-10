```yaml
number: 1076
title: "`pandas` `infer_objects` type stub missing `copy` arg"
type: issue
state: closed
author: janosh
labels: []
assignees: []
created_at: 2025-08-21T06:39:33Z
updated_at: 2025-08-21T13:11:52Z
url: https://github.com/astral-sh/ty/issues/1076
synced_at: 2026-01-10T02:06:24Z
```

# `pandas` `infer_objects` type stub missing `copy` arg

---

_Issue opened by @janosh on 2025-08-21 06:39_

### Summary

both calls to `infer_objects` on `pandas` `DataFrame` and `Series` trigger

> Argument `copy` does not match any known parameter of bound method `infer_objects` (https://ty.dev/rules#unknown-argument)

```py
import pandas as pd

df = pd.DataFrame({"A": [1, 2, 3], "B": [4, 5, 6], "C": [7, 8, 9]})

df.infer_objects(copy=False)
df.A.infer_objects(copy=False)
```

docs on the `copy` parameter:

- https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.infer_objects.html
- https://pandas.pydata.org/docs/reference/api/pandas.Series.infer_objects.html

i looked over at https://github.com/search?q=repo%3Apython%2Ftypeshed%20infer_objects&type=code but found no mention of `infer_objects` so not sure where this incorrect type stub is coming from

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-08-21 11:36_

Thanks for the report!

> i looked over at [github.com/search?q=repo%3Apython%2Ftypeshed%20infer_objects&type=code](https://github.com/search?q=repo%3Apython%2Ftypeshed%20infer_objects&type=code) but found no mention of `infer_objects` so not sure where this incorrect type stub is coming from

Typeshed only provides stubs for objects defined in the standard library. (Well, it has stubs for other libraries, but ty doesn't vendor those out of the box -- you have to `(uv) pip install` (or the Conda equivalent) those stubs packages into a Python environment.) The reason why typeshed's stubs were relevant in https://github.com/astral-sh/ty/issues/1048 was that pandas's type annotations indicated that the `Dataframe.attrs` attribute is a dictionary, so ty then looked at the stubs for the `dict` class and its methods in typeshed to figure out what the signature of `dict.update()` should be.

Here, it looks like the `infer_object()` method is directly defined on `pandas.DataFrame`, so I'd expect ty to look either in the `pandas` library or the `pandas-stubs` library for information on the signature of that method. (In general, it'll be much easier to debug issues with `pandas` if you could let us know whether you have `pandas-stubs` installed or not! If it is installed into your environment, ty will look at that stubs package for information about pandas; if not, it'll look at the `pandas` library itself for information about the classes, functions and methods in `pandas`.)

---

_Comment by @AlexWaygood on 2025-08-21 12:04_

I can reproduce the error if I have `pandas-stubs` installed but not if I only have `pandas` installed, so I'm guessing you have `pandas-stubs` installed into your environment!

---

_Comment by @janosh on 2025-08-21 12:22_

thanks a lot @AlexWaygood for troubleshooting this. you're exactly right:

```
$ pip show pandas-stubs
Using Python 3.13.0 environment at: /Users/janosh/.venv/py313
Name: pandas-stubs
Version: 2.3.0.250703
Requires: numpy, types-pytz
Required-by:
```

required by nothing so what triggered its installation initially seems to have been uninstalled or no longer depends on it.

i'll report this over at https://github.com/pandas-dev/pandas-stubs

---

_Closed by @janosh on 2025-08-21 12:22_

---

_Comment by @AlexWaygood on 2025-08-21 12:28_

> thanks a lot [@AlexWaygood](https://github.com/AlexWaygood) troubleshooting this. you're exactly right:

No problem!

I think it was harder than it should be here to figure out where ty was getting its type information from -- I'm working on a patch now to improve our diagnostics here

---

_Comment by @AlexWaygood on 2025-08-21 13:11_

See
- https://github.com/astral-sh/ruff/pull/20022

---
