---
number: 3247
title: "FBT003: False positive with positional-only parameters"
type: issue
state: open
author: aberres
labels:
  - type-inference
assignees: []
created_at: 2023-02-27T09:00:08Z
updated_at: 2025-08-21T03:27:26Z
url: https://github.com/astral-sh/ruff/issues/3247
synced_at: 2026-01-07T13:12:14-06:00
---

# FBT003: False positive with positional-only parameters

---

_Issue opened by @aberres on 2023-02-27 09:00_

`FBT003` is triggered even if only positional arguments are allowed.

Toy example below - I stumbled upon it with a Pandas decorator:

```
with pd.option_context("mode.use_inf_as_na", True):
    ....
```

## Example

```
def foo(*args):
    pass

def foo2(a = False, *args):
    pass


foo(False) # Should not warn
# foo2(True), # Warns as expected
foo2("a", False) # Should not warn
```

## Errors

```
fbt003.py:8:5: FBT003 Boolean positional value in function call
fbt003.py:10:11: FBT003 Boolean positional value in function call
```

---

_Comment by @aberres on 2023-02-27 09:20_

A related case is this one:

```
def foo(a: bool, /):
    ...
```

The error message is correct in this case, but I'd argue that the author is doing this intentionally.

---

_Label `type-inference` added by @charliermarsh on 2023-02-27 21:49_

---

_Comment by @allisonkarlitskaya on 2023-05-30 10:32_

A case from the standard library:

https://docs.python.org/3/library/os.html#os.set_blocking

```python
def set_blocking(fd, blocking, /):
    ...
```

Pretty much no way to call that without triggering the error or using `0` instead of `False` (which is a hack).

There are also quite some functions which tend to be present on gobject-introspection bound libraries with names like like `set_*()` where the only argument to the function is the boolean flag.  Example:

 - `Gio.Action.set_enabled()`
 - `WebKit2.WebContext.set_sandbox_enabled()`
 - `WebKit2.Settings.set_enable_developer_extras()`

All of these (including the `set_blocking` case) are functions named `set_*` that take two arguments:
  - either a bound instance, or explicitly passed (ie: the fd)
  - the boolean that is being set on that thing

Seems like something which might be suitable for allowlisting?

---

_Comment by @rlipperts on 2023-06-05 13:18_

