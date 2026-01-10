```yaml
number: 12476
title: NPY201 triggers on code written to support both numpy 2 and legacy numpy
type: issue
state: closed
author: jenshnielsen
labels:
  - rule
assignees: []
created_at: 2024-07-23T09:30:59Z
updated_at: 2024-07-24T20:03:24Z
url: https://github.com/astral-sh/ruff/issues/12476
synced_at: 2026-01-10T11:09:54Z
```

# NPY201 triggers on code written to support both numpy 2 and legacy numpy

---

_Issue opened by @jenshnielsen on 2024-07-23 09:30_

Considder an example like this which is explicitly written to support both numpy 2 and legacy numpy
```
import warnings

try:
    from numpy.exceptions import VisibleDeprecationWarning
except ImportError:
    # numpy < 1.25.0
    from numpy import VisibleDeprecationWarning


with warnings.catch_warnings():
    warnings.filterwarnings(
        "ignore",
        category=VisibleDeprecationWarning,
        message="Creating an ndarray from ragged nested sequences",
    )
```

Running `ruff check file.py` with NPY201 enabled triggers

```
error: Failed to create fix for Numpy2Deprecation: Unable to insert `VisibleDeprecationWarning` into scope due to name conflict
src\qcodes\utils\numpy_utils.py:13:18: NPY201 `np.VisibleDeprecationWarning` will be removed in NumPy 2.0. Use `numpy.exceptions.VisibleDeprecationWarning` instead.
   |
11 |     warnings.filterwarnings(
12 |         "ignore",
13 |         category=VisibleDeprecationWarning,
   |                  ^^^^^^^^^^^^^^^^^^^^^^^^^ NPY201
14 |         message="Creating an ndarray from ragged nested sequences",
15 |     )
   |
   = help: Replace with `numpy.exceptions.VisibleDeprecationWarning`
```
This is a false positive since the deprecated location is only used in legacy numpy versions that do not have a exceptions submodule.

This started happening in 0.5.1 probably as a result of #12065

Keyworkds Numpy, NPY201

Ruff 0.5.1 to 0.5.4

Config:

```
[tool.ruff.lint]
select = ["NPY201"]
```


---

_Comment by @MichaReiser on 2024-07-23 11:09_

I can see how the violation is undesired in this case, and we could probably specialize the rule to detect this very specific pattern. But I think it's also valid to use a suppression comment in this case to explicitly state that the use of the deprecated API is fine. 

@mtsokol what do you think?

---

_Label `rule` added by @MichaReiser on 2024-07-23 11:09_

---

_Comment by @AlexWaygood on 2024-07-23 12:25_

I think this pattern is likely to be quite common for projects that want to write code which is compatible with both versions of numpy. Maintaining compatibility with both versions is likely to be something that many projects are going to want to do for a while, as well.

I'd be okay with not flagging deprecated numpy imports iff:
- The deprecated import is imported in an `except ImportError` block
- The `try` block of that `except` block imports the undeprecated alternative

We should make sure all `NPY` rules are consistent about this sort of thing, whatever we do, though.

---

_Comment by @charliermarsh on 2024-07-23 13:51_

That makes sense to me. We should have code to detect the current set of handled exceptions.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-07-23 14:14_

---

_Comment by @mtsokol on 2024-07-23 17:27_

> I can see how the violation is undesired in this case, and we could probably specialize the rule to detect this very specific pattern. But I think it's also valid to use a suppression comment in this case to explicitly state that the use of the deprecated API is fine.
> 
> @mtsokol what do you think?

@MichaReiser I agree that this `try catch` pattern could be recognized by the rule, or users should add a suppression comment.

One note: Apart from `try except` pattern there's also an `if else` used downstream, like here in SciPy:
https://github.com/scipy/scipy/blob/c354a0482e08ef68bcdd2163f9b394f2c8e9a0a5/scipy/_lib/_util.py#L25

But generally `try except` should be preferred. 

---

_Comment by @jenshnielsen on 2024-07-23 17:33_

Thanks everybody for the input.

> @MichaReiser I agree that this try catch pattern could be recognized by the rule, or users should add a suppression comment.

I personally would be fine with just adding a suppression comment as long as this limitation is documented for the rule.

---

_Comment by @AlexWaygood on 2024-07-23 17:34_

I'm working on a fix as per my suggestion in https://github.com/astral-sh/ruff/issues/12476#issuecomment-2245116474.

---

_Closed by @AlexWaygood on 2024-07-24 20:03_

---
