---
number: 13184
title: "`pydoclint` diagnostics should target docstrings instead of the first undocumented return, yield, or exception"
type: issue
state: closed
author: tjkuson
labels:
  - docstring
  - accepted
assignees: []
created_at: 2024-08-31T17:03:31Z
updated_at: 2024-11-16T18:32:21Z
url: https://github.com/astral-sh/ruff/issues/13184
synced_at: 2026-01-07T13:12:15-06:00
---

# `pydoclint` diagnostics should target docstrings instead of the first undocumented return, yield, or exception

---

_Issue opened by @tjkuson on 2024-08-31 17:03_

Please consider changing the diagnostic target of the `pydoclint` rules in Ruff. I think the docstring itself should be the target instead.

## It is more consistent with `pydocstyle` rules

The `pydocstyle` diagnostics target the docstring. Having inconsistent targets for `pydocstyle` and `pydoclint` rules may cause user confusion and separates `noqa` directives.

For example, to suppress two docstring-related diagnostics, users would have to do something like

```python
def foo(n: int) -> int:
    """Foo a number

    Args:
        n: an integer.
    """  # noqa: D400
    ...
    return bar  # noqa: DOC201
```

## Changing the function body may force users to move `noqa` directives

Adding an early return to the example above forces the `noqa: DOC201` directive to be moved up, which is an extra burden for users and produces a noisier diff than if the directive were attached to the docstring itself.

```diff
 def foo(n: int) -> int:
     """Foo a number.

     Args:
         n: an integer.
     """  # noqa: D400
+    if baz:
+        return 0  # noqa: DOC201
     ...
-    return bar  # noqa: DOC201
+    return bar
```

## It is easier to understand

This may be subjective, but I think that using respective returns, yields, or exceptions as targets themselves is unintuitive. Per the naming convention,

> Like Clippy, Ruff's rule names should make grammatical and logical sense when read as "allow ${rule}" or "allow ${rule} items", as in the context of suppression comments.
>
> For example, AssertFalse fits this convention: it flags assert False statements, and so a suppression comment would be framed as "allow assert False".

I think the `pydoclint` diagnostics targetting their respective undocumented statement undermines this principle. For example,

```python
return bar  # "allow the docstring to not document returns"
```

would imply it is only that this specific return needn't be documented. However, suppressing the diagnostic for one return statement suppresses the diagnostic for all return statements (in other words, it suppresses the diagnostic for the entire docstring). If it operates the docstring and not any one particular statement, that it would be the most semantically logical and consistent for the diagnostic range to be the docstring.

---

_Comment by @AlexWaygood on 2024-08-31 19:04_

This seems reasonable to me, and it'll definitely be better to make this change while these rules are still in preview... @charliermarsh, what do you think?

---

_Label `docstring` added by @AlexWaygood on 2024-08-31 21:08_

---

_Comment by @MichaReiser on 2024-09-02 07:30_

It does make sense to me that the diagnostic range is the docstring. However, this points out a limitation of our diagnostic system that only allows a single code frame. I can see situations where the rule flags a docstring but finding the return/yield statement isn't trivial because it is a very long function. 

I still think we should change the range and the solution to the above problem is to extend our diagnostic system to:

* Have a violation range
* Support zero or many additional code frames that contextualize the violation. 


---

_Comment by @AlexWaygood on 2024-09-02 07:33_

> the solution to the above problem is to extend our diagnostic system to:
> 
> * Have a violation range
> * Support zero or many additional code frames that contextualize the violation.

In the meantime, the diagnostic messages for these rules could potentially mention the problematic line number in the function body that triggered the violation on the docstring

---

_Label `accepted` added by @AlexWaygood on 2024-09-02 07:33_

---

_Comment by @MichaReiser on 2024-09-02 07:34_

> In the meantime, the diagnostic messages for these rules could potentially mention the problematic line number in the function body that triggered the violation on the docstring

I would rather not do that. Computing the line number inside a lint rule is discouraged because it is fairly expensive. 

---

_Comment by @AlexWaygood on 2024-09-02 07:36_

Ah yeah, we only have direct access to the TextRange inside the rule, not the line number. Makes sense.

---

_Referenced in [astral-sh/ruff#14381](../../astral-sh/ruff/pulls/14381.md) on 2024-11-16 17:46_

---

_Closed by @charliermarsh on 2024-11-16 18:32_

---

_Closed by @charliermarsh on 2024-11-16 18:32_

---