Stumbled upon this with [numpy.where](https://numpy.org/doc/stable/reference/generated/numpy.where.html#numpy-where) when just supplying single boolean values as fill values:
```python
import numpy as np
np.where(np.random.rand(3, 3) > 0.5, True, False)
```
But keyword-arguments are not allowed. A workaround is to supply the truth values as single-value arrays, but thats quite ugly:
```python
import numpy as np
np.where(np.random.rand(3, 3) > 0.5, np.array([True]), np.array([False]))
```

An even simpler (but completely useless) builtin example is the [builtin function bool](https://docs.python.org/3/library/functions.html#bool) - Converting True from bool to bool with:
```python
bool(True)
```
Technically this is not wrong but ruff will complain with FBT003. On the other hand for 
```python
bool(x=True)
```
Python will complain, because apparently pythons [C-API doesn't allow this](https://stackoverflow.com/a/24463222)
```
TypeError: bool() takes no keyword arguments
```



---

_Comment by @charliermarsh on 2023-06-05 13:19_

We should add some of these to the allowlist. There isn't much we can do right now apart from allowing these calls selectively.

---

_Comment by @allisonkarlitskaya on 2023-06-05 13:51_

I guess the rule I'd like in the list is "instance method named `set_*` with single positional boolean argument" and "(non-class) function named `set_*` with two arguments, second of which is a positional-only boolean".

> We should add some of these to the allowlist. There isn't much we can do right now apart from allowing these calls selectively.

@charliermarsh was that "... so I'll take a look at this soon" or was that "... so, patches welcome"? :)  I never wrote rust before, but no time like the present....


---

_Comment by @charliermarsh on 2023-06-05 15:52_

Haha patches are definitely welcome if anyone is interested! The code is in `rules/flake8_boolean_trap/helpers.rs` IIRC. Otherwise, I can add some of these specific methods quickly, but I wasn't going to extend the logic to handle arbitrary setters.

---

_Comment by @allisonkarlitskaya on 2023-06-05 15:54_

I already started taking a look and it's definitely going to take me a while to understand enough rust to scratch a patch together for the generic "`set_*` with two args, second arg is boolean".  Would you accept that patch?

Otherwise, I agree that at least `set_blocking` should go into the whitelist since it's in the standard library.

---

_Comment by @charliermarsh on 2023-06-05 15:57_

Yeah I would accept that patch. But also fine if you just want to add a handful of methods to the allowlist in there. (We should include `bool`, and we might as well add `option_context`, `set_enabled`, etc. There's little harm IMO.)

---

_Comment by @allisonkarlitskaya on 2023-06-05 15:59_

> Yeah I would accept that patch. But also fine if you just want to add a handful of methods to the allowlist in there. (We should include `bool`, and we might as well add `option_context`, `set_enabled`, etc. There's little harm IMO.)

Okay.  Let me see if I can even get the thing to build, first ;)

---

_Comment by @ZeeD on 2023-12-06 09:43_

FWIW the same issue is for all the various `set`* methods you can find in PySide / PyQt, for example [QTableView.setSortingEnabled](https://doc.qt.io/qtforpython-6/PySide6/QtWidgets/QTableView.html#PySide6.QtWidgets.PySide6.QtWidgets.QTableView.setSortingEnabled) or [QHeaderView.setSortIndicatorShown](https://doc.qt.io/qtforpython-6/PySide6/QtWidgets/QHeaderView.html#PySide6.QtWidgets.PySide6.QtWidgets.QHeaderView.setSortIndicatorShown) (or many others).

If you try to call them setting the flag with a keyword, you get a runtime error, like
```
TypeError: QHeaderView.setVisible() takes no keyword arguments
```

---

_Referenced in [astral-sh/ruff#9287](../../astral-sh/ruff/issues/9287.md) on 2023-12-26 21:57_

---

_Referenced in [astral-sh/ruff#8923](../../astral-sh/ruff/issues/8923.md) on 2024-01-08 03:46_

---

_Comment by @AbdealiLoKo on 2024-02-05 18:44_

Just wanted to mention, there are cases in pytest fixtures where this comes up too. For example:
```
@pytest.mark.parametrize("throw_err", (True, False))
def test_something(throw_err: bool):
    assert ...
```

In this case, the FBT rule says this `throw_err` needs to be a kwarg. Which is a bit weird as tests are never really going to be called by anyone other than pytest ?

---

_Comment by @Avasam on 2024-03-09 18:01_

Qt is also the biggest reason I can't use this rule. Signals and value setters.
![image](https://github.com/astral-sh/ruff/assets/1350584/81a5ca5e-4937-4dff-a840-78e5a234fdff)
(mostly commenting so I can track this issue)

---

_Referenced in [PlasmaPy/PlasmaPy#2648](../../PlasmaPy/PlasmaPy/pulls/2648.md) on 2024-04-25 22:02_

---

_Comment by @nineteendo on 2024-05-08 08:15_

Also here:

```python
def upper(obj: object = "", /) -> str:
    return str(obj).upper()

print(upper(True))
```

---

_Comment by @stefanks on 2024-06-28 14:36_

Also this is flagged:

```with pd.option_context("future.no_silent_downcasting", True):```

---

_Comment by @spacemanspiff2007 on 2024-10-18 05:56_

Another false positive with positional required parameters.
This is exceptionally annoying because this also triggers on [pydantic.Field](https://docs.pydantic.dev/latest/api/fields/#pydantic.fields.Field)

````python
def func(flag: bool, *, other: bool = False) -> None:
    pass


func(True, other=True)
````

````
  |
5 | func(True, other=True)
  |      ^^^^ FBT003
  |
````

---

_Comment by @henryiii on 2024-11-22 20:04_

Also, I think this might be bad enough to special case:

```python
true = typing.cast(sessions.Result, True)  # noqa: FBT003
false = typing.cast(sessions.Result, False)  # noqa: FBT003
```

I don't think it's at all unclear what True/False are in this situation.

Related, I wonder if a config option would make sense, a setting for the number of arguments where true/false become confusing. If it's the only argument, or maybe one of two (especially in cases like above), it's not confusing.

---

_Referenced in [astral-sh/ruff#11264](../../astral-sh/ruff/issues/11264.md) on 2025-03-19 05:28_

---

_Comment by @hunterhogan on 2025-08-21 03:27_

If I'm putting this in the wrong place, please tell me a good process by which I can find the right place.

```python
selectGroupAlphaCurves: numpy.ndarray[tuple[int, ...], numpy.dtype[numpy.bool_]]
...
selectGroupAlphaCurves.fill(value=False)
selectGroupZuluCurves.fill(False)  # noqa: FBT003
...
  File "C:\apps\mapFolding\mapFolding\_oeisFormulas\matrixMeanders64.py", line 112, in count64
    selectGroupAlphaCurves.fill(value=False)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
TypeError: ndarray.fill() takes no keyword arguments
```

But see this [Pull Request to update their stub](https://github.com/numpy/numpy/pull/29608) for `ndarray.fill()`.

---
