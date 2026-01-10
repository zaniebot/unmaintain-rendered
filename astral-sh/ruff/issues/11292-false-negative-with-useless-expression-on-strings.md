```yaml
number: 11292
title: False negative with useless-expression on strings
type: issue
state: open
author: NeilGirdhar
labels:
  - bug
assignees: []
created_at: 2024-05-05T19:39:22Z
updated_at: 2024-07-02T10:30:46Z
url: https://github.com/astral-sh/ruff/issues/11292
synced_at: 2026-01-10T11:09:53Z
```

# False negative with useless-expression on strings

---

_Issue opened by @NeilGirdhar on 2024-05-05 19:39_

For some reason useless-expression is not triggered by loose strings.  Should these be caught?  If comments are intended, then Ruff could suggest converting them comments?
```python
class X:
    """Some docstring."""


def f() -> None:
    """Some docstring."""
    [123]  # noqa: B018
    "Some loose string."  # Should be B018.


x = 1
"""Some editors consider this a variable docstring, but it doesn't affect any __doc__ and
ordinary comments are ubiquitous."""
```

---

_Comment by @charliermarsh on 2024-05-05 22:34_

The intention here is to avoid flagging docstrings, but I think we need to make it more targeted.

---

_Label `bug` added by @charliermarsh on 2024-05-05 22:34_

---

_Comment by @NeilGirdhar on 2024-05-05 22:40_

Yeah, that's what I figured.  I tried to illustrate some reasonable uses of strings.  I'm not sure how you feel about the "variable docstring", which some editors recognize.  As far as I know, they're rarely used and not really a Python concept.  They look pretty weird in code:
```python
length: int  # The length of the object.
width: int  # The width of the object.
```
versus
```python
length: int
"The length of the object."
width: int
"The width of the object."
```

---

_Comment by @charliermarsh on 2024-05-05 23:09_

Yeah, I think we _do_ need to ignore attribute docstrings. They're not standardized, but they're popular enough that I think it'd be a net-negative to flag them.

---

_Comment by @dhruvmanila on 2024-05-06 06:50_

I think this can be fixed because our docstring detection is quite accurate now.

---

_Comment by @dhruvmanila on 2024-06-03 08:53_

It seems that updating `B018` seems to raise a lot of diagnostics in the ecosystem checks. What about implementing the [`PLW0105`](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/pointless-string-statement.html) rule from Pylint instead of updating `B018`? This will also match `flake8-bugbear` behavior.

---

_Comment by @NeilGirdhar on 2024-06-03 09:14_

Please don't use the same documentation as PLW0105: strings are not reasonable documentation in Python, and I don't think the linter should suggest that.

---

_Comment by @estyrke on 2024-07-02 08:56_

If B018 is to be modified, consider the special case of useless string expression after a string assignment. This is rather nasty and hard to detect for a human (the correct code should have parentheses to make this a multi line implicit string concatenation):

```python
    def __str__(self) -> str:
        string = f"AnalysisConfig(ID: {self.uuid},"
        f"BackendURL: {self.backend_url},"
        f"AnnotationTypes: {self.annotation_types})"
        return string
```

For that matter, any case with an f-string is probably always an error, since it would be pointless to use those for comments.

---

_Comment by @NeilGirdhar on 2024-07-02 10:30_

Maybe Ruff should flag "attribute docstrings" using a separate rule to convert them to annotated annotations?
```python
x: int
"blah"
```
becomes
```python
x: Annotated[int, "blah"]
```
This is more consistent with modern Python.

---
