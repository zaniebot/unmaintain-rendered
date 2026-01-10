```yaml
number: 598
title: "\"Too many positional arguments\" for pydantic dataclass constructor"
type: issue
state: closed
author: Ryang20718
labels:
  - needs-mre
  - dataclasses
assignees: []
created_at: 2025-06-07T00:14:03Z
updated_at: 2025-06-07T14:12:58Z
url: https://github.com/astral-sh/ty/issues/598
synced_at: 2026-01-10T02:34:10Z
```

# "Too many positional arguments" for pydantic dataclass constructor

---

_Issue opened by @Ryang20718 on 2025-06-07 00:14_

### Summary

```py
from pydantic.dataclasses import dataclass as dataclass_with_validation

@dataclass_with_validation
class BoundingBox3d:
    """Bounding box object.

    Attributes:
        x_size_m (float): Size of bounding box along x-axis.
        y_size_m (float): Size of bounding box along y-axis.
        z_size_m (float): Size of bounding box along z-axis.
    """

    x_size_m: float
    y_size_m: float
    z_size_m: float

print(BoundingBox3d(2.0, 4.0, 6.0))
```


```
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[too-many-positional-arguments]: Too many positional arguments to bound method `__init__`: expected 0, got 3
  --> /tmp/a.py:17:21
   |
15 |     z_size_m: float
16 |
17 | print(BoundingBox3d(2.0, 4.0, 6.0))
   |                     ^^^
   |
info: `too-many-positional-arguments` is enabled by default

Found 1 diagnostic
```

### Version

0.0.1a8

---

_Comment by @carljm on 2025-06-07 01:32_

Hi, thanks for the report. I'm afraid we'll need a bit more information to do anything with this bug report -- the key question is how the class `BoundingBox3d` is defined.

---

_Label `needs-info` added by @carljm on 2025-06-07 01:32_

---

_Comment by @MichaReiser on 2025-06-07 06:25_

A good indicator that our diagnostic should include more information 😅

---

_Comment by @Ryang20718 on 2025-06-07 06:46_

I'm so sorry, I think the copy - paste repro didn't go through the first time. 

added

---

_Comment by @AlexWaygood on 2025-06-07 09:00_

`pydantic.dataclasses.dataclass()` is defined [here](https://github.com/pydantic/pydantic/blob/88c75cd5adbfbfea4ff1aceb597c93f238db8dcb/pydantic/dataclasses.py#L29-L319). It uses overloads decorated with `dataclass_transform`, which is quite complicated, but which I would expect to work following https://github.com/astral-sh/ruff/pull/17835. We had a previous issue regarding `pydantic.dataclasses.dataclass` being inferred as `Never`, but that was also closed: https://github.com/astral-sh/ty/issues/307.

---

_Label `dataclasses` added by @AlexWaygood on 2025-06-07 09:00_

---

_Renamed from "Too many positional args for dataclass init" to "Too many positional args for pydantic dataclass constructor" by @AlexWaygood on 2025-06-07 09:00_

---

_Renamed from "Too many positional args for pydantic dataclass constructor" to ""Too many positional arguments" for pydantic dataclass constructor" by @AlexWaygood on 2025-06-07 10:20_

---

_Comment by @AlexWaygood on 2025-06-07 10:26_

@Ryang20718, how exactly are you running ty? I can't reproduce this check locally if I run `uv run --no-project --with=pydantic --with=ty ty check foo.py` (where `foo.py` contains the snippet you pasted above). But I _can_ reproduce if it I run `uv run --no-project --with=ty ty check foo.py`.

Do you definitely have pydantic installed into a virtual environment? If so, where is the virtual environment located? Are you invoking ty directly, or via a project manager such as uv? Are you passing the `--python` flag to ty?

---

_Label `needs-info` removed by @AlexWaygood on 2025-06-07 10:27_

---

_Label `needs-mre` added by @AlexWaygood on 2025-06-07 10:27_

---

_Comment by @Ryang20718 on 2025-06-07 14:12_

We do something of the nature and pass `ty check --extra-search-path /tmp/testing/.venv/lib/python3.12/site-packages <file>` which leads to this error. However, it works if we do 

`ty check --extra-search-path /tmp/testing/.venv/lib/python3.12/site-packages/pydantic <file>`

We do something of the nature for mypy where if we invoke mypy via python (i.e python -m mypy) the PYTHONPATH of our python interpreter propagates and I couldn't quite figure out how to do that in ty 😅. Apologies here. We use bazel so we have to wrangle a lot of tools to work with our custom workspace

---

_Closed by @Ryang20718 on 2025-06-07 14:12_

---
