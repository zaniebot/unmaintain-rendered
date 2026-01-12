```yaml
number: 924
title: "feature: Support D417 for __init__ Parameters in numpy class docstrings"
type: issue
state: open
author: tlambert03
labels:
  - bug
  - docstring
assignees: []
created_at: 2022-11-27T17:58:04Z
updated_at: 2024-06-06T11:16:21Z
url: https://github.com/astral-sh/ruff/issues/924
synced_at: 2026-01-12T15:54:40Z
```

# feature: Support D417 for __init__ Parameters in numpy class docstrings

---

_@tlambert03_

(I think this could technically be called a bug rather than a feature request, but ruff's current behavior is consistent with pydocstyle's, so let's call it a feature request :smile:)

**issue:**  if `__init__` parameters are documented in the class docstring, they are not caught by D417

In the [class docstrings section](https://numpydoc.readthedocs.io/en/latest/format.html#class-docstring) of the numpy docstring style guide, they state that `__init__` parameters should actually be documented in the class docstrings:

> # Class docstring
> Use the same sections as outlined above (all except [Returns](https://numpydoc.readthedocs.io/en/latest/format.html#returns) > are applicable). The constructor (`__init__`) should also be documented here, the [Parameters](https://numpydoc.readthedocs.io/en/latest/format.html#params) section of the docstring details the constructor’s parameters.

Obviously, this one is trickier:  there's a fair degree of variation out there in source code, with many still using the `__init__` docstring to document the constructor parameters. The sphinx docstring parser napoleon also supports putting the parameters in _either_ docstring, see [`ExampleClass` here](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html#example-numpy-style-python-docstrings), where they say:

        The __init__ method may be documented in either the class level
        docstring, or as a docstring on the __init__ method itself.

        Either form is acceptable, but the two should not be mixed. Choose one
        convention to document the __init__ method and be consistent with it.

Anyway, not sure how you'd like to handle this one, but thought I'd bring it to your attention.

thanks again!

-----
**edit**:  here's an example of a class that I would _like_ to throw a D417 error for a missing `b` argument:

```python
class C:
    """Summary.

    Parameters
    ----------
    a : int
        Description.
    """

    # b is not documented above
    def __init__(self, a: int, b: str) -> None:
        ...
```

---

_Comment by @charliermarsh on 2022-11-27 20:37_

Heh, I think it’s fair to call this a bug. Should fix. A little more complex of course since it requires some bookkeeping…

---

_Label `bug` added by @charliermarsh on 2022-11-27 20:37_

---

_Comment by @tlambert03 on 2022-11-27 20:39_

what's your gut feeling?  support both `__init__` and class docstrings like napoleon?  or just look in class docstrings like the spec says?

---

_Comment by @charliermarsh on 2022-11-27 20:46_

My gut is to support either, like napoleon. (Maybe biased since I tend to document the init.)

---

_Label `docstring` added by @charliermarsh on 2022-12-31 18:18_

---

_Comment by @my1e5 on 2024-06-06 11:16_

Adding to this, it would be nice to pick up missing attributes in dataclasses/attrs. 

For example

```py
@dataclass
class Foo:
    """My class.

    Attributes:
        x: The x attribute.
    """

    x: int
    y: float
    _z: bool
```
And perhaps if the attribute is prefixed with an `_` then it is optional whether that gets included in the docstring? But I'm not sure if that's a widely accepted practice.

---
